---
layout: page
show_meta: false
title: "University courses"
subheadline: "Only the interesting ones"
header:
   image_fullwidth: "header_unsplash_5.jpg"
permalink: "/courses/"
---
<ul>
    {% for post in site.categories.design %}
    <li><a href="{{ site.url }}{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>
