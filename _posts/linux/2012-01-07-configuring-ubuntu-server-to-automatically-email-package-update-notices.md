---
categories:
- ubuntu
layout: article
title: Configuring Ubuntu server to automatically email package update notices
created: 1325912536
---
Apticron will give you the ability to automatically email  information about any packages on a Ubuntu system that needs to be updated.

Installing it and configuring it, is dead simply that even my six year old nephew can do it.
<blockquote>
sudo apt-get install apticron
</blockquote>
Now simply update the /etc/apticron/apticron.conf config with your email address. By default the cron entry gets added to run every day, /etc/cron.daily/apticron .

Unlike Red Hat's yum-updatesd utility, the apticron tool also includes a summary information about the package's update changes.
