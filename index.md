
{% for post in site.posts limit:1 %}
{{ post.content }}
{{ break }}
{% endfor %}

<p align="right"><font size="-1"><a href="posts">See older posts</a></font></p>
