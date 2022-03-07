---
layout: page
show_meta: false
title: "All the links"
subheadline: "Only the interesting ones"
header:
   image_fullwidth: "header_unsplash_5.jpg"
permalink: "/links/"
---
<ul>
    {% for post in site.categories.courses %}
    <li><a href="{{ site.url }}{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>
