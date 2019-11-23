---
layout: page
title: Machine Learning
---
<ul>
	{%- assign sorted_post = site.portfolio | sort: "date" | reverse -%}
	{%- for post in sorted_post -%}
    {% if post.categories contains "Machine" %}
		    <li><a class="post-link" href = "{{post.url | relative_url}}">{{post.title | escape}}</a>
			     {%- if site.show_excerpt -%} {{post.excerpt}} {%- endif -%}
        </li>
    {% endif %}
	{%- endfor -%}
</ul>
