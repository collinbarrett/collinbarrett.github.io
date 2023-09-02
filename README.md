# collinbarrett.github.io
A simple static GitHub Pages portfolio to replace my old WordPress site

## Blog

{% assign sorted_posts = site.posts | sort: 'date' | reverse %}
{% for post in sorted_posts limit:10 %}
* {{ post.date | date: "%Y-%m-%d" }} | [{{ post.title }}]({{ post.url }})
{% endfor %}
* [View All](/blog)