---
categories:
  - linux
  - ubuntu
tags: ubuntu
layout: post
title: Black background in all desktops after Ubuntu 13.10 upgrade
created: 1382840602
---

So I just upgraded my Dell XPS 13 laptop from Ubuntu 13.04 to 13.10, and immediately the first thing I noticed that all of my desktops had a black background. and manually changing the background wallpaper took no effect. Turns out this is a common problem. In my case it turned out to be related to Gnome, which I found it to be rather interesting giving that a Gnome specific setting will cause this problem in Unity. 

### Fix

```bash
gsettings set org.gnome.settings-daemon.plugins.background active true
```

Reference:

* <a href="http://askubuntu.com/questions/287571/desktop-shows-a-white-or-black-background-instead-of-wallpapers" target="_blank">http://askubuntu.com/questions/287571/desktop-shows-a-white-or-black-background-instead-of-wallpapers</a>
