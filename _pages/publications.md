---
layout: page
permalink: /publications/
title: Publications
description: Peer-reviewed publications and other research outputs.
nav: true
nav_order: 1
---

<div class="publications" markdown="1">

## Peer-Reviewed Publications

<div style="margin-top: 20px;"></div>

{% bibliography -f papers -q @article %}

<div style="margin-top: 40px;"></div>

## Other Publications

<div style="margin-top: 20px;"></div>

{% bibliography -f papers -q @misc,@phdthesis %}

</div>
