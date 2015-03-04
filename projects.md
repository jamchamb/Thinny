---
layout: page
title: Projects
permalink: projects/
---

# Projects

{% for project in site.projects %}
   <div class="project">

   <h2>{{ project.title }}</h2>
   
   {% if project.thumbnail %}
      <img alt="{{ project.title }}" src="{{ project.thumbnail }}"/>
   {% endif %}
   
   {{ project.content }}

   {% if project.links %}
      {% for link in project.links %}
      	 <a href="{{ link.url }}">{{ link.title }}</a>
	 {% unless forloop.last %} &#8226; {% endunless %}
      {% endfor %}
   {% endif %}

   </div>
{% endfor %}


