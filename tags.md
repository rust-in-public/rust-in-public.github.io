---
layout: default
title: Posts by Tag
permalink: /tags/
---
<!--
Posts tagged 
{% for tag in site.tags %}
  {% assign t = tag | first %}
  {% assign posts = tag | last %}
{{ t | downcase }}
<ul>
{% for post in posts %}
  {% if post.tags contains t %}
  <li>
    <a href="{{ post.url }}">{{ post.title }}</a>
    <span class="date">{{ post.date | date: "%B %-d, %Y"  }}</span>
  </li>
  {% endif %}
{% endfor %}
</ul>
{% endfor %}
-->


{% for category in page.tags %}
  {% assign moreThanOneInCategory = false %}
  {% assign posts = site.tags[category] %}

  {% for post in posts %}
    {% if forloop.length > 1 %}
      {% assign moreThanOneInCategory = true %}
    {% endif %}
  {% endfor %}

  {% if moreThanOneInCategory == true %}
    <div class="related-posts">
      <h3>Other posts archived in “{{ category }}”</h3>

      <ul>
        {% for post in posts %}
          {% unless post.url == page.url %}
            <li>
              <a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>

              Published on ({ post.date | date: "%B %-d, %Y" }}
            </li>
          {% endunless %}
        {% endfor %}
      </ul>
    </div>
  {% endif %}
{% endfor %}