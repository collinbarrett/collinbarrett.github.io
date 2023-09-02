## Blog

{% assign sorted_posts = site.posts | sort: 'date' | reverse %}
{% for post in sorted_posts limit:10 %}
* {{ post.date | date: "%Y-%m-%d" }} &#124; [{{ post.title }}]({{ post.url }})
{% endfor %}
* [View All](/blog)