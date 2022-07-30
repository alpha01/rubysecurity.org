---
categories:
  - awesome-applications
  - nginx
tags:
  - centos
  - nginx
layout: post
title: Compiling Nginx dynamically loadable modules
created: 1479457821
---

I’ve compiled plenty of dynamic loadable modules for Apache, but never for Nginx up until now. I must say compiling modules for Apache is a much easier process compared to Nginx. 

I use Nginx as a SSL reverse proxy/terminator. I wanted to be able to remove certain http headers at the Nginx level rather than on my Apache backend. The way to achieve this is using a module called <a href="https://www.nginx.com/resources/wiki/modules/headers_more/" target="_blank">headers more</a>, which is not part of core Nginx.

### My Setup

I’m using the nginx package installed via by the <a href="https://fedoraproject.org/wiki/EPEL" target="_blank" >epel repository</a> for CentOS 6.x. So compiling the module was a bit tricky initially.

### Compilation Process

1). Download the source code for the module, from <a href="https://github.com/openresty/headers-more-nginx-module/tags" target="_blank">https://github.com/openresty/headers-more-nginx-module/tags</a>
 
2). You need to find out the running version of Nginx:

```bash
$ nginx -v
nginx version: nginx/1.10.1
```

3). Download and extract the source code for that version of Nginx:

```bash
$ wget 'http://nginx.org/download/nginx-1.10.1.tar.gz'
```

4). Compilation: On my first initial compilation, I mistakenly compiled the module using the following:

```bash
./configure --prefix=/opt/nginx \
     --add-dynamic-module=/opt/headers-more-nginx-module/headers-more-nginx-module-0.32
```

Even though the module compiled successfully, when trying to load the module, it gave me the following error:

```bash
$ nginx -t
nginx: [emerg] module "/usr/lib64/nginx/modules/ngx_http_headers_more_filter_module.so" is not binary compatible in /etc/nginx/nginx.conf:9
nginx: configuration file /etc/nginx/nginx.conf test failed
```

It turns out the module has to be compiled using the same ./configure flags as my running version of Nginx, in addition to the `--add-dynamic-module=/opt/headers-more-nginx-module/headers-more-nginx-module-0.32` flag. Luckily, this can easily been obtained by running the following:

```bash
$  nginx -V
nginx version: nginx/1.10.1
built by gcc 4.4.7 20120313 (Red Hat 4.4.7-17) (GCC)
built with OpenSSL 1.0.1e-fips 11 Feb 2013
TLS SNI support enabled
configure arguments: --prefix=/usr/share/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib64/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --http-client-body-temp-path=/var/lib/nginx/tmp/client_body --http-proxy-temp-path=/var/lib/nginx/tmp/proxy --http-fastcgi-temp-path=/var/lib/nginx/tmp/fastcgi --http-uwsgi-temp-path=/var/lib/nginx/tmp/uwsgi --http-scgi-temp-path=/var/lib/nginx/tmp/scgi --pid-path=/var/run/nginx.pid --lock-path=/var/lock/subsys/nginx --user=nginx --group=nginx --with-file-aio --with-ipv6 --with-http_ssl_module --with-http_v2_module --with-http_realip_module --with-http_addition_module --with-http_xslt_module=dynamic --with-http_image_filter_module=dynamic --with-http_geoip_module=dynamic --with-http_sub_module --with-http_dav_module --with-http_flv_module --with-http_mp4_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_random_index_module --with-http_secure_link_module --with-http_degradation_module --with-http_slice_module --with-http_stub_status_module --with-http_perl_module=dynamic --with-mail=dynamic --with-mail_ssl_module --with-pcre --with-pcre-jit --with-stream=dynamic --with-stream_ssl_module --with-debug --with-cc-opt='-O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector --param=ssp-buffer-size=4 -m64 -mtune=generic' --with-ld-opt=' -Wl,-E'
```

This is the tricky and annoying part. Since, the <a href="https://fedoraproject.org/wiki/EPEL" target="_blank" >epel repo</a>  packaged binary of nginx was compiled with support of additional modules, the identical manual compilation that I was trying to do required some dependencies to be installed. In my case I just had to installed the following dependencies:

```bash
yum install libxslt-devel perl-ExtUtils-Embed GeoIP-devel
```

After compilation completes. Then it's just a matter of adding the module to the nginx.conf config. In my case, I manually copied over the compiled module to /usr/lib64/nginx/modules, so my config looks like this:

```bash
load_module "/usr/lib64/nginx/modules/ngx_http_headers_more_filter_module.so";
```

It is worth mentioning that Nginx is very picky on the order in which its configuration is set. For example. `load_module` option has be set before the `events` configuration block like this:

```nginx
load_module "/usr/lib64/nginx/modules/ngx_http_headers_more_filter_module.so";

events {
    worker_connections  1024;
}
```

Otherwise, you'll get the following error when trying to load the module.

```bash
nginx -t
nginx: [emerg] "load_module" directive is specified too late in /etc/nginx/nginx.conf:14
nginx: configuration file /etc/nginx/nginx.conf test failed
```

### Important

One really important thing to keep in mind, is that you'll need to recompile the module each time Nginx gets updated!
