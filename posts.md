---
title: "All posts"
---
{% for post in site.posts %}
**[{{ post.title }}]({{ post.url }})** (posted on {{ post.date | date: "%B %-d, %Y" }})
{{ post.excerpt }}
{% endfor %}