---
layout: page
permalink: /cv/
title: CV
nav: true
nav_order: 5
description:
---

<div class="text-center mb-4">
  <button id="btn-academic" class="btn btn-primary mr-2" onclick="setCV('academic')">Academic CV</button>
  <button id="btn-industry" class="btn btn-secondary" onclick="setCV('industry')">Industry Resume</button>
</div>

<div class="cv-container" style="height: 100vh; width: 100%; border: 1px solid #ccc; border-radius: 5px;">
  <iframe id="cv-iframe" src="{{ '/assets/pdf/academic_cv.pdf' | relative_url }}" style="width: 100%; height: 100%; border: none;"></iframe>
</div>

<script>
  function setCV(type) {
    const iframe = document.getElementById('cv-iframe');
    const btnAcademic = document.getElementById('btn-academic');
    const btnIndustry = document.getElementById('btn-industry');
    
    if (type === 'academic') {
      iframe.src = "{{ '/assets/pdf/academic_cv.pdf' | relative_url }}";
      btnAcademic.className = "btn btn-primary mr-2";
      btnIndustry.className = "btn btn-secondary";
    } else {
      iframe.src = "{{ '/assets/pdf/industry_resume.pdf' | relative_url }}";
      btnIndustry.className = "btn btn-primary";
      btnAcademic.className = "btn btn-secondary mr-2";
    }
  }
</script>
