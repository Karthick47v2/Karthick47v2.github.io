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

<div class="d-flex justify-content-between align-items-center mb-4">
  <div>
    <button id="btn-academic" class="btn btn-theme-active mr-2" onclick="setCV('academic')">Academic CV</button>
    <button id="btn-industry" class="btn btn-theme-inactive" onclick="setCV('industry')">Industry Resume</button>
  </div>
  <div>
    <a id="btn-download" href="{{ '/assets/pdf/academic_cv.pdf' | relative_url }}" class="btn btn-outline-primary" target="_blank" download>
      <i class="fas fa-file-download mr-1"></i> Download PDF
    </a>
  </div>
</div>

<div class="cv-container" style="height: 100vh; width: 100%; border: 1px solid var(--global-divider-color, #ccc); border-radius: 5px; overflow: hidden;">
  <iframe id="cv-iframe" src="{{ '/assets/pdf/academic_cv.pdf' | relative_url }}#toolbar=0" style="width: 100%; height: 100%; border: none;"></iframe>
</div>

<script>
  function setCV(type) {
    const iframe = document.getElementById('cv-iframe');
    const downloadBtn = document.getElementById('btn-download');
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
    
    // Update iframe and download link
    iframe.src = pdfPath + "#toolbar=0";
    if (downloadBtn) {
      downloadBtn.href = pdfPath;
    }
  }
</script>
