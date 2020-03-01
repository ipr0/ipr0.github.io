---
layout: default
---

<div class="posts">
  {% for post in site.posts %}
  <article class="post">
    <small>{{ page.date | date: "%-d %B %Y" }} · </small> 
    {% if page.tags %}
      <small>tags: <em>{{ page.tags | join: "</em> - <em>" }}</em></small>
    {% endif %}
    <h1><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></h1>
  
    <div class="entry">
      {{ post.excerpt }}
    </div>    
  </article>
  {% endfor %}
</div>
