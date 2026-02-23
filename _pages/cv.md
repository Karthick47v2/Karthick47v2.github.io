---
layout: page
permalink: /cv/
title: CV
nav: true
nav_order: 5
description:
---

<style>
  .btn-theme-active {
    background-color: var(--global-theme-color);
    border-color: var(--global-theme-color);
    color: var(--global-bg-color);
  }
  .btn-theme-active:hover {
    background-color: var(--global-theme-color);
    border-color: var(--global-theme-color);
    color: var(--global-bg-color);
    opacity: 0.9;
  }
  .btn-theme-inactive {
    background-color: transparent;
    border-color: var(--global-theme-color);
    color: var(--global-theme-color);
  }
  .btn-theme-inactive:hover {
    background-color: rgba(0, 0, 0, 0.05);
    color: var(--global-theme-color);
  }
  [data-theme='dark'] .btn-theme-inactive:hover {
    background-color: rgba(255, 255, 255, 0.1);
  }
</style>

<div class="text-center mb-4">
  <button id="btn-academic" class="btn btn-theme-active mr-2" onclick="setCV('academic')">Academic CV</button>
  <button id="btn-industry" class="btn btn-theme-inactive" onclick="setCV('industry')">Industry Resume</button>
</div>

<div class="cv-container" style="height: 100vh; width: 100%; border: 1px solid var(--global-divider-color, #ccc); border-radius: 5px;">
  <object id="cv-object" data="{{ '/assets/pdf/academic_cv.pdf' | relative_url }}" type="application/pdf" width="100%" height="100%">
    <div class="p-5 text-center">
      <p>It appears your browser has trouble rendering the PDF directly.</p>
      <a id="cv-fallback-link" href="{{ '/assets/pdf/academic_cv.pdf' | relative_url }}" class="btn btn-theme-active" target="_blank">Download PDF to view</a>
    </div>
  </object>
</div>

<script>
  function setCV(type) {
    const obj = document.getElementById('cv-object');
    const fallbackLink = document.getElementById('cv-fallback-link');
    const btnAcademic = document.getElementById('btn-academic');
    const btnIndustry = document.getElementById('btn-industry');
    
    let pdfPath = "";
    if (type === 'academic') {
      pdfPath = "{{ '/assets/pdf/academic_cv.pdf' | relative_url }}";
      btnAcademic.className = "btn btn-theme-active mr-2";
      btnIndustry.className = "btn btn-theme-inactive";
    } else {
      pdfPath = "{{ '/assets/pdf/industry_resume.pdf' | relative_url }}";
      btnIndustry.className = "btn btn-theme-active";
      btnAcademic.className = "btn btn-theme-inactive mr-2";
    }
    
    // Update the object data and the fallback link
    obj.data = pdfPath;
    if (fallbackLink) {
      fallbackLink.href = pdfPath;
    }
  }
</script>
