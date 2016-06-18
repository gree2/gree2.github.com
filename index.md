---
layout: page
title: Hello World!
tagline: Coding on shoulders of Giants with music.
---
{% include JB/setup %}

## Info

<del>my qq is `530534714`.</del>

my wechat is `hqlgree2`

my email is `hqlgree2` at `gmail.com`

pay a visit to my [github](http://github.com/gree2).

## Posts

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date: "%Y-%m-%d" }}</span> ==&gt; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
