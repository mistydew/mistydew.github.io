---
layout: default
permalink: /
---
<div class="home-left">
  <ul class="post-list">
    <li>
      <span class="post-meta"><abbr title="{{ site.posts.first.date | date_to_xmlschema }}">{{ site.posts.first.date | date: "%Y年%m月%d日 %H:%M" }}</abbr>【最新】</span>
      <span class="right">分类：<a class="category" href="{{ site.category }}#{{ site.posts.first.category }}">{{ site.posts.first.category }}</a></span>
      <h2>
        <a class="post-link" href="{{ site.posts.first.url }}">{{ site.posts.first.title | escape }}</a>
      </h2>
      {{ site.posts.first.excerpt | markdownify | strip_newlines }}
      <span><a class="readmore" href="{{ site.posts.first.url }}">阅读全文 &raquo;</a></span>
    </li>
  </ul>
</div>

<div class="home-right">
  <h2 class="page-heading">近期文章</h2>
  <ul class="post-list-more">{% for post in site.posts offset:1 limit:7 %}
    <li>
      <span><abbr title="{{ post.date | date_to_xmlschema }}">{{ post.date | date: '%Y年%m月%d日' }}</abbr></span><br>
      <a href="{{ post.url }}">{{ post.title | escape }}</a>
    </li>{% endfor %}
    <li><a class="readmore" href="{{ site.blog }}">更多内容……</a></li>
  </ul>
</div>
