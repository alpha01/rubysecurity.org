---
categories:
  - awesome-applications
  - postfix
tags:
  - postfix
  - google
layout: post
title: Configuring Postfix to use Gmail
created: 1558771842
---

Configuring Postfix to use Gmail as the outgoing SMTP relay endpoint is a relatively simple process. I’m my case, I’m not using an @gmail.com account. Rather, since all of my domains use G Suite, I’ve created a special dedicated email account that I’ll be using to send out email from.

Before starting configuring Postfix, it is important that you enable <a href="https://support.google.com/accounts/answer/6010255" target="_blank">"Less secure app access"</a>  on the Gmail account that you will be configuring to send outgoing messages.

I’m using CentOS 7.x as my mail server OS. These were the steps I used to configure Postfix.

1). Install necessary packages:

```bash
yum install postfix mailx cyrus-sasl cyrus-sasl-plain
```

2). Create `/etc/postfix/sasl_passwd` file with the your authentication credentials:

```bash
[smtp.gmail.com]:587    mail@rubyninja.org:mypassword
```

3). Update file permissions to lockdown access to our newly created authentication config file:

```bash
chmod 600 /etc/postfix/sasl_passwd
```

4). Use the `postmap` command to compile and hash the contents of sasl_passwd:

```bash
postmap /etc/postfix/sasl_passwd
```

5). Update `/etc/postfix/main.cf`

```bash
relayhost = [smtp.gmail.com]:587
smtp_use_tls = yes
smtp_sasl_auth_enable = yes
smtp_sasl_security_options =
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
smtp_tls_CAfile = /etc/ssl/certs/ca-bundle.crt
```

6). Finally, enable and restart postfix:

```bash
systemctl enable postfix
systemctl restart postfix
```

Lastly, although it’s not needed to get a working Postfix to Gmail STMP config working. I would recommend enabling outgoing throttling. Otherwise Google might temporarily suspend your account from sending messages!

Additional `/etc/postfix/main.cf` updates:

```bash
smtp_destination_concurrency_limit = 2
smtp_destination_rate_delay = 10s
smtp_extra_recipient_limit = 5
```

In my case, I configured Postfix to only handle two concurrent relay connections, wait at least 10 seconds to send out the email and set the recipient limit to 5 (per queue message session).

**NOTE:** As I mentioned, since I'm not using an @gmail.com, I had to add an <a href="https://support.google.com/a/answer/33786?hl=en" target="_blank">SPF DNS record</a> so that the outgoing emails pass all of Google’s spam tests.

DNS txt record:

```bash
v=spf1 include:_spf.google.com ~all
```

Example received email header that was sent from the newly Postfix to Gmail smtp configuration:

<img src="/assets/awesome-applications/email-header.png" alt="Passing Gmail Email Header">

To conclude, it is import to remember that this Postfix configuration will overwrite whatever `From` source set by your mail user agent (as the above email header image demonstrates).

### Resources

* <a href="https://www.howtoforge.com/tutorial/configure-postfix-to-use-gmail-as-a-mail-relay" target="_blank">https://www.howtoforge.com/tutorial/configure-postfix-to-use-gmail-as-a-mail-relay</a>
* <a href="https://wiki.deimos.fr/Postfix:_limit_outgoing_mail_throttling.html" target="_blank">https://wiki.deimos.fr/Postfix:_limit_outgoing_mail_throttling.html</a>
