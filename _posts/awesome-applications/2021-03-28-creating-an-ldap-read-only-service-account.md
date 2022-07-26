---
categories:
  - awesome-applications
  - 389-directoryserver
tags:
  - 389-directoryserver
  - ldap
layout: post
title: Creating an LDAP read-only service account
created: 1616908330
---

So now that I have an LDAP server up and running. I can finally start creating ldap clients to authenticate to my ldap.rubyninja.org server. Before I can start configuring applications or even adding normal LDAP users, 

1). Creating the service account
```bash
dsidm localhost user create \
--uid binduser \
--uidNumber 1001 \
--gidNumber 1001 \
--cn binduser \
--displayName binduser 
```

2). Create a password for the service account
```bash
dsidm localhost account reset_password uid=binduser,ou=people,dc=rubyninja,dc=org
```

3). To Modify/add permissions of the binduser service account. I created a file called `binduser.ldif` with the following contents:
```bash
dn: ou=people,dc=rubyninja,dc=org
changetype: modify
add: aci
aci: (targetattr="*") (version 3.0; acl "Allow uid=binduser reading to everything";
 allow (search, read) userdn = "ldap:///uid=binduser,ou=people,dc=rubyninja,dc=org";)
```

Apply the changes

```bash
ldapmodify -H ldaps://localhost -D "cn=Directory Manager" -W -x -f binduser.ldif
```

**NOTE**: A fair warning, although I've worked with LDAP and had some experience with it. Even at some point one of my job responsibilities was managing an enterprise OpenLDAP infrastructure. LDAP is not quite one of my forté, so in no way shape or form are these best practices! These is just a mere POC for my homelab.

Resources:
* <a href="https://access.redhat.com/documentation/en-us/red_hat_directory_server/10/html/administration_guide/defining_bind_rules#granting_access_to_authenticated_users" target="_blank">https://access.redhat.com/documentation/en-us/red_hat_directory_server/10/html/administration_guide/defining_bind_rules#granting_access_to_authenticated_users</a>
 
