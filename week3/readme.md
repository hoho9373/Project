# Week3 웹사이트 구현


# 🌍 GoVoyage 메인화면 기술 및 기능 정리

---

## ✅ 사용 기술 (기술 스택)

| 분류 | 기술 |
|------|------|
| **Markup** | HTML5 (`<!DOCTYPE html>`) |
| **스타일링** | 내장 CSS (`<style>` 태그 사용) |
| **폰트** | Noto Sans KR (웹 폰트로 추정, 외부 링크는 현재 없음) |
| **반응형 디자인** | `viewport` 설정 포함 (`<meta name="viewport"...>`) |
| **애니메이션** | CSS `@keyframes` 사용 (shine 애니메이션) |
| **레이아웃 구성** | Flexbox 기반 구조 (`display: flex`, `justify-content`, `gap`, 등) |

---

## ⭐ 주요 기능 및 화면 구성

### 1. 상단 헤더
- 사이트 제목 **GoVoyage**를 크게 강조
- 주황 계열의 컬러로 브랜드 아이덴티티 형성
- 사용자에게 강렬한 인상 제공

---

### 2. 내비게이션 바
- **항공권, 호텔, 음식, 관광지, 일정 만들기** 페이지로 이동
- 직관적인 이모지와 함께 제공되어 탐색성 우수
- `nav a:hover` 효과로 사용성 강화

---

### 3. 메인 콘텐츠 영역 (왼쪽)

#### 🔹 여행 소개 섹션
- GoVoyage 웹사이트의 통합 여행 추천 서비스 설명

#### 🔹 여행 성향 테스트 섹션
- 사용자의 성향을 분석하여 최적의 여행지를 추천
- `test.html` 링크를 통해 성향 테스트 페이지로 이동 가능

---

### 4. 이 달의 추천 여행지 (오른쪽 사이드바)

- **추천 도시**: 이탈리아 로마, 일본 도쿄, 프랑스 파리
- CSS 애니메이션 (`@keyframes`)을 사용해 배경에 반짝이는 효과 적용
- 각 도시 카드에 `hover` 시 `transform` 효과로 생동감 부여
- 여행지 항목은 `.destination-item` 클래스를 사용하여 스타일 적용

---

## 💡 향후 확장 아이디어

- 각 추천 항목(항공, 호텔, 음식 등)에 JS 연동 → 실시간 데이터 호출
- 여행지 카드에 이미지 추가 및 상세 정보 보기 기능 추가
- 여행 성향 테스트 결과에 따라 추천 여행지를 동적으로 변경



## 📌 최종 작성 코드

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>GoVoyage</title>
  <style>
    body {
      margin: 0;
      font-family: 'Noto Sans KR', sans-serif;
      background-color: #fdfdfd;
      display: flex;
      flex-direction: column;
    }
    header {
      background-color: #ff7043;
      color: white;
      padding: 1.2rem 2rem;
      text-align: center;
      font-size: 1.8rem;
      font-weight: bold;
    }
    nav {
      display: flex;
      justify-content: center;
      background-color: #fff3e0;
      padding: 1rem 0;
      gap: 2rem;
      flex-wrap: wrap;
    }
    nav a {
      text-decoration: none;
      color: #333;
      font-weight: 600;
      padding: 0.5rem 1rem;
      border-radius: 1.5rem;
      transition: all 0.2s ease-in-out;
    }
    nav a:hover {
      background-color: #ffe0b2;
    }
    main {
      display: flex;
      justify-content: space-between;
      padding: 2rem;
      gap: 2rem;
    }
    .content {
      width: 70%;
    }
    .section {
      margin: 3rem auto;
      background-color: #fff8f0;
      padding: 2rem;
      border-radius: 1rem;
      box-shadow: 0 4px 10px rgba(0,0,0,0.05);
    }
    .section h2 {
      color: #ff7043;
    }
    .section p {
      color: #444;
      line-height: 1.6;
    }

    /* 이 달의 여행지 강조 */
    .travel-destination {
      width: 25%;
      background: linear-gradient(45deg, #ff7043, #ffab91);
      padding: 2rem;
      border-radius: 1rem;
      box-shadow: 0 4px 10px rgba(0,0,0,0.05);
      animation: shine 2s infinite;
    }
    .travel-destination h3 {
      color: #fff;
      margin-bottom: 1rem;
      text-shadow: 2px 2px 4px rgba(0,0,0,0.6);
    }
    .destination-list {
      display: flex;
      flex-direction: column;
      gap: 1.5rem;
    }
    .destination-item {
      background-color: #fff8f0;
      padding: 1rem;
      border-radius: 0.8rem;
      box-shadow: 0 4px 5px rgba(0,0,0,0.1);
      transition: transform 0.3s ease;
      cursor: pointer;
    }
    .destination-item:hover {
      transform: translateY(-5px);
    }
    .destination-item img {
      width: 100%;
      border-radius: 0.8rem;
    }
    .destination-item p {
      font-weight: bold;
      margin-top: 0.5rem;
      color: #333;
    }

    /* 반짝이는 효과 */
    @keyframes shine {
      0% {
        background: linear-gradient(45deg, #ff7043, #ffab91);
        box-shadow: 0 0 20px rgba(255, 105, 0, 0.8);
      }
      50% {
        background: linear-gradient(45deg, #ffab91, #ff7043);
        box-shadow: 0 0 30px rgba(255, 105, 0, 0.8);
      }
      100% {
        background: linear-gradient(45deg, #ff7043, #ffab91);
        box-shadow: 0 0 20px rgba(255, 105, 0, 0.8);
      }
    }
  </style>
</head>
<body>
  <header>
    GoVoyage 
  </header>

  <nav>
    <a href="flights.html">✈️ 최저가 항공권</a>
    <a href="hotel.html">🏨 리뷰 많은 호텔</a>
    <a href="food.html">🍜 음식점 추천</a>
    <a href="attractions.html">🗺 관광지 추천</a>
    <a href="planner.html">📅 여행 일정 만들기</a>
  </nav>

  <main>
    <div class="content">
      <div class="section">
        <h2>당신만의 여행을 계획하세요</h2>
        <p>
          출발 전 최고의 여행을 위해, 현지 정보 기반의 항공권, 숙소, 음식점, 관광지까지!<br>
          여기에 당신의 성향을 반영한 맞춤 일정 플래너까지 한 번에 준비하세요.
        </p>
      </div>

      <div class="section">
        <h2>🚀 여행 성향 테스트</h2>
        <p>
          감성 여행파? 먹방 위주? 딱 맞는 추천지를 찾아드립니다.<br>
          <a href="test.html">여행 성향 테스트 시작하기 →</a>
        </p>
      </div>
    </div>

    <!-- 이 달의 여행지 강조 -->
    <div class="travel-destination">
      <h3>이 달의 추천 여행지 !!!!</h3>
      <div class="destination-list">
        <div class="destination-item">
          <p>이탈리아 로마</p>
        </div>
        <div class="destination-item">
          <p>일본 도쿄</p>
        </div>
        <div class="destination-item">
        
          <p>프랑스 파리</p>
        </div>
      </div>
    </div>
  </main>
</body>
</html>

```

## 🖼️ 미리보기 (스크린샷)
<img width="1280" alt="image" src="https://github.com/user-attachments/assets/46e0351d-4b33-43ef-83a3-fe5264a6ae65" />


