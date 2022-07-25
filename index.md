## Recent Posts
<ul>
  {% for post in site.posts %}
    {% unless post.categories contains "books" %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a> <small><em>Posted on {{ post.date | date: "%B %-d, %Y" }}</em></small>
    </li>
    {% endunless %}
  {% endfor %}
</ul>

## Recent Book Reviews
<ul>
  {% assign book_review_counter = 0 %}
  {% assign book_review_limit = 5 %}
  {% for post in site.posts %}
    {% if post.categories contains "books" %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a> <small><em>Posted on {{ post.date | date: "%B %-d, %Y" }}</em></small>
    </li>
    {% assign book_review_counter = book_review_counter | plus: 1 %}
      {% if book_review_counter == book_review_limit %}
        {% break %}
      {% endif %}
    {% endif %}
  {% endfor %}
  <small><a href="/books">All Book Reviews</a></small>
</ul>


<h1>Book Tag Cloud</h1>
<div class="blog-tags">
    {% assign tags = site.categories | sort %}
    {% for tag in tags %}
      {% unless tag contains "books" %}
    <a href="#{{ tag[0] | slugify }}" class="btn btn-default" style="font-size: {{ tag | last | size  |  times: 4 | plus: 80  }}%"> <!-- style="color: #1C1C1C;" is font color of cloud index -->
      <span class="fa fa-folder-open" aria-hidden="true">
        {{ tag[0] }} <i class="badge">{{ tag | last | size }}</i>
      </span>
    </a>
      {% endunless %}
    {% endfor %}
  </div>

## TEST

{% for post in site.posts %}
    {% unless post.categories contains "books" or post.title contains "About" %}

<small>{{ post.date | date: "%-d %B %Y" }}</small>
# <a href="{{ post.url | relative_url }}" class="blog-post">{{ post.title }}</a>

<p class="view">by {{ post.author | default: site.author }}</p>

{{ post.content }}

{% if post.tags %}
  <small>tags: <em>{{ post.tags | join: "</em> - <em>" }}</em></small>
{% endif %}
<hr>
    {% endunless %}
  {% endfor %}
