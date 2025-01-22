---
layout: resume
title: "About Me"
categories: AboutMe
---

<!-- PDF 컨테이너 -->
<div style="position: relative; display: inline-block; width: 100%;">
    <!-- 전체 화면 버튼 -->
    <button id="fullscreenBtn" 
        style="position: absolute; 
               top: -50px; 
               left: 0; 
               padding: 10px 15px; 
               background-color: transparent; 
               color: #6c757d; 
               border: 2px solid #6c757d; 
               border-radius: 5px; 
               font-size: 14px; 
               font-weight: 500; 
               cursor: pointer; 
               transition: background-color 0.3s, color 0.3s;">
    전체 화면으로 보기
    </button>
    <!-- PDF Object -->
    <object id="pdfViewer" type="application/pdf" data="assets/resume.pdf" style="width: 100%; height: 800px;">
        <p>죄송해요, 사용하신 브라우저가 PDF 삽입을 지원하지 않아요. 😢 
           <a href="https://gobyeonghu.github.io/assets/resume.pdf">직접 다운로드 해보기</a>
        </p>
    </object>
</div>

<!-- Fullscreen 버튼 스크립트 -->
<script>
  const fullscreenBtn = document.getElementById('fullscreenBtn');
  const pdfViewer = document.getElementById('pdfViewer');

  fullscreenBtn.addEventListener('click', () => {
    if (pdfViewer.requestFullscreen) {
      pdfViewer.requestFullscreen();
    } else if (pdfViewer.webkitRequestFullscreen) { // Safari
      pdfViewer.webkitRequestFullscreen();
    } else if (pdfViewer.msRequestFullscreen) { // IE11
      pdfViewer.msRequestFullscreen();
    }
  });
</script>
