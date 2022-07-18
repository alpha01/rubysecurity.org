## Welcome to GitHub Pages

### Posts
<ul>
  {% for post in site.posts %}
    {% unless post.categories contains "books" %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </li>
    {% endunless %}
  {% endfor %}
</ul>

### Book Reviews
<ul>
  {% for post in site.posts %}
    {% if post.categories contains "books" %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </li>
    {% endif %}
  {% endfor %}
</ul>


<h1>Book Tag Cloud</h1>
{% assign tags = site.tags | sort %}
{% for tag in tags %}
 <span class="site-tag">
    <a href="/tag/{{ tag | first | slugify }}/"
        style="font-size: {{ tag | last | size  |  times: 4 | plus: 80  }}%">
            {{ tag[0] | replace:'-', ' ' }} ({{ tag | last | size }})
    </a>
</span>
{% endfor %}

### TEST

{% for post in site.posts %}
    {% unless post.categories contains "books" or post.categories contains "header" %}

<small>{{ post.date | date: "%-d %B %Y" }}</small>
<h1>{{ post.title }}</h1>

<p class="view">by {{ post.author | default: site.author }}</p>

{{content}}

{% if post.tags %}
  <small>tags: <em>{{ post.tags | join: "</em> - <em>" }}</em></small>
{% endif %}
<hr>
    {% endunless %}
  {% endfor %}