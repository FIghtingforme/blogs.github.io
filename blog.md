---
layout: default
title: 博客列表
permalink: /blog.html
---

# 所有文章

<!-- 下面这段代码会自动循环读取 _posts 里的文件 -->
<ul>
  {% for post in site.posts %}
    <li>
      <span class="post-date">{{ post.date | date: "%Y-%m-%d" }}</span>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
