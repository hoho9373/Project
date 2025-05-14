# Week4 웹사이트 구현

##  항공권 웹사이트 구현

# ✈️ 항공권 검색 페이지 (flights.html) 정리

---

## ✅ 사용 기술

| 분류        | 기술 및 도구 |
|-------------|--------------|
| **Markup**  | HTML5 |
| **스타일링**| CSS (내부 `<style>` 태그 사용) |
| **프론트엔드 로직** | JavaScript (DOM 조작, 이벤트 처리, 비동기 함수) |
| **폰트** | Noto Sans KR (한글 웹 폰트) |
| **레이아웃 구성** | Flexbox, 반응형 뷰포트 설정 |
| **UX 요소** | 버튼 hover, 로딩 처리, 입력폼 인터페이스 |

---

## ⭐ 주요 기능

### 1. 항공권 검색 폼

- 사용자 입력 항목:
  - 출발지 (`departure`)
  - 도착지 (`destination`)
  - 출발 날짜 (`date`)
  - 시간대 필터 (`select` 박스: 오전/오후/저녁)
- `submit` 이벤트 감지 후 JavaScript로 처리

---

### 2. 항공권 검색 결과 출력

- `fetchFlights()` 함수는 현재 **더미 데이터** 사용
- 실제 구현 시 항공권 검색 API 연동 가능
- 결과는 `displayFlights()` 함수를 통해 `.results` 영역에 출력됨
- 각 결과 항목은 항공사, 출발시간, 가격을 표시

---

### 3. 사용자 인터페이스(UI)

- 내비게이션 바: 메인, 항공권, 호텔, 음식점, 관광지, 일정 등으로 이동 가능
- `메인 화면으로 돌아가기` 버튼 제공 (`11.html`로 연결)
- 폼 구성 요소는 `label + input/select` 조합으로 직관적인 UX 제공
- `hover` 및 `transition` 효과를 사용하여 반응성 있는 디자인 구성

---

### 4. 부가 기능

- **로딩 인디케이터** (`#loadingSpinner`) 구현 준비됨 (아직 spinner 스타일은 없음)
- 사용자가 검색 버튼 클릭 시 로딩 상태 표시 가능

---

## 💡 향후 확장 아이디어

- 항공권 API 연동 (예: Skyscanner API, Kiwi API 등)
- 시간대 필터링 로직 적용 (현재 시간대는 UI만 구현되어 있음)
- 로딩 스피너 디자인 추가 (`.spinner` 요소 스타일 정의 필요)
- 사용자가 검색 조건을 변경해도 URL 파라미터로 유지되도록 개선


---


## 최종 작성 코드 

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>항공권 검색</title>
  <style>
    body {
      font-family: 'Noto Sans KR', sans-serif;
      background-color: #fff9f5;
      margin: 0;
      padding: 0;
    }
    header {
      background-color: #ff7043;
      color: white;
      padding: 1.5rem;
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
      max-width: 800px;
      margin: 2rem auto;
      background-color: #fff;
      padding: 2rem;
      border-radius: 1rem;
      box-shadow: 0 4px 10px rgba(0,0,0,0.05);
    }
    .form-group {
      margin-bottom: 1.5rem;
    }
    label {
      display: block;
      margin-bottom: 0.5rem;
      font-weight: 600;
    }
    input, select {
      width: 100%;
      padding: 0.8rem;
      font-size: 1rem;
      border: 1px solid #ddd;
      border-radius: 0.5rem;
    }
    button {
      padding: 0.8rem 1.5rem;
      background-color: #ff7043;
      color: #fff;
      border: none;
      border-radius: 0.5rem;
      font-size: 1rem;
      cursor: pointer;
      transition: background-color 0.3s ease;
    }
    button:hover {
      background-color: #ff5722;
    }
    .results {
      margin-top: 2rem;
    }
    .flight-item {
      padding: 1rem;
      border: 1px solid #eee;
      border-radius: 0.5rem;
      margin-bottom: 1rem;
      background-color: #fffaf6;
    }
    #backToHome {
      margin-top: 2rem;
      padding: 1rem;
      background-color: #ff7043;
      color: white;
      border: none;
      border-radius: 0.5rem;
      font-size: 1.2rem;
      cursor: pointer;
      text-align: center;
      display: block;
      width: 100%;
    }
    #backToHome:hover {
      background-color: #ff5722;
    }
  </style>
</head>
<body>
  <div id="loadingSpinner" style="display: none;">
    <div class="spinner"></div>
  </div>

  <header>✈️ 최저가 항공권 찾기</header>
   <nav>
    <a href="11.html">🏠 메인</a>
    <a href="flights.html">✈️ 항공권</a>
    <a href="hotel.html">🏨 호텔</a>
    <a href="food.html">🍜 음식점</a>
    <a href="attractions.html">🗺 관광지</a>
    <a href="planner.html">📅 일정</a>
  </nav>
  
  <main>
    <form id="flightForm">
      <label for="departure">출발지</label>
      <input type="text" id="departure">
      
      <label for="destination">도착지</label>
      <input type="text" id="destination">
      
      <label for="date">출발 날짜</label>
      <input type="date" id="date">
      
      <label for="timeFilter">시간대</label>
      <select id="timeFilter">
        <option value="morning">오전</option>
        <option value="afternoon">오후</option>
        <option value="evening">저녁</option>
      </select>

      <button type="submit">항공권 검색</button>
    </form>

    <div class="results" id="flightResults"></div>
    
    <button id="backToHome" onclick="window.location.href='11.html'">메인 화면으로 돌아가기</button> <!-- 메인 화면으로 돌아가기 버튼 -->
  </main>

  <script>
    document.getElementById('flightForm').addEventListener('submit', function(e) {
      e.preventDefault();
      const departure = document.getElementById('departure').value;
      const destination = document.getElementById('destination').value;
      const date = document.getElementById('date').value;
      const timeFilter = document.getElementById('timeFilter').value;
      const loadingSpinner = document.getElementById('loadingSpinner');

      loadingSpinner.style.display = 'block'; // 로딩 시작

      fetchFlights(departure, destination, date, timeFilter)
        .then(flights => {
          displayFlights(flights);
          loadingSpinner.style.display = 'none'; // 로딩 끝
        });
    });

    async function fetchFlights(departure, destination, date, timeFilter) {
      // 여기에서 API를 호출하고 데이터를 가져오는 부분
      // 예시로 더미 데이터를 사용함
      const flights = [
        { airline: '대한항공', departureTime: '2025-06-15T08:00:00', price: 120000 },
        { airline: '아시아나', departureTime: '2025-06-15T10:15:00', price: 110000 },
        { airline: '에어부산', departureTime: '2025-06-15T13:00:00', price: 98000 }
      ];

      return flights;
    }

    function displayFlights(flights) {
      const results = document.getElementById('flightResults');
      results.innerHTML = ''; // 결과 초기화

      flights.forEach(flight => {
        const div = document.createElement('div');
        div.classList.add('flight-item');
        div.innerHTML = `<strong>${flight.airline}</strong><br>${flight.departureTime}<br>${flight.price}원`;
        results.appendChild(div);
      });
    }
  </script>
</body>
</html>

```
## 🖼️ 미리보기 (스크린샷)
<img width="1277" alt="image" src="https://github.com/user-attachments/assets/e4479043-c5d9-4ef0-bb69-0138e7c4871c" />

<img width="1267" alt="image" src="https://github.com/user-attachments/assets/0f78d0a1-9848-4c24-b98d-f74c15d46b55" />

---
# 🏨 나라별 호텔 추천 웹페이지

## 📌 개요
사용자가 나라 이름을 입력하면 해당 국가의 지역 버튼이 생성되고, 버튼 클릭 시 해당 지역의 추천 호텔 3곳을 보여주는 인터랙티브 웹페이지입니다.

---

## 🛠 사용 기술

| 기술 스택 | 설명 |
|-----------|------|
| **HTML5** | 웹 페이지 구조 작성 |
| **CSS3** | 시각적 스타일링 및 반응형 디자인 구현 |
| **Vanilla JavaScript** | 동적 DOM 조작 및 사용자 입력 처리 |
| **Datalist** | 나라 이름 자동완성 기능 제공 |
| **Flex/Grid Layout** | 버튼과 호텔 리스트를 반응형으로 배치 |

---

## ⚙ 주요 기능

- 🔍 **나라 검색 기능**  
  사용자가 입력한 나라 이름을 기반으로 해당 지역 버튼 동적 생성

- 💡 **자동완성 지원**  
  `<datalist>` 태그를 사용하여 입력값 자동완성 제공

- 🌍 **지역 버튼 생성**  
  선택된 나라의 지역 리스트를 버튼으로 렌더링

- 🏨 **호텔 카드 출력**  
  선택된 지역에 따라 호텔 3곳을 카드 형태로 출력 (이름, 설명, 이미지, 평점)

- ❌ **결과 없음 안내**  
  검색어가 없거나 일치하는 국가가 없는 경우 사용자에게 안내 메시지 출력

- 📱 **반응형 디자인**  
  화면 크기에 따라 호텔 카드가 유연하게 정렬됨 (`grid` 사용)

- ✨ **선택된 나라 표시**  
  검색 후 상단에 선택된 나라 이름 강조 출력

---

## 🔧 향후 확장 아이디어

- 지역별 호텔 상세 페이지 연결
- 사용자 리뷰 및 평점 시스템 구축
- 외부 API 연동으로 호텔 정보 실시간 불러오기
- 나라 선택을 드롭다운 셀렉터로 구현

---

## ✨ UI 예시

- 상단 네비게이션 바
- 검색창 및 나라 자동완성
- 지역 버튼 → 클릭 시 호텔 카드 출력
- 카드: 호텔 이미지 + 이름 + 설명 + 별점




## 최종 작성 코드 

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>나라별 호텔 추천</title>
  <style>
    body {
      margin: 0;
      font-family: 'Noto Sans KR', sans-serif;
      background-color: #fdfdfd;
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
      padding: 2rem;
      max-width: 2000px;
      margin: auto;
    }

    .search-box {
      text-align: center;
      margin-bottom: 2rem;
    }

    .search-box input {
      padding: 0.6rem 1rem;
      width: 320px;
      font-size: 1rem;
      border: 1px solid #ccc;
      border-radius: 2rem;
    }

    .region-buttons {
  text-align: center;
  margin-top: 2rem;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 1rem; /* 버튼들 사이에 1rem 간격 추가 */
}

    .region-buttons button {
      padding: 0.8rem 1.5rem;
      font-size: 1.1rem;
      background-color: #ff7043;
      color: white;
      border: none;
      border-radius: 1.5rem;
      cursor: pointer;
      transition: background-color 0.3s;
    }

    .region-buttons button:hover {
      background-color: #ff5722;
    }

    .hotel-list {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
      gap: 2rem;
      margin-top: 2rem;
    }

    .hotel {
      background-color: #fffef6;
      border-radius: 1rem;
      box-shadow: 0 4px 10px rgba(0,0,0,0.05);
      padding: 1rem;
      text-align: center;
    }

    .hotel img {
      width: 100%;
      height: 180px;
      object-fit: cover;
      border-radius: 0.8rem;
    }

    .hotel h3 {
      color: #ff7043;
      margin: 1rem 0 0.5rem;
    }

    .hotel p {
      color: #555;
      font-size: 0.95rem;
      line-height: 1.4;
    }

    .hotel .reviews {
      margin-top: 0.5rem;
      font-size: 0.9rem;
      color: #777;
    }

    .no-result {
      text-align: center;
      color: #888;
      font-size: 1rem;
      margin-top: 2rem;
    }
  </style>
</head>
<body>
  <header>🏨 나라별 호텔 추천</header>
  <nav>
    <a href="11.html">🏠 메인</a>
    <a href="flights.html">✈️ 항공권</a>
    <a href="hotel.html">🏨 호텔</a>
    <a href="food.html">🍜 음식점</a>
    <a href="attractions.html">🗺 관광지</a>
    <a href="planner.html">📅 일정</a>
  </nav>

  <main>
    <div class="search-box">
      <input type="text" id="searchInput" placeholder="나라 이름을 입력하세요 (예: 일본, 베트남, 미국)" oninput="searchCountry()" />
    </div>

    <div class="region-buttons" id="regionButtons"></div>
    <div class="hotel-list" id="hotelList"></div>
    <div class="no-result" id="noResult">나라 이름을 입력하면 추천 호텔이 보여집니다.</div>
  </main>

   <script>
    const hotelData = {
      "일본": {
        "도쿄": [
          {name: "호텔 뉴 오타니 도쿄 가든 타워", description: "도쿄 타워 인근의 럭셔리 호텔", rating: "⭐ 4.8 (1,200개 리뷰)", img: "images/doho1.jpg"},
          {name: "더 아오야마 그랜드 호텔", description: "신주쿠 중심가의 편리한 위치", rating: "⭐ 4.7 (1,100개 리뷰)", img: "images/doho2.jpg"},
          {name: "미츠이 가든 호텔 긴자 프리미어", description: "비즈니스 여행객을 위한 고급스러운 숙소", rating: "⭐ 4.6 (900개 리뷰)", img: "images/doho3.jpg"}
        ],
        "오사카": [
          {name: "도미인 프리미엄 난바 내추럴 핫 스프링", description: "오사카 성 근처의 고급 호텔", rating: "⭐ 4.9 (950개 리뷰)", img: "images/ohho1.jpg"},
          {name: "홀리데이 인 오사카 난바", description: "난바 지역의 편리한 숙박 시설", rating: "⭐ 4.7 (850개 리뷰)", img: "images/ohho2.jpg"},
          {name: "호텔 윙 인터내셔널 프리미엄 오사카 신세카이", description: "오사카의 전통적인 분위기를 느낄 수 있는 호텔", rating: "⭐ 4.8 (780개 리뷰)", img: "images/ohho3.jpg"}
        ],
        "후쿠오카": [
          {name: "후쿠오카 비즈니스 호텔", description: "후쿠오카 시내 중심의 비즈니스 호텔", rating: "⭐ 4.7 (800개 리뷰)", img: "images/fuho1.jpg"},
          {name: "하카타 리버뷰 호텔", description: "하카타 역 근처의 고급스러운 숙소", rating: "⭐ 4.8 (900개 리뷰)", img: "images/fuho2.jpg"},
          {name: "텐진 마르이 호텔", description: "텐진 쇼핑 거리와 가까운 호텔", rating: "⭐ 4.6 (770개 리뷰)", img: "images/fuho3.jpg"}
        ]
      },
      "베트남": {
        "호치민": [
          {name: "호치민 시티 호텔", description: "호치민 시티 중심에 위치한 고급 호텔", rating: "⭐ 4.8 (1,200개 리뷰)", img: "images/hoho1.jpg"},
          {name: "벤탄 시장 근처 호텔", description: "벤탄 시장과 가까운 편리한 위치", rating: "⭐ 4.7 (850개 리뷰)", img: "images/hoho2.jpg"},
          {name: "푸미흥 리버사이드 호텔", description: "호치민의 아름다운 리버뷰 호텔", rating: "⭐ 4.9 (900개 리뷰)", img: "images/hoho3.jpg"}
        ],
        "하노이": [
          {name: "하노이 올드쿼터 호텔", description: "하노이 구시가지 중심의 편리한 위치", rating: "⭐ 4.8 (1,150개 리뷰)", img: "images/haho1.jpg"},
          {name: "호안끼엠 레이크 호텔", description: "호안끼엠 호수 근처의 전통적인 분위기", rating: "⭐ 4.6 (900개 리뷰)", img: "images/haho2.jpg"},
          {name: "하노이 프리미어 호텔", description: "하노이의 주요 명소와 가까운 편리한 숙소", rating: "⭐ 4.7 (800개 리뷰)", img: "images/haho3.jpg"}
        ],
        "다낭": [
          {name: "다낭 비치 리조트", description: "다낭의 아름다운 해변에서의 숙박", rating: "⭐ 4.9 (1,100개 리뷰)", img: "images/daho1.jpg"},
          {name: "다낭 시티 호텔", description: "시내 중심의 편리한 위치", rating: "⭐ 4.8 (900개 리뷰)", img: "images/daho2.jpg"},
          {name: "몽드리안 다낭", description: "럭셔리 리조트 호텔", rating: "⭐ 4.7 (850개 리뷰)", img: "images/daho3.jpg"}
        ]
      },
      "미국": {
        "뉴욕": [
          {name: "뉴욕 타임스퀘어 호텔", description: "타임스퀘어 인근의 럭셔리 호텔", rating: "⭐ 4.9 (1,200개 리뷰)", img: "images/newho1.jpg"},
          {name: "센트럴 파크 호텔", description: "센트럴 파크와 가까운 호텔", rating: "⭐ 4.8 (1,100개 리뷰)", img: "images/newho2.jpg"},
          {name: "브로드웨이 호텔", description: "브로드웨이 극장가와 인접한 위치", rating: "⭐ 4.7 (900개 리뷰)", img: "images/newho3.jpg"}
        ],
        "로스앤젤레스": [
          {name: "로스앤젤레스 비치 호텔", description: "로스앤젤레스 해변 근처의 호텔", rating: "⭐ 4.8 (1,100개 리뷰)", img: "images/loho1.jpg"},
          {name: "할리우드 스타 호텔", description: "할리우드 거리와 가까운 호텔", rating: "⭐ 4.9 (950개 리뷰)", img: "images/loho2.jpg"},
          {name: "베벌리 힐스 리조트", description: "베벌리 힐스의 고급 리조트", rating: "⭐ 4.7 (800개 리뷰)", img: "images/loho3.jpg"}
        ],
        "샌프란시스코": [
          {name: "샌프란시스코 시티 호텔", description: "시내 중심에 위치한 편리한 호텔", rating: "⭐ 4.8 (1,100개 리뷰)", img: "images/seho1.jpg"},
          {name: "금문교 호텔", description: "금문교 근처의 아름다운 숙소", rating: "⭐ 4.9 (950개 리뷰)", img: "images/seho2.jpg"},
          {name: "피셔맨스 워프 호텔", description: "피셔맨스 워프와 가까운 위치", rating: "⭐ 4.7 (900개 리뷰)", img: "images/seho3.jpg"}
        ]
      }
    };


    function searchCountry() {
      const input = document.getElementById("searchInput").value.trim();
      const regionButtons = document.getElementById("regionButtons");
      const hotelList = document.getElementById("hotelList");
      const noResult = document.getElementById("noResult");

      regionButtons.innerHTML = "";
      hotelList.innerHTML = "";
      noResult.style.display = "none";

      if (input === "" || !hotelData[input]) {
        noResult.style.display = "block";
        return;
      }

      const regions = Object.keys(hotelData[input]);
      regions.forEach(region => {
        const button = document.createElement("button");
        button.textContent = region;
        button.onclick = () => showHotels(input, region);
        regionButtons.appendChild(button);
      });
    }

    function showHotels(country, region) {
      const hotelList = document.getElementById("hotelList");
      hotelList.innerHTML = "";

      const selectedHotels = hotelData[country][region];
      selectedHotels.forEach(hotel => {
        const hotelDiv = document.createElement("div");
        hotelDiv.className = "hotel";
        hotelDiv.innerHTML = `
          <img src="${hotel.img}" alt="${hotel.name}">
          <h3>${hotel.name}</h3>
          <p>${hotel.description}</p>
          <div class="reviews">${hotel.rating}</div>
        `;
        hotelList.appendChild(hotelDiv);
      });
    }
  </script>
</body>
</html>

```

## 🖼️ 미리보기 (스크린샷)

<img width="1278" alt="image" src="https://github.com/user-attachments/assets/a5db8661-11a8-4357-95ca-1bd4c0457ea7" />

-음식점, 관광치 추천 페이지도 이와 같이 구현

<img width="1277" alt="image" src="https://github.com/user-attachments/assets/24b0dd01-ed8b-4460-99a7-30016eae978e" />

<img width="1280" alt="image" src="https://github.com/user-attachments/assets/1066b9af-9d80-48cf-8e8d-022865082648" />


# 📅 나만의 여행 일정 만들기 - 페이지 설명서

## 📌 개요
사용자가 버튼을 눌러 여행 일정을 일(day) 단위로 추가하고, 각 날짜에 계획을 입력하고 삭제할 수 있는 직관적인 일정 작성 웹페이지입니다.

---

## 🛠 사용 기술

| 기술 | 설명 |
|------|------|
| **HTML5** | 페이지의 기본 구조 작성 |
| **CSS3** | 따뜻한 톤의 컬러 스타일링, 카드 UI, 버튼 효과 등 시각적 표현 |
| **Vanilla JavaScript** | 동적 일정 섹션 추가, 계획 항목 입력 및 삭제 처리 |

---

## ⚙ 주요 기능

- ➕ **하루 일정 추가**  
  "+ 하루 일정 추가" 버튼 클릭 시 새로운 `Day N` 일정 박스 생성

- 📝 **계획 항목 추가**  
  입력창에 계획을 입력하고 "추가" 버튼 클릭 시 리스트에 항목 추가  
  예: `오후 2시 — 루브르 박물관 방문`

- ❌ **계획 항목 삭제**  
  리스트 우측의 "❌" 버튼 클릭 시 해당 일정 항목 제거

- 📦 **동적 요소 생성**  
  각 일자마다 고유한 `id`로 계획 리스트 및 입력 필드 자동 생성

- 🍀 **아이콘 포인트**  
  각 일정 항목 앞에 "🍀" 아이콘 자동 표시

- 📱 **반응형 디자인 요소 적용**  
  버튼과 박스 간 여백, 폰트 크기 및 박스 스타일 등을 고려한 레이아웃 구성

---

## 🎨 UI 스타일링 요약

- 따뜻한 오렌지톤 (`#ffab91`, `#ff7043`, `#ffe0b2`) 배경색과 그림자 효과
- 각 일자 박스는 `dashed border + rounded corners + shadow`
- 일정 항목은 리스트 형식 + 삭제 버튼
- 입력 필드와 버튼은 정렬된 `flex` 박스로 구성

---

## 🔧 확장 아이디어

- 일정 저장/불러오기 기능 (LocalStorage 또는 서버)
- 시간 선택 기능 추가 (시간 선택 위젯)
- 드래그 앤 드롭으로 순서 변경 가능하도록 구현
- 하루 계획에 이미지나 장소 지도 삽입
- PDF 또는 인쇄 기능 추가

---

## 📁 연관된 다른 페이지

| 페이지 | 설명 |
|--------|------|
| `11.html` | 메인 페이지 |
| `flights.html` | 항공권 추천 |
| `hotel.html` | 호텔 추천 |
| `food.html` | 음식점 추천 |
| `attractions.html` | 관광지 추천 |
| `planner.html` | **현재 페이지 (일정 작성)** |

## 최종 작성 코드 
```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>📅 나만의 여행 일정 만들기</title>
  <style>
    body {
      margin: 0;
      font-family: 'Noto Sans KR', sans-serif;
      background-color: #fff8f0;
    }

    header {
      background-color: #ffab91;
      color: #fff;
      text-align: center;
      padding: 1.5rem;
      font-size: 2rem;
      font-weight: bold;
      border-bottom: 4px dashed #ff7043;
    }

    nav {
      display: flex;
      justify-content: center;
      gap: 1.5rem;
      padding: 1rem;
      background-color: #ffe0b2;
      flex-wrap: wrap;
    }

    nav a {
      text-decoration: none;
      color: #444;
      font-weight: 600;
      padding: 0.5rem 1rem;
      background-color: #ffffff;
      border-radius: 1.2rem;
      transition: 0.2s;
    }

    nav a:hover {
      background-color: #ffd180;
    }

    main {
      max-width: 800px;
      margin: auto;
      padding: 2rem;
    }

    .day-section {
      background-color: #fff;
      border: 2px dashed #ffab91;
      border-radius: 1rem;
      padding: 1.5rem;
      margin-bottom: 2rem;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.05);
      position: relative;
    }

    .day-section h2 {
      color: #ff7043;
      margin-bottom: 1rem;
    }

    .plan-list {
      list-style: none;
      padding-left: 1rem;
    }

    .plan-list li {
      margin-bottom: 0.7rem;
      position: relative;
      padding-left: 1.2rem;
    }

    .plan-list li::before {
      content: "🍀";
      position: absolute;
      left: 0;
      top: 0;
    }

    .plan-list li button {
      background: none;
      border: none;
      color: #ff7043;
      font-size: 1.2rem;
      position: absolute;
      right: 0;
      top: 0;
      cursor: pointer;
    }

    .add-plan {
      display: flex;
      gap: 0.5rem;
      margin-top: 1rem;
    }

    .add-plan input {
      flex: 1;
      padding: 0.5rem;
      border: 1px solid #ccc;
      border-radius: 0.5rem;
    }

    .add-plan button {
      padding: 0.5rem 1rem;
      background-color: #ffccbc;
      border: none;
      border-radius: 0.5rem;
      cursor: pointer;
      font-weight: bold;
    }

    .add-plan button:hover {
      background-color: #ffab91;
    }

    .add-day-btn {
      display: block;
      margin: 2rem auto;
      padding: 0.8rem 2rem;
      background-color: #ff7043;
      color: white;
      font-size: 1rem;
      border: none;
      border-radius: 2rem;
      cursor: pointer;
    }

    .add-day-btn:hover {
      background-color: #f4511e;
    }
  </style>
</head>
<body>
  <header>📅여행 planner</header>
  <nav>
    <a href="11.html">🏠 메인</a>
    <a href="flights.html">✈️ 항공권</a>
    <a href="hotel.html">🏨 호텔</a>
    <a href="food.html">🍜 음식점</a>
    <a href="attractions.html">🗺 관광지</a>
    <a href="planner.html">📅 일정</a>
  </nav>

  <main>
    <div id="planContainer"></div>
    <button class="add-day-btn" onclick="addDay()">+ 하루 일정 추가</button>
  </main>

  <script>
    let dayCount = 0;

    function addDay() {
      dayCount++;
      const container = document.getElementById("planContainer");

      const section = document.createElement("div");
      section.className = "day-section";
      section.innerHTML = `
        <h2>Day ${dayCount}</h2>
        <ul class="plan-list" id="list${dayCount}"></ul>
        <div class="add-plan">
          <input type="text" id="input${dayCount}" placeholder="예: 오전 10시 — 에펠탑 방문" />
          <button onclick="addPlan(${dayCount})">추가</button>
        </div>
      `;
      container.appendChild(section);
    }

    function addPlan(day) {
      const input = document.getElementById(`input${day}`);
      const value = input.value.trim();
      if (value === "") return;

      const list = document.getElementById(`list${day}`);
      const li = document.createElement("li");
      li.textContent = value;

      const deleteBtn = document.createElement("button");
      deleteBtn.textContent = "❌";
      deleteBtn.onclick = function() {
        list.removeChild(li);
      };

      li.appendChild(deleteBtn);
      list.appendChild(li);

      input.value = "";
    }
  </script>
</body>
</html>

```
## 🖼️ 미리보기 (스크린샷)
<img width="1280" alt="image" src="https://github.com/user-attachments/assets/0c13d3b6-e177-49e9-95db-2283bebceecd" />

