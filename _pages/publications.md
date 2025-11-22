---
layout: page
permalink: /publications/
title: publications
description:
nav: true
nav_order: 2
---

<!-- _pages/publications.md -->

<div class="publications">

<h2>Manuscripts</h2>

{% bibliography --query @*[category=manuscript]* %}

<h2>Published Work</h2>

{% bibliography --query @*[category=published]* %}

</div>
