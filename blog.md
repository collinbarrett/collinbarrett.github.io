---
layout: page
title: Blog
permalink: /blog
---

{% assign sorted_posts = site.posts | sort: 'date' | reverse %}
{% for post in sorted_posts %}
* {{ post.date | date: "%m.%d.%y" }} &#124; [{{ post.title }}]({{ post.url }})
{% endfor %}