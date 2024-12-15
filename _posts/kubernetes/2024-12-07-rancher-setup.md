---
categories:
  - kubernetes
  - rancher
tags:
  - kubernetes
  - rancher
  - cert-manager
  - letsencrypt
layout: post
title: Deploying Rancher on my Homelab
---

Finally! A new post on my technical site that I feel it's post worthy. So, I've been working with Rancher on a daily basis for a few years now. The last time I ran this awesome application on my homelab, I was using [RKE](https://rke.docs.rancher.com/) for the Kubernetes distribution. This time I'm using the new and improved [RKE2](https://docs.rke2.io/).

I wanted to have a Rancher environment on my homelab so I can experiment and play around with this awesome piece of technology. So I decided to spend some time setting up a fully functional Rancher environment on my homelab.

This is just the initial setup doc, of course I'm going to be adding more components to overly engineer my Rancher and Kubernetes homelab environment as much as possible.

### Homelab Infra

* Ubuntu Host System (Intel NUC)
* Hypervisor: KVM
* Networking: BIND DNS and ISC DHCP

### VM Host Information

* OS: Ubuntu 24.04.x
* Hostname: https://rancher.k8s.rubyninja.org/
* Kubernetes Distribution: RKE2 v1.30.5+rke2r1

### Stack

* Rancher Community (Management Plane)
* MetalLB (LB IP Addressing)
* Nginx Ingress Controller (Ingress)
* Cert-manager ACME Let's Encrypt Issuer (SSL/TLS)
* Istio (Service Mesh)
* Keycloak (Identity Provider)
* Knative (Serverles)
* Longhorn (Block Storage)
* Grafana Loki (Logs)

0). Added the following Helm chart repositories:

```bash
helm repo add rancher-stable https://releases.rancher.com/server-charts/stable
helm repo add jetstack       https://charts.jetstack.io
helm repo add ingress-nginx  https://kubernetes.github.io/ingress-nginx
helm repo add metallb        https://metallb.github.io/metallb
helm repo update
```

1). Disabled rke2-ingress-nginx add-on `/etc/rancher/rke2/config.yaml`

```yaml
disable: rke2-ingress-nginx
```

2). Install RKE2

```bash
curl -sfL https://get.rke2.io | sh -
systemctl enable rke2-server.service

export PATH=$PATH:/var/lib/rancher/rke2/bin/
export KUBECONFIG=/etc/rancher/rke2/rke2.yam
```

3). Install MetalLB

```bash
helm install metallb metallb/metallb --version 0.14.8 --create-namespace -n metallb-system
```

4). MetalLB Configuration for the Nginx Ingress Controller for Rancher

```yaml
kubectl -n metallb-system apply -f - <<EOF
apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  name: rancher
spec:
  addresses:
  - 192.168.1.71-192.168.1.73
---
apiVersion: metallb.io/v1beta1
kind: L2Advertisement
metadata:
  name: rancher
spec:
  ipAddressPools:
  - rancher
EOF
```

5). Install Nginx Ingress Controller.

```bash
helm install ingress-nginx ingress-nginx/ingress-nginx --version 4.11.3 -n ingress-system
```

6). Install cert-manager.

```bash
helm upgrade \
  cert-manager jetstack/cert-manager \
  --install \
  --create-namespace \
  --namespace cert-manager \
  --version v1.16.1 \
  --set crds.enabled=true \
  --set dns01RecursiveNameservers="8.8.8.8:53" \
  --set dns01RecursiveNameserversOnly=true \
  --set global.logLevel=4
```

I needed to use Google's Public DNS for the recursive nameserver, otherwise it will default to my local BIND DNS which cert-manger owner verification process would fail because the requesting SSL/TLS certificates for `rancher.k8s.rubyninja.org` are hosted on local homelab BIND DNS server.

