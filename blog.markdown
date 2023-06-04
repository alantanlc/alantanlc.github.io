---
layout: default
categories: blog
---

{% if site.posts.size > 0 %}

<h3>Posts</h3>
<ul style="padding-left: 0;">
{% for post in site.posts %}
<li style="list-style: none; margin-bottom: .5rem;">{{post.date | date: "%B %-d, %Y" }} - <a href="{{ post.url | relative_url }}"><u>{{post.title}}</u></a></li>
{% endfor %}
</ul>
{% endif %}
