---
layout: default
---

### @atsfour

主にscalaでアドテク系開発をやっています。

### 最近の記事

<ul class="posts">
  {% for post in site.posts %}
    <li>
      <span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a>
      <div>{{post.excerpt}}</div>
    </li>
  {% endfor %}
</ul>
