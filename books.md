---
layout: default
title: Books
permalink: /books/
categories: header
---

## Book Reviews

<!-- markdownlint-disable -->
{% for post in site.categories.books %}
</li><a class="post-link" href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
<!-- markdownlint-enable -->

<hr>
