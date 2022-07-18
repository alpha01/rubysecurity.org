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

### Books
<ul>
  {% for post in site.posts %}
    {% if post.categories contains "books" %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </li>
    {% endif %}
  {% endfor %}
</ul>
