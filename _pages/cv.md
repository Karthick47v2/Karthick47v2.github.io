---
layout: page
permalink: /cv/
title: CV
nav: true
nav_order: 5
description:
---

<div class="d-flex justify-content-end mb-4">
  <a id="btn-download" href="{{ '/assets/pdf/industry_resume.pdf' | relative_url }}" class="btn btn-outline-primary" target="_blank" download>
    <i class="fas fa-file-download mr-1"></i> Download Resume
  </a>
</div>

<div class="cv-container" style="height: 100vh; width: 100%; border: 1px solid var(--global-divider-color, #ccc); border-radius: 5px; overflow: hidden;">
  <iframe id="cv-iframe" src="{{ '/assets/pdf/industry_resume.pdf' | relative_url }}#toolbar=0" style="width: 100%; height: 100%; border: none;"></iframe>
</div>