7). I'm using the DNS verification process for cert-manager Let's Encrypt. So I needed to configure cert-manager to talk to Cloudflare since `rubyninja.org` public domain is hosted by them.

Create API key as outlined on the [Cloudflare Create API token](https://developers.cloudflare.com/fundamentals/api/get-started/create-token/) document, and create the Kubernetes Secret with the corresponding API key data.

```bash
kubectl create secret generic cloudflare-cred --from-literal api-key="<retracted>" -n cert-manager
```

8). Now I was able to configure my `ClusterIssuer` cert-manager resource.

```yaml
kubectl apply -f - <<EOF
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-cloudflare-prod
spec:
  acme:
    # The ACME server URL
    server: https://acme-v02.api.letsencrypt.org/directory
    # Email address used for ACME registration
    email: <RETRACTED>
    # Name of a secret used to store the ACME account private key
    privateKeySecretRef:
      name: letsencrypt-cloudflare-prod-key
    solvers:
      - dns01:
          cloudflare:
            email: <RETRACTED>
            apiKeySecretRef:
              name: cloudflare-cred
              key: api-key
        selector:
          dnsZones:
           - "rubyninja.org"
EOF
```

9). Created the `cattle-namespace` namespace for the `Certificate` cert-manager object and SSL/TLS issued certificate that will be saved as a Kubernetes secret and later be used for the Rancher installation.

```bash
kubectl create ns cattle-system
```

```yaml
kubectl -n cattle-system apply -f - <<EOF
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: rancher-k8s-rubyninja-org
spec:
  dnsNames:
  - rancher.k8s.rubyninja.org
  issuerRef:
    group: cert-manager.io
    kind: ClusterIssuer
    name: letsencrypt-cloudflare-prod
  secretName: rancher-k8s-rubyninja-org
EOF
```

Afterwards, the kubernetes.io/tls type Secret `rancher-k8s-rubyninja-org` was created on the cattle-system namespace with the corresponding certificate key pair issued by Let's Encrypt. The [Cert-manager Troubleshooting](https://cert-manager.io/docs/troubleshooting/) documentation has really straight forward explanation of the entire issuer process in case you run into problems getting an SSL/TLS certificate.

10). Finally, install Rancher.

```bash
helm upgrade --install rancher rancher-stable/rancher --version 2.9.2 \
  --namespace cattle-system \
  --set hostname=rancher.k8s.rubyninja.org \
  --set agentTLSMode="system-store" \
  --set ingress.ingressClassName=nginx \
  --set ingress.tls.source="secret" \
  --set ingress.tls.secretName="rancher-k8s-rubyninja-org" \
  --set bootstrapPassword="ChangeMe12345"
```

**NOTE:** For some reason the default bootstrap password didn't seem to work for me, so I had to manually reset it at the Pod level.

```bash
kubectl --kubeconfig $KUBECONFIG -n cattle-system exec $(kubectl --kubeconfig $KUBECONFIG -n cattle-system get pods -l app=rancher | grep '1/1' | head -1 | awk '{ print $1 }') -- reset-password
```

### Resources


* [https://developers.cloudflare.com/fundamentals/api/get-started/create-token/](https://developers.cloudflare.com/fundamentals/api/get-started/create-token/)
* [https://cert-manager.io/docs/configuration/acme/dns01/#setting-nameservers-for-dns01-self-check](https://cert-manager.io/docs/configuration/acme/dns01/#setting-nameservers-for-dns01-self-check)
* [https://cert-manager.io/docs/configuration/acme/dns01/cloudflare/](https://cert-manager.io/docs/configuration/acme/dns01/cloudflare/)
* [https://cert-manager.io/docs/troubleshooting/](https://cert-manager.io/docs/troubleshooting/)
* [https://medium.com/nerd-for-tech/ensure-having-a-rancher-admin-4ee62cd066e1](https://medium.com/nerd-for-tech/ensure-having-a-rancher-admin-4ee62cd066e1)
