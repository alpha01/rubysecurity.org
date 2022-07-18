---
categories:
- awesome-applications
- ansible
- bind
layout: post
tags: ansible
title: Updating BIND DNS records using Ansible
created: 1617169192
---
This is a follow up to the post. <a href="https://www.rubysecurity.org/Configure-BIND-to-support-DDNS-updates" target="_blank">Configure BIND to support DDNS updates
</a> Now, that I'm able to dynamically update DNS records, this is where Ansible comes in. Ansible is hands down my favorite orchestration/automation tool. So I choose to use it to update my local DNS records going forward.

I'll be using the <a href="https://docs.ansible.com/ansible/latest/collections/community/general/nsupdate_module.html" target="_blank">community.general.nsupdate</a> module.

I constructed my DNS records on my nameserver's corresponding Ansible group_vars using the following structure:

```yaml
all_dns_records:
  - zone: DNS-NAME
    records:
      - record: (@ for $ORIGIN or normal record name )
        ttl: TTL-VALUE
        state: (present or absent)
        type: DNS-TYPE
        value: VALUE-OF-DNS-RECORD
```

Example

```yaml
---
all_dns_records:
  - zone: "rubyninja.org"
    records:
      - record: "@"
        ttl: "10800"
        state: "present"
        type: "A"
        value: "192.168.1.63"
      - record: "shit"
        ttl: "10800"
        state: "present"
        type: "A"
        value: "192.168.1.64"
  - zone: "alpha.org"
    records:
      - record: "@"
        ttl: "10800"
        state: "present"
        type: "A"
        value: "192.168.1.63"
      - record: "test"
        ttl: "10800"
        state: "present"
        type: "A"
        value: "192.168.1.64"
[...]
```

Deployment Ansible playbook:

{% raw %}
```yaml
---
- hosts: ns1.rubyninja.org
  pre_tasks:
    - name: Get algorithm from vault
      ansible.builtin.set_fact:
        vault_algorithm: "{{ lookup('community.general.hashi_vault', 'secret/systems/bind:algorithm') }}"
      delegate_to: localhost

    - name: Get rndckey from vault
      ansible.builtin.set_fact:
        vault_rndckey: "{{ lookup('community.general.hashi_vault', 'secret/systems/bind:rndckey') }}"
      delegate_to: localhost

  tasks:
    - name: Sync $ORIGIN records"
      community.general.nsupdate:
        key_name: "rndckey"
        key_secret: "{{ vault_rndckey }}"
        key_algorithm: "{{ vault_algorithm }}"
        server: "ns1.rubyninja.org"
        port: "53"
        protocol: "tcp"
        ttl: "{{ item.1.ttl }}"
        record: "{{ item.0.zone }}."
        state: "{{ item.1.state }}"
        type: "{{ item.1.type }}"
        value: "{{ item.1.value }}"
      when: item.1.record == "@"
      with_subelements:
        - "{{ all_dns_records }}"
        - records
      notify: Sync zone files
      delegate_to: localhost

    - name: Sync DNS records"
      community.general.nsupdate:
        key_name: "rndckey"
        key_secret: "{{ vault_rndckey }}"
        key_algorithm: "{{ vault_algorithm }}"
        server: "ns1.rubyninja.org"
        port: "53"
        protocol: "tcp"
        zone: "{{ item.0.zone }}"
        ttl: "{{ item.1.ttl }}"
        record: "{{ item.1.record }}"
        state: "{{ item.1.state }}"
        type: "{{ item.1.type }}"
        value: "{{ item.1.value }}"
      when: item.1.record != "@"
      with_subelements:
        - "{{ all_dns_records }}"
        - records
      notify: Sync zone files
      delegate_to: localhost

  post_tasks:
    - name: Check master config
      command: named-checkconf /var/named/chroot/etc/named.conf
      delegate_to: ns1.rubyninja.org
      changed_when: false

    - name: Check zone config
      command: "named-checkzone {{ item }} /var/named/chroot/etc/zones/db.{{ item }}"
      with_items:
        - "{{ all_dns_records | map(attribute='zone') | list }}"
      delegate_to: ns1.rubyninja.org
      changed_when: false

  handlers:
    - name: Sync zone files
      command: rndc -c /var/named/chroot/etc/rndc.conf sync -clean
      delegate_to: ns1.rubyninja.org
```
{% endraw %}

My DNS deployment a playbook breakdown:

1. Grabs the Dynamic DNS update keys from HashiCorp Vault
2. Syncs all of @ $ORIGIN records for all zone.
3. Syncs all of the records.
4. For good measure, but not necessary: Checks named.conf file
5. For good measure, but not necessary: Checks each individual zone file
6. Force dynamic changes to be applied to disk.

Given that in my environment I have roughly a couple of dozen DNS records, the structured for DNS records works in my environment. Thus said, my group_vars file with all my DNS records is almost 600 lines long. The playbook executing run takes around 1-2 minutes to complete. If I were to be in an environment where I had thousands of DNS records, the approached that I described here might not be the most efficient.
