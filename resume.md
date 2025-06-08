---
layout: resume
title: "About Me"
categories: AboutMe
---

<style>
  .section {
    margin: 80px auto;
    max-width: 960px;
    padding: 0 20px;
  }

  .section h2 {
    text-align: center;
    margin-bottom: 20px;
  }

  .fullscreen-btn {
    display: block;
    margin: 0 auto 20px auto; /* 버튼이 위쪽에, 가운데 정렬되며 아래 여백 추가 */
    padding: 10px 20px;
    font-size: 14px;
    font-weight: 500;
    border: 2px solid #6c757d;
    color: #6c757d;
    background-color: white;
    border-radius: 6px;
    cursor: pointer;
    transition: all 0.3s ease;
  }

  .fullscreen-btn:hover {
    background-color: #6c757d;
    color: white;
  }

  .pdf-viewer {
    width: 100%;
    height: 800px;
    border: 1px solid #ddd;
    display: block;
  }

  .pdf-wrapper {
    margin-bottom: 60px;
  }
</style>

<div class="section">
  <h2>이력서</h2>
  <button class="fullscreen-btn" onclick="openFullscreen('pdfViewer1')">전체 화면으로 보기</button>
  <div class="pdf-wrapper">
    <iframe id="pdfViewer1" class="pdf-viewer" src="assets/resume.pdf"></iframe>
  </div>
</div>

<div class="section">
  <h2>포트폴리오</h2>
  <button class="fullscreen-btn" onclick="openFullscreen('pdfViewer2')">전체 화면으로 보기</button>
  <div class="pdf-wrapper">
    <iframe id="pdfViewer2" class="pdf-viewer" src="assets/portfolio.pdf"></iframe>
  </div>
</div>

<script>
  function openFullscreen(id) {
    const elem = document.getElementById(id);
    if (elem.requestFullscreen) {
      elem.requestFullscreen();
    } else if (elem.webkitRequestFullscreen) {
      elem.webkitRequestFullscreen();
    } else if (elem.msRequestFullscreen) {
      elem.msRequestFullscreen();
    }
  }
</script>
