---
categories:
  - awesome-applications
  - github
tags:
  - github
  - varnish
  - nginx
  - jekyll
layout: post
title: Moved antoniobaltazar.com to GitHub Pages
---

Since I recently shutdown one my Intel Nuc homelab servers due to space, in addition to going forward I'll be using public clouds for any testing that requires additional extensive computing. I was forced to migrate off my portfolio from a Kubernetes platform. I've been in a GitHub Pages honeymoon, so this was my first choice to move the site too. Since the <a href="https://github.com/alpha01/antoniobaltazar.com" target="_blank">portfolio site</a> is a simple Node app, the containerized app was already a complete static site. The only dynamic aspect of the application is the custom Gulp automation that is used to compile the Sass assets.

The only changes I made to get the site to easily publish to Git Hub Pages was restructuring the site files under the `_site` directory. By default this is the directory that gets used by the `configure-pages`, `upload-pages-artifact`, and `deploy-pages` actions. GitHub has awesome documentation, using their examples I was able to quickly write a <a href="https://github.com/alpha01/antoniobaltazar.com/blob/master/.github/workflows/npm-gulp.yml" target="_blank">Workflow</a> to build the node site, and publish it to GitHub Pages. It was a very extremely easy process! The difficult process was all my fault.

A while back, I consolidated my <a href="https://www.antoniobaltazar.com/blog/" target="_blank">Blog</a>, <a href="https://www.antoniobaltazar.com/photos/" target="_blank">Photos</a>, and <a href="https://www.antoniobaltazar.com/collection/" target="_blank">Collection</a> Wordpress sites under the domain `antoniobaltazar.com`. This presented a problem because I can't simply update DNS and point `antoniobaltazar.com` to GitHub Pages because it will break access to my other WordPress sites. I use <a href="https://www.cloudflare.com/" target="_blank">Cloudflare</a> free DNS hosting, so I have very limited access to their <a href="https://support.cloudflare.com/hc/en-us/articles/218411427" target="_blank">Rewrite Page Rules</a> features. So this meant that I would still need to keep `antoniobaltazar.com` DNS pointing to my current infrastructure, rather than having the complex redirects at the DNS level. Fortunately setting the redirects at the Varnish (http 80) and Nginx (https 443) app level was extremely easy.

### Nginx

On the Nginx side of things, I didn't had to change anything on my configuration. Nginx serves as an SSL termination proxy on my environment, so all incoming `antoniobaltazar.com` https requests are automatically forwarded to my http Varnish backend.

```perl
location / {
    proxy_pass   http://my-varnish-backend/;

    ### Set headers ####
    proxy_set_header        Accept-Encoding   "";
    proxy_set_header        Host            $host;
    proxy_set_header        X-Real-IP       $remote_addr;
    proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;

    ### Most PHP, Python, Rails, Java App can use this header ###
    #proxy_set_header X-Forwarded-Proto https;##
    #This is better##
    proxy_set_header        X-Forwarded-Proto $scheme;
    add_header              Front-End-Https   on;
    add_header              X-Powered-By "Unicorns";
    add_header              X-hacker "Alpha01";

    client_max_body_size 10m;

    # These headers are only accessible internally
    #more_clear_headers 'x-generator';
    #more_clear_headers 'x-drupal-cache';

    # We expect the downsteam servers to redirect to the right hostname, so don't do any rewrites here.
    proxy_redirect     off;
}
```

### Varnish

Its in Varnish where all the magic happens. For the configuration I'm simply redirecting all `antoniobaltazar.com` requests that don't belong to my WordPress sites, to the GitHub Pages location `https://alpha01.github.io/antoniobaltazar.com`.

```perl
sub vcl_recv {
    # Portoflio is now hosted on GitHub Pages
    if (req.http.host ~ "(?i)^(www.)?antoniobaltazar\.(com|org)" && (req.url !~ "/(blog|collection|photos)")) {
        set req.http.host = "https://alpha01.github.io";
        return (synth(750, req.http.host + "/antoniobaltazar.com" + req.url));
    }
}

sub vcl_backend_error {
    # Take care of custom redirects
    if (beresp.status == 750) {
        set beresp.http.Location = beresp.reason;
        set beresp.status = 301;
        return(deliver);
    }
} 
```

Not a pretty solution, but it does the job.
