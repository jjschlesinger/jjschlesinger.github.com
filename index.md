---
layout: page
title: Yet Another Tech Blog
tagline: 
---
{% include JB/setup %}

<div>
  {% for post in site.posts %}
    <div>
      <p><a href="{{ BASE_PATH }}{{ post.url }}"><h3>{{ post.title }}</h3></a></p>
      <p>
        {{ post.content | strip_html | truncatewords: 50 }}
      </p>

    </div>
  {% endfor %}
</div>