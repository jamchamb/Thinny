---
layout: page
title: Projects
permalink: projects/
---
# Projects

{% for project in site.projects %}
   <div class="project">

   <h2>{{ project.title }}</h2>

   <table>
   <tr>
   {% if project.thumbnail %}
      <td class="projtable">
      <img alt="{{ project.title }}" src="{{ project.thumbnail }}"/>
      </td>
   {% endif %}

   <td class="projtable">
   {{ project.content }}

   {% if project.links %}
      {% for link in project.links %}
      	 <a href="{{ link.url }}">{{ link.title }}</a>
	 {% unless forloop.last %} &#8226; {% endunless %}
      {% endfor %}
   {% endif %}
   </td>
   </tr>
   </table>

   </div>
{% endfor %}

_Check out my [GitHub profile](https://github.com/jamchamb/) for more projects and open-source contributions._