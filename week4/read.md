# Week4 웹사이트 구현

##  지역별 웹사이트 화면 구현

# 🥢 Local Hotspot - 현지인이 추천하는 전국 맛집 지도

**Local Hotspot**은 서울, 전주, 제주, 부산, 강릉 등 다양한 지역의 **현지인 추천 맛집**을 소개하는 웹사이트입니다. 지역 버튼을 누르면 지도와 함께 인기 맛집을 확인할 수 있어요!

---

## 📌 주요 기능

- ✅ 지역별 현지 맛집 정보 제공
- ✅ Google Maps 또는 Naver Maps 연동
- ✅ 카드 형식의 리뷰 UI
- ✅ 현지인 vs 외지인 평점 구분
- ✅ 반응형 디자인 (모바일 대응)
- ✅ 로고 애니메이션 및 감성 배경 효과
---

## 🌆 지역별 맛집 구성

### 📍 서울
- **망원동 즉석떡볶이**  
- **성수 마리오네** 
- **연남 갓잇**
  
-> **현지인/외지인 평점 구분**

### 📍 부산
- **사상 찹천일류돼지국밥**  
- **남포동 씨앗호떡** 
- **해운대 가야밀면**  

-> **현지인/외지인 평점 구분**

### 📍 전주
- **전동 떡갈비**
- **효자 막창**
- **한국집**

-> **현지인/외지인 평점 구분**

### 📍 제주
- **돈사돈**
- **우진 해장국**
- **내가리 식당**

-> **현지인/외지인 평점 구분**

### 📍 강릉
- **돈사돈**
- **우진 해장국**
- **내가리 식당**

-> **현지인/외지인 평점 구분**


---

## 🛠️ 기술 스택

- HTML5 / CSS3 (Vanilla)
- Google Fonts (Noto Sans KR)
- 애니메이션: `@keyframes`
- 지도: Google Maps iframe, Naver Map 링크
- 반응형: Media Queries

---

## ✨ 향후 개선사항 (TODO)

- [ ] 리뷰 사용자 등록 기능 추가
- [ ] 별점 평가 기능 연동 (JS 또는 DB)
- [ ] 어드민 페이지로 신규 맛집 등록
- [ ] 다국어 지원 (EN, JP 등)
- [ ] SPA 전환 (React / Vue 가능)

---

## 🖼️ 미리보기 (스크린샷)

## 최종 작성 코드 

<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>서울 맛집</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      padding: 2rem;
      color: #333;
      background: linear-gradient(to bottom, #f3f3f3, #e0e0e0);
      background-attachment: fixed;
      background-repeat: no-repeat;
      position: relative;
    }

    body::after {
      content: '';
      position: fixed;
      bottom: 0;
      left: 0;
      width: 100%;
      height: 200px;
      background: url('images/seoul-silhouette-namsan.svg') no-repeat bottom center;
      background-size: cover;
      opacity: 0.15;
      pointer-events: none;
      z-index: -1;
    }

    h1 {
      text-align: center;
      font-size: 2.2rem;
      margin-bottom: 1.5rem;
    }

    p {
      text-align: center;
      margin-bottom: 2rem;
    }

    .map-container {
      display: flex;
      justify-content: center;
      margin-bottom: 2rem;
    }

    iframe {
      border: 1px solid #aaa;
      border-radius: 1rem;
      width: 90%;
      max-width: 800px;
      height: 400px;
    }

    .restaurant-list {
      max-width: 800px;
      margin: 0 auto;
      display: flex;
      flex-direction: column;
      gap: 1.5rem;
    }

    .restaurant-card {
      display: flex;
      align-items: center;
      background-color: #ffffff;
      border: 1.5px solid #ddd;
      border-radius: 1rem;
      padding: 1rem;
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
      gap: 1.2rem;
      transition: transform 0.2s ease;
    }

    .restaurant-card:hover {
      transform: translateY(-5px);
    }

    .thumbnail {
      width: 90px;
      height: 90px;
      object-fit: cover;
      border-radius: 0.75rem;
      border: 1px solid #ccc;
    }

    .card-content {
      flex: 1;
      text-align: left;
    }

    .card-content h2 {
      margin-bottom: 0.5rem;
      font-size: 1.5rem;
      color: #ff5733;
    }

    .card-content .review {
      font-style: italic;
      color: #555;
      margin-bottom: 0.5rem;
    }

    .card-content .address {
      font-size: 0.95rem;
      color: #777;
    }

    .stars {
      font-size: 1.2rem;
      color: #ffcc00;
      margin-bottom: 0.3rem;
    }

    .map-link {
      display: inline-block;
      margin-top: 0.5rem;
      font-size: 0.95rem;
      color: #2a8cff;
      text-decoration: none;
      border-bottom: 1px solid transparent;
      transition: all 0.2s;
    }

    .map-link:hover {
      border-bottom: 1px solid #2a8cff;
    }
  </style>
</head>
<body>
  <h1>서울 맛집 리스트</h1>
  <p>망원동, 성수동, 연남동 등 서울의 현지 맛집을 만나보세요!</p>

  <div class="map-container">
    <iframe 
      src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3162.593907078092!2d126.91041667638044!3d37.5551957242274!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x357c9f31d4d4460d%3A0x5cf8dc6b258b258f!2z66-87KeA64yA7ZmU66as!5e0!3m2!1sko!2skr!4v1712565438285!5m2!1sko!2skr"
      allowfullscreen=""
      loading="lazy"
      referrerpolicy="no-referrer
