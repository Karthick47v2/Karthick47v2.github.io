---
layout: page
permalink: /publications/
title: publications
description:
nav: true
nav_order: 2
---

<div class="publications">

{% bibliography --query @*[other!=true]* %}

<h2 class="bibliography">Other</h2>

{% bibliography --group_by none --query @*[other=true]* %}

</div>
