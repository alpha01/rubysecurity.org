---
categories:
  - awesome-applications
  - gitlist
tags:
  - php
  - git
  - apache
layout: post
title: 'Running my own Git server: GitList'
created: 1436765078
---

For the longest time I've been wanting to streamline updates to my sites, ie. implement good software deployment technique and procedures. To be specific, start using Git for source code management, and Jenkins to deploy. No, I'm not drinking the whole Agile Kool-Aid. After all we're in 2015, and people who still continue to use FTP/SFTP to push out changes to their sites should really need to be practicing more long term sustainable procedures. Setting up a git server is really simple. See <a href="https://dev.rubysecurity.org/awesome-applications/git/setting-up-a-git-server-in-centos-6-5" target="_blank">https://dev.rubysecurity.org/awesome-applications/git/setting-up-a-git-server-in-centos-6-5</a>.

#### Git workflow

I prefer to only communicate with Git over ssh and not https. Since I don't use the default ssh port, the initial repository clone looks like this:

```bash
git clone ssh://$GIT-USER@$GIT-SERVER:$SSH-PORT/home/git/$REPO
```

GitHub has become the defacto Git hosting provider. I think much of it's success, aside from the fact that Git is an amazing piece of software, is GitHub's polished web user interface. While Git ships with a daemon that provides a visual look at the repositories, it's definitely not pretty. I wanted to have a local GitHub like interface on my private git repos, so I decided to use <a href="http://gitlist.org/" target="_blank">GitList</a>. <a href="http://gitlist.org/" target="_blank">GitList</a> is fairly minimalistic. Requiring just PHP and mod_rewrite, it allows you to browse your repositories, view files under different revisions, commit history and diffs. Configuring GitList is really easy.

```bash
git clone https://github.com/klaussilveira/gitlist.git
cd gitlist
chmod 777 cache
mv config.ini-example config.ini
```

Then update config.ini to point to the location where the Git repositories are stored in the server. On my server, they're located in /home/git.

```bash
repositories[] = '/home/git/';
```

Lastly, is just configuring the web server's virtual host. Since I use Apache mine looks like this.

{% raw %}

```bash
<VirtualHost 192.168.1.16:443>
        ServerName git.rubyninja.org
        ServerAlias git.rubyninja.org

        DocumentRoot /var/www/gitlist

        <Directory "/var/www/gitlist">
                AllowOverride All
                AuthType Basic
                AuthName "Git Repos"
                AuthUserFile /home/svn/.htpasswd
                Require valid-user
        </Directory>;

        SSLEngine on
        SSLCertificateFile /etc/httpd/certs/svn.rubyninja.org.crt
        SSLCertificateKeyFile /etc/httpd/certs/svn.rubyninja.org.key
        SSLCACertificateFile /etc/httpd/certs/rubyninjaCA.crt

        ErrorLog logs/git_ssl_error_log
        CustomLog logs/git_ssl_request_log \
                "%t %h %{SSL_PROTOCOL}x %{SSL_CIPHER}x \"%r\" %b"
</VirtualHost>
```

{% endraw %}

### GitList

<a href="/assets/awesome-applications/git-list.png"><img src="/assets/awesome-applications/git-list.png" width="480" height="261" alt="GitList" /></a>
