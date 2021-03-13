## Welcome

### Recent Posts
{% for post in site.posts limit:30 %}

[{{post.title}} {{post.date | date:"%d-%m-%Y"}}]({{site.baseurl}}{{post.url}})
{{post.description}}

{% endfor %}

