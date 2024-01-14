---
layout: about
title: about
permalink: /
subtitle: 
profile:
  align: left
  image: # add suitable image
  image_circular: false # crops the image to make it circular
  address: 
news: false  # includes a list of news items
selected_papers: true # includes a list of papers marked as "selected={true}"
social: true  # includes social icons at the bottom of the page
display_categories: [research, work, teaching, fun]
horizontal: false
---

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/portfolio.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

I am an undergraduate student at the University of Edinburgh, a researcher at the [Robust Autonomy and Decisions Group](https://rad.inf.ed.ac.uk/) and Robotics Engineer at the soft robotics startup [Konpanion](konpanion.com).

My research output is currently centered around achieving more with less in the field of legged robotics. In my first research project, I developed a quadruped robot capable of rough-terrain walking using neural networks with 8-12 neurons. In my [most recent publication](https://arxiv.org/abs/2207.08765) at [IROS 2022](https://ras.papercept.net/conferences/conferences/IROS22/program/IROS22_ContentListWeb_2.html#mob-4_02), I presented a design for a four-legged robot capable of manipulation with 1 or 2 limbs while costing 5 times less than a conventional arm while minimally impacting its moment of inertia. I'm currently working on adapting this design for use on the Spot and ANYmal quadruped robots. On the industry side,  I interned at Sony's New Mobility department in Tokyo, assisting in the development of [Tachyon](https://www.sony.com/en/SonyInfo/research/technologies/new_mobility/) - a wheeled-legged hexapod robot. I have also collaborated with the Edinburgh startup [Konpanion](https://konpanion.com/) on the soft social robot Maah and an art installation robot involved in a human-robot dance performance, part of the [Edinburgh Futures Conversations](https://efi.ed.ac.uk/watch-event-recordings/) programme.

I can be reached by email at [Y.T.Tsvetkov@sms.ed.ac.uk](mailto:Y.T.Tsvetkov@sms.ed.ac.uk).

Aside from robotics, I enjoy scale modelling, animation in Blender and learning new things in general (currently: learning the Pacific Rim theme on the guitar).

<!-- pages/projects.md -->
<div class="projects">
{%- if site.enable_project_categories and page.display_categories %}
  <!-- Display categorized projects -->
  {%- for category in page.display_categories %}
  <h2 class="category">{{ category }}</h2>
  {%- assign categorized_projects = site.projects | where: "category", category -%}
  {%- assign sorted_projects = categorized_projects | sort: "importance" %}
  <!-- Generate cards for each project -->
  {% if page.horizontal -%}
  <div class="container">
    <div class="row row-cols-3">
    {%- for project in sorted_projects -%}
      {% include projects_horizontal.html %}
    {%- endfor %}
    </div>
  </div>
  {%- else -%}
  <div class="grid">
    {%- for project in sorted_projects -%}
      {% include projects.html %}
    {%- endfor %}
  </div>
  {%- endif -%}
  {% endfor %}

{%- else -%}
<!-- Display projects without categories -->
  {%- assign sorted_projects = site.projects | sort: "importance" -%}
  <!-- Generate cards for each project -->
  {% if page.horizontal -%}
  <div class="container">
    <div class="row row-cols-2">
    {%- for project in sorted_projects -%}
      {% include projects_horizontal.html %}
    {%- endfor %}
    </div>
  </div>
  {%- else -%}
  <div class="grid">
    {%- for project in sorted_projects -%}
      {% include projects.html %}
    {%- endfor %}
  </div>
  {%- endif -%}
{%- endif -%}
</div>
