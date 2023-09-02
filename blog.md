---
layout: default
title: Blog
redirect_from: /blog/
---

# Blog

{% assign sorted_posts = site.posts | sort: 'date' | reverse %}
{% for post in sorted_posts %}
* {{ post.date | date: "%Y-%m-%d" }} &#124; [{{ post.title }}]({{ post.url }})
{% endfor %}