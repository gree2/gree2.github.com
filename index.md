---
layout: page
title: Hello World!
tagline: Coding with music.
---
{% include JB/setup %}

## Info

My QQ is 530534714.

Pay a visit to my [github](http://github.com/gree2).

## Posts

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> ==&gt; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
