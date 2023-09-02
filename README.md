# collinbarrett.github.io
A simple static GitHub Pages portfolio to replace my old WordPress site

## Blog

<ul>
    {% assign sorted_posts = site.posts | sort: 'date' | reverse %}
    {% for post in sorted_posts limit:10 %}
    <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>