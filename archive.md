---
layout: page
---

<h2>Tags</h2>
<ul>
  {% assign sorted_tags = site.tags | sort %}
  {% for tag in sorted_tags %}
    {% assign t = tag | first %}
    {% assign posts = tag | last %}
    <li>
      <a href="/tag/{{ t | replace:' ','-'}}">
        {{t | replace:' ','-' }}
        <span>({{ posts | size }})</span>
      </a>
    </li>
  {% endfor %}
</ul>

<!--{% include comments.html %}-->