<img width="1280" alt="image" src="https://github.com/user-attachments/assets/fe266162-e849-4a8a-9601-4b08fc047a7b" />

<img width="1278" alt="image" src="https://github.com/user-attachments/assets/84e67239-a362-4ec5-921a-2237e1b98e90" />


# 🎒 여행 성향 테스트 - 페이지 설명서

## 📌 개요
사용자가 간단한 질문 3개에 답하면, 그에 따라 여행 성향을 알려주는 인터랙티브한 테스트 페이지입니다. 사용자의 선택에 따라 다른 결과 메시지를 보여줍니다.

---

## 🛠 사용 기술

| 기술 | 설명 |
|------|------|
| **HTML5** | 기본 구조와 콘텐츠 구성 |
| **CSS3** | 색상, 레이아웃, 버튼 스타일 등 UI 구성 |
| **Vanilla JavaScript** | 질문 흐름 제어, 결과 출력, DOM 조작 처리 |

---

## ⚙ 주요 기능

- ❓ **3단계 질문 테스트 진행**
  - 질문 1: 여행에서 중요하게 생각하는 것
  - 질문 2: 선호하는 여행 스타일
  - 질문 3: 기대하는 여행 활동

- 🔁 **선택에 따른 동적 질문 전환**
  - 각 질문은 사용자의 클릭에 따라 다음 질문으로 자동 진행
  - JavaScript로 질문과 버튼 내용을 실시간 변경

- 📊 **최종 결과 제공**
  - 마지막 질문에 대한 답변을 바탕으로 여행 성향 결과 출력
  - 성향에 맞는 여행 스타일 및 추천 방향 안내

- 🏠 **메인 페이지 이동 버튼**
  - 테스트 결과 화면에서 메인 페이지(`11.html`)로 돌아가는 버튼 제공

---

## 🎨 UI 스타일링 요약

- 오렌지 계열의 톤 (`#ff7043`, `#ffab91`)으로 따뜻한 분위기 연출
- `.container`에 `box-shadow`와 `border-radius`를 적용한 카드형 디자인
- 버튼에 `hover` 효과와 부드러운 색상 전환 구현
- 결과창 `.result`는 초기엔 숨겨두고 필요 시 `display: block` 처리

---

## 🔧 확장 아이디어

- 각 질문 선택을 기억해 종합적인 성향 결과 제공
- 결과에 따라 여행지 이미지/링크/추천 상품 제공
- SNS 공유 기능 (카카오톡, 인스타그램 등)
- MBTI 기반 성향 매칭 테스트로 확장

---

## 📁 연관된 다른 페이지

| 페이지 | 설명 |
|--------|------|
| `11.html` | 메인 페이지 |
| `flights.html` | 항공권 추천 |
| `hotel.html` | 호텔 추천 |
| `food.html` | 음식점 추천 |
| `attractions.html` | 관광지 추천 |
| `planner.html` | 일정 작성 |
| `test.html` | **현재 페이지 (성향 테스트)** |

