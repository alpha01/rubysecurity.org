---
categories:
  - cloud
  - google
tags:
  - google
  - smtp
layout: post
title: Send Email from a Shell Script Using Gmail’s SMTP
created: 1560194404
---

In my <a href="https://www.rubysecurity.org/Configuring-Postfix-to-use-Gmail" target="_blank">previous post</a>, I enabled my local mail server to relay all outgoing mail to Google's SMTP servers.  However if you want to completely bypass using any sort of MTA, then you will only need to configure your Mail User Agent client to use Gmail STMP settings directly.

In Linux, I've always used the **mailx** utility to send out email messages from the command line or from a shell script. By default, **mailx** uses the local mail server to send out messages, but configuring it to use a custom SMTP server is extremely easy.

Inside a shell script configuration would look like this:

```bash
to="root@rubyninja.org"
from="mail@rubyninja.org"
email_config="
-S smtp-use-starttls \
-S ssl-verify=ignore \
-S smtp-auth=login \
-S smtp=smtp://smtp.gmail.com:587 \
-S from=$from \
-S smtp-auth-user=mail@rubyninja.org \
-S smtp-auth-password=ULTRASECUREPASSWORDHERE \
-S ssl-verify=ignore \
-S nss-config-dir=/etc/pki/nssdb \
$to"

echo "Test email from mailx" | mail -s "TEST" $email_config
```

To have the mail settings configured to be used by **mailx** from the command line simply set your settings in `~/.mail.rc`

```bash
set smtp-use-starttls
set ssl-verify=ignore
set smtp=smtp://smtp.gmail.com:587
set smtp-auth=login
set smtp-auth-user=mail@rubyninja.org
set smtp-auth-password=ULTRASECUREPASSWORDHERE
set from="mail@rubyninja.org"
set nss-config-dir=/etc/pki/nssdb
```

### Resources

* <a href="https://www.systutorials.com/1411/sending-email-from-mailx-command-in-linux-using-gmails-smtp/" target="_blank">https://www.systutorials.com/1411/sending-email-from-mailx-command-in-linux-using-gmails-smtp/</a>
