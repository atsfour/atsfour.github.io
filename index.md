---
layout: default
---

### @atsfour


<ul>
{% for post in site.posts %}
  <li>
    <a href="{{ post.url }}">{{ post.date | date_to_long_string }} : {{ post.title }}</a>
    {{ post.excerpt}}
  </li>
{% endfor %}
</ul>