## 최종 작성 코드 
```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>🎒 여행 성향 테스트</title>
  <style>
    body {
      margin: 0;
      font-family: 'Noto Sans KR', sans-serif;
      background-color: #fff8f0;
      text-align: center;
    }

    header {
      background-color: #ff7043;
      color: white;
      padding: 1.5rem;
      font-size: 2rem;
      font-weight: bold;
    }

    .container {
      max-width: 700px;
      margin: 2rem auto;
      padding: 2rem;
      background-color: #ffffff;
      border-radius: 1rem;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
    }

    h2 {
      color: #ff7043;
      font-size: 1.8rem;
      margin-bottom: 1rem;
    }

    .question {
      font-size: 1.3rem;
      margin: 2rem 0;
    }

    .answers {
      display: flex;
      flex-direction: column;
      gap: 1rem;
    }

    .answer-btn {
      padding: 0.8rem;
      background-color: #ffab91;
      color: white;
      border: none;
      border-radius: 0.8rem;
      cursor: pointer;
      font-weight: bold;
      transition: 0.3s;
    }

    .answer-btn:hover {
      background-color: #ff7043;
    }

    .result {
      display: none;
      margin-top: 2rem;
      padding: 1.5rem;
      background-color: #ffefd5;
      border-radius: 1rem;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.05);
    }

    .result h3 {
      color: #ff7043;
    }

    .go-home-btn {
      background-color: #ff7043;
      color: white;
      padding: 1rem 2rem;
      border-radius: 1.5rem;
      cursor: pointer;
      font-weight: bold;
      transition: 0.3s;
      margin-top: 2rem;
    }

    .go-home-btn:hover {
      background-color: #ff7043;
    }
  </style>
</head>
<body>
  <header>🎒 나의 여행 성향을 알아보자!</header>
  <div class="container">
    <h2>여행 성향 테스트</h2>
    <div class="question" id="question">
      <p>1. 여행에서 가장 중요하게 생각하는 것은?</p>
    </div>
    <div class="answers">
      <button class="answer-btn" onclick="nextQuestion(0)">자연과 경치</button>
      <button class="answer-btn" onclick="nextQuestion(1)">맛있는 음식</button>
      <button class="answer-btn" onclick="nextQuestion(2)">문화와 역사</button>
      <button class="answer-btn" onclick="nextQuestion(3)">액티비티와 모험</button>
    </div>
    <div class="result" id="result">
      <h3>당신의 여행 성향은:</h3>
      <p id="result-text"></p>
      <!-- 메인 페이지로 돌아가기 버튼 추가 -->
      <button class="go-home-btn" onclick="goToMainPage()">메인 화면으로 돌아가기</button>
    </div>
  </div>

<script>
  let currentQuestion = 0;
  const questions = [
    {
      question: "1.여행에서 가장 중요하게 생각하는 것은?",
      answers: ["자연과 경치", "맛있는 음식", "문화와 역사", "액티비티와 모험"],
      results: [
        "자연을 사랑하는 당신! 자연과 경치가 중요한 여행지로 추천드립니다.",
        "먹거리가 중요한 당신! 미식 여행을 즐길 수 있는 장소를 추천드립니다.",
        "문화와 역사를 탐구하는 당신! 역사적이고 문화적인 명소를 추천드립니다.",
        "모험을 좋아하는 당신! 액티비티가 가득한 여행지를 추천드립니다."
      ]
    },
    {
      question: "2.어떤 스타일의 여행을 선호하시나요?",
      answers: ["혼자 여행", "가족 여행", "친구들과의 여행", "로맨틱한 여행"],
      results: [
        "혼자만의 시간을 즐기고 싶다면, 고요한 자연 속 여행을 추천드립니다.",
        "가족과 함께라면, 편안하고 안전한 여행지를 추천드립니다.",
        "친구들과 함께라면, 액티비티가 가득한 여행지를 추천드립니다.",
        "로맨틱한 여행을 원하신다면, 아름다운 풍경과 고즈넉한 여행지를 추천드립니다."
      ]
    },
    {
      question: "3.여행지에서 가장 기대되는 활동은?",
      answers: ["하이킹", "쇼핑", "박물관 탐방", "수상 스포츠"],
      results: [
        "자연 속에서 하이킹을 즐기고 싶다면, 등산로가 아름다운 곳을 추천드립니다.",
        "쇼핑을 좋아하신다면, 트렌디한 도시를 추천드립니다.",
        "문화 탐방을 좋아하신다면, 역사적인 도시와 박물관을 추천드립니다.",
        "수상 스포츠를 즐기고 싶다면, 바다가 아름다운 여행지를 추천드립니다."
      ]
    }
  ];

  function nextQuestion(answer) {
    const resultText = document.getElementById("result-text");
    const resultDiv = document.getElementById("result");
    const questionDiv = document.getElementById("question");

    currentQuestion++;

    if (currentQuestion >= questions.length) {
      resultText.textContent = questions[currentQuestion - 1].results[answer];
      questionDiv.style.display = "none";
      resultDiv.style.display = "block"; // 결과 출력
    } else {
      updateQuestion(currentQuestion);
    }
  }

  function updateQuestion(index) {
    const questionDiv = document.getElementById("question");
    const answersDiv = document.querySelector(".answers");

    questionDiv.innerHTML = `<p>${questions[index].question}</p>`;
    answersDiv.innerHTML = "";
    questions[index].answers.forEach((answer, i) => {
      const button = document.createElement("button");
      button.classList.add("answer-btn");
      button.textContent = answer;
      button.onclick = () => nextQuestion(i);
      answersDiv.appendChild(button);
    });
  }

  function goToMainPage() {
    window.location.href = '11.html';  // 메인 페이지로 이동
  }
</script>
</body>
</html>

```


## 🖼️ 미리보기 (스크린샷)

<img width="1280" alt="image" src="https://github.com/user-attachments/assets/63ca0e7f-6d54-4176-beaf-81282cff5596" />

<img width="1280" alt="image" src="https://github.com/user-attachments/assets/6d50f75a-acf1-4c7a-8b74-18dd7c593115" />



