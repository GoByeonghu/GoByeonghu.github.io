---
layout: resume
title: "About Me"
categories: AboutMe
---

<!-- 제목 -->
<h2 style="text-align: center; margin-top: 20px;">이력서</h2>
<!-- PDF 컨테이너 -->
<div style="position: relative; display: inline-block; width: 100%;">
    <!-- 전체 화면 버튼 -->
    <button id="fullscreenBtn1" 
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
    <object id="pdfViewer1" type="application/pdf" data="assets/resume.pdf" style="width: 100%; height: 800px;">
        <p>죄송해요, 사용하신 브라우저가 PDF 삽입을 지원하지 않아요. 😢 
           <a href="https://gobyeonghu.github.io/assets/resume.pdf">이력서 직접 다운로드 해보기</a>
        </p>
    </object>
</div>

<br/>
<br/>
<br/>
<br/>
<br/>


<!-- 제목 -->
<h2 style="text-align: center; margin-top: 20px;">포트폴리오</h2>
<!-- PDF 컨테이너 -->
<div style="position: relative; display: inline-block; width: 100%;">
    <!-- 전체 화면 버튼 -->
    <button id="fullscreenBtn2" 
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
    <object id="pdfViewer2" type="application/pdf" data="assets/portfolio.pdf" style="width: 100%; height: 800px;">
        <p>죄송해요, 사용하신 브라우저가 PDF 삽입을 지원하지 않아요. 😢 
           <a href="https://gobyeonghu.github.io/assets/portfolio.pdf">포트폴리오 직접 다운로드 해보기</a>
        </p>
    </object>
</div>

<!-- Fullscreen 버튼 스크립트 -->
<script>
  const fullscreenBtn1 = document.getElementById('fullscreenBtn1');
  const pdfViewer1 = document.getElementById('pdfViewer1');

  const fullscreenBtn2 = document.getElementById('fullscreenBtn2');
  const pdfViewer2 = document.getElementById('pdfViewer2');

  fullscreenBtn1.addEventListener('click', () => {
    if (pdfViewer1.requestFullscreen) {
      pdfViewer1.requestFullscreen();
    } else if (pdfViewer1.webkitRequestFullscreen) { // Safari
      pdfViewer1.webkitRequestFullscreen();
    } else if (pdfViewer1.msRequestFullscreen) { // IE11
      pdfViewer1.msRequestFullscreen();
    }
  });

  fullscreenBtn2.addEventListener('click', () => {
    if (pdfViewer2.requestFullscreen) {
      pdfViewer2.requestFullscreen();
    } else if (pdfViewer2.webkitRequestFullscreen) { // Safari
      pdfViewer2.webkitRequestFullscreen();
    } else if (pdfViewer2.msRequestFullscreen) { // IE11
      pdfViewer2.msRequestFullscreen();
    }
  });
</script>
