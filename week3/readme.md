# Week3 웹사이트 구현

## 웹사이트 메인화면 구현

# 🗺️ Local Hotspot

**현지인이 추천하는 맛집 리스트를 지역별로 소개하는 웹사이트입니다!**  
여행자와 미식가 모두에게 유용한 지역별 맛집 가이드로,  
심플하고 직관적인 UI와 함께 지도로 위치까지 제공합니다.

## 📸 데모 화면

![메인화면 예시](images/screenshot-main.png) <!-- 필요 시 캡쳐 이미지 넣기 -->

---

## 🛠️ 사용 기술

- **HTML5 + CSS3**  
- **Google Fonts - Noto Sans KR**
- **반응형 디자인 (Responsive Web)**  
- **간단한 애니메이션** (로고 등장 효과)

---

## 📁 프로젝트 구조



- 지도 안내 아이콘:
  🗺️ [images/free-icon-south-korea-4498545.png]
  > 지역 버튼을 누르면 현지 맛집 리스트와 지도가 함께 제공돼요!


---

## 🧭 주요 기능

- 📍 **지역 버튼 클릭 시** 해당 지역의 맛집 리스트로 이동  
- 🗺️ **지도 아이콘**과 함께 지역 안내  
- 🎨 **애니메이션 효과**로 부드러운 첫 화면 경험  
- 📱 **모바일 반응형** 대응 (600px 이하 화면 크기 최적화)

---

## 📌 예시: 메인 페이지 구성

```html
<!-- 주요 구조 요약 -->
<div class="logo-container">
  <img src="images/free-icon-restaurant-5141120.png" alt="Hotspot Icon">
  <div class="logo-text">Local Hotspot</div>
</div>

<main>
  <h2>현지인이 추천해주는 맛집 리스트!</h2>
  <p>Local Hotspot을 이용해 지역별 현지인 맛집 리스트를 추천받으세요!</p>

  <div class="region-buttons">
    <a href="seoul.html"><button>서울</button></a>
    <a href="busan.html"><button>부산</button></a>
    <a href="jeonju.html"><button>전주</button></a>
    <a href="jeju.html"><button>제주</button></a>
    <a href="gangneung.html"><button>강릉</button></a>
  </div>

  <div class="map-icon-section">
    <img src="images/free-icon-south-korea-4498545.png" alt="지도 아이콘">
    <p>지역 버튼을 누르면 현지 맛집 리스트와 지도가 함께 제공돼요!</p>
  </div>
</main>

## 최종 작성 코드
