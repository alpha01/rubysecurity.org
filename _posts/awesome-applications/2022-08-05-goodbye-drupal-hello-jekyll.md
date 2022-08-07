---
categories:
  - awesome-applications
  - jekyll
tags:
  - jekyll
  - github
layout: post
title: Goodbye Drupal, Hello Jekyll
---

Ever since the <a href="https://www.drupal.org/psa-2022-02-23" target="_blank">Drupal Project had announced support for Drupal was going to drop on November 1, 2023.</a> I've been dreading the fact that I was going to be forced to upgrade to Drupal 9. This is mainly because I'm using some modules that I know are no longer being actively developed, and I've made some changes to my theme that I know for certain will not be compatible with the new version of Drupal.

I've looked into some static site generations like, <a href="harp.js" target="_blank">Harp</a> and <a href="https://surge.sh/" target="_blank">Surge</a> in the past, I but didn't seemed to get a proper workflow with them to replace Drupal or WordPress for that matter. While I'm not new to the GitHub Pages world, I am new to it's built-in integration with <a href="https://jekyllrb.com/" target="_blank">Jekyll.</a> Recently, I had to use this feature for a work project, and I must say the <a href="https://pages.github.com/" target="_blank">GitHub Pages built-in integration is Jekyll</a> awesome! Jekyll is a fantastic piece of software. The documentation is very comprehensible and easy to follow.

So I decided to migrate this site from Drupal 7 to Jekyll. The migration process was relatively painless. The Jekyll project has tons of exporters, including one for <a href="https://import.jekyllrb.com/docs/drupal7/" target="_blank">Drupal 7.</a> The only caveat (though expected) was that the exported posts were using the content type and taxonomies set for Drupal. In Jekyll these are differently, so after few global search and replace tasks, I was able to easily transform the exported data into useable valid Jekyll content. As far as the theming is concerned, I tried to follow a similar look and view as the previous Drupal 7 site. Overall, as a person with a decent (though at times limited since JavaScript is not my forté) web development skills, customizing Jekyll has been a straight forward process. I would even say it's easier than Drupal development!

Perhaps the only drawback of the built-in integration with GitHub Pages and Jekyll is that you only have a set of <a href="https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/about-github-pages-and-jekyll#plugins" target="_blank">plugins</a> and <a href="https://pages.github.com/themes/" target="_blank">themes</a> to work with. This became evident when I was creating a custom pagination page, and GitHub Pages uses a older version of the pagination plugin that isn't compatible with newer v2 version which most examples found in when Google searching! Thus said, nothing can prevent you from using GitHub Actions to build and publish a Jekyll (or anything static site generator) generated site regardless of plugins or themes.

This site is now hosted on GitHub Pages, instead of on my homelab infrastructure.
