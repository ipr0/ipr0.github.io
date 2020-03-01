---
layout: default
---
### _tdrgr_ - test driven red green refactor 

Some field notes on #networking #go #python #linux

<div class="posts">
  {% for post in site.posts %}
  <article class="post">

    <h1><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></h1>
  
    <div class="entry">
      {{ post.excerpt }}
    </div>

    <a href="{{ site.baseurl }}{{ post.url }}" class="read-more">Read More</a>
  </article>
  {% endfor %}
</div>

link [github](https://tlrgr.github.com)

Fowler on [#TDD](https://martinfowler.com/bliki/TestDrivenDevelopment.html)
[#pair_programming](https://martinfowler.com/articles/on-pair-programming.html)
