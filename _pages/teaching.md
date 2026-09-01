---
layout: page
permalink: /teaching/
title: Teaching
description: Course descriptions and teaching materials.
nav: true
nav_order: 5
---

## Courses

<div style="margin-top: 20px;"></div>

&nbsp; All my courses follow an open-science approach, with non-sensitive materials published on public GitHub repositories — click through each card to access the corresponding repository.

<div style="margin-top: 20px;"></div>

<div class="projects">
  <div class="row row-cols-1 row-cols-md-2">
    {% assign courses = site.projects | where: "category", "courses" | sort: "importance" %}
    {% for project in courses %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
</div>

<div style="margin-top: 40px;"></div>

## Interactive Materials

<div style="margin-top: 20px;"></div>

&nbsp; I enjoy programming and have integrated this passion into my teaching, creating apps that simplify complex concepts and make them easier to understand. Below are the interactive apps I have developed for my courses — click through to open each one.

<div style="margin-top: 20px;"></div>

<div class="projects">
  <div class="row row-cols-1 row-cols-md-2">
    {% assign teaching_apps = site.projects | where: "category", "teaching" | sort: "importance" %}
    {% for project in teaching_apps %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
</div>

<div style="margin-top: 40px;"></div>

## Supervision

<div style="margin-top: 20px;"></div>

&nbsp; In addition to teaching, I supervise Bachelor’s and Master’s theses. To date, I have supervised (or am currently supervising) **ten** MA theses (five as main supervisor) and **nine** BA theses (all as main supervisor), all at the University of Lucerne.
