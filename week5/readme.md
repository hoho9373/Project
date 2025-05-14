# week5 🌍 여행 웹사이트 개선 제안 (기능, 디자인, 코드 관점)

## 1. 데이터 & 기능 확장

| 제안 | 왜 좋은가 | 힌트 |
|------|-----------|------|
| ✅ **여행 일정 로컬 저장/불러오기** | 새로고침·재방문 시 일정 보존 | `localStorage` 사용, 추후 Firebase Firestore 확장 |
# 수정된 코드
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

    // 새로고침 시 저장된 일정 불러오기
    window.onload = function() {
      const savedPlans = JSON.parse(localStorage.getItem('travelPlans'));
      if (savedPlans) {
        dayCount = savedPlans.length;
        savedPlans.forEach((dayPlan, index) => {
          addDay(dayPlan);
        });
      }
    };

    function addDay(savedPlan = null) {
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

      // 이전에 저장된 계획이 있다면 로드
      if (savedPlan) {
        savedPlan.forEach(plan => addPlanToList(dayCount, plan));
      }
    }

    function addPlan(day) {
      const input = document.getElementById(`input${day}`);
      const value = input.value.trim();
      if (value === "") return;

      addPlanToList(day, value);
      input.value = "";

      // 계획을 로컬 스토리지에 저장
      savePlans();
    }

    function addPlanToList(day, plan) {
      const list = document.getElementById(`list${day}`);
      const li = document.createElement("li");
      li.textContent = plan;

      const deleteBtn = document.createElement("button");
      deleteBtn.textContent = "❌";
      deleteBtn.onclick = function() {
        list.removeChild(li);
        savePlans();
      };

      li.appendChild(deleteBtn);
      list.appendChild(li);
    }

    // 일정을 localStorage에 저장하는 함수
    function savePlans() {
      const plans = [];
      for (let i = 1; i <= dayCount; i++) {
        const dayPlan = [];
        const list = document.getElementById(`list${i}`);
        const items = list.getElementsByTagName("li");
        for (let item of items) {
          dayPlan.push(item.textContent.replace("❌", "").trim());
        }
        plans.push(dayPlan);
      }
      localStorage.setItem('travelPlans', JSON.stringify(plans));
    }
  </script>
</body>
</html>

---

## 2. 회원 정보를 저장할 수 있는 로그인, 회원가입란 만들기

#수정된 코드
```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>로그인 - GoVoyage</title>
  <style>
    body {
      font-family: 'Noto Sans KR', sans-serif;
      background-color: #fff9f5;
      margin: 0;
      padding: 0;
    }
    .container {
      max-width: 400px;
      margin: 5rem auto;
      background-color: #fff;
      padding: 2rem;
      border-radius: 1rem;
      box-shadow: 0 4px 10px rgba(0,0,0,0.05);
    }
    h1 {
      text-align: center;
      color: #ff7043;
      margin-bottom: 2rem;
    }
    label {
      display: block;
      margin-bottom: 0.5rem;
      font-weight: 600;
    }
    input {
      width: 100%;
      padding: 0.8rem;
      margin-bottom: 1.5rem;
      border: 1px solid #ccc;
      border-radius: 0.5rem;
    }
    button {
      width: 100%;
      padding: 0.8rem;
      background-color: #ff7043;
      color: #fff;
      font-weight: bold;
      border: none;
      border-radius: 0.5rem;
      cursor: pointer;
    }
    button:hover {
      background-color: #f4511e;
    }
    .link {
      text-align: center;
      margin-top: 1rem;
    }
    .link a {
      color: #ff7043;
      text-decoration: none;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>로그인</h1>
    <form>
      <label for="email">이메일</label>
      <input type="email" id="email" placeholder="you@example.com" required />
      
      <label for="password">비밀번호</label>
      <input type="password" id="password" placeholder="••••••••" required />
      
      <button type="submit">로그인</button>
    </form>
    <div class="link">
      아직 회원이 아니신가요? <a href="signup.html">회원가입</a>
    </div>
  </div>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>회원가입 - GoVoyage</title>
  <style>
    body {
      font-family: 'Noto Sans KR', sans-serif;
      background-color: #fff9f5;
      margin: 0;
      padding: 0;
    }
    .container {
      max-width: 400px;
      margin: 5rem auto;
      background-color: #fff;
      padding: 2rem;
      border-radius: 1rem;
      box-shadow: 0 4px 10px rgba(0,0,0,0.05);
    }
    h1 {
      text-align: center;
      color: #ff7043;
      margin-bottom: 2rem;
    }
    label {
      display: block;
      margin-bottom: 0.5rem;
      font-weight: 600;
    }
    input {
      width: 100%;
      padding: 0.8rem;
      margin-bottom: 1.5rem;
      border: 1px solid #ccc;
      border-radius: 0.5rem;
    }
    button {
      width: 100%;
      padding: 0.8rem;
      background-color: #ff7043;
      color: #fff;
      font-weight: bold;
      border: none;
      border-radius: 0.5rem;
      cursor: pointer;
    }
    button:hover {
      background-color: #f4511e;
    }
    .link {
      text-align: center;
      margin-top: 1rem;
    }
    .link a {
      color: #ff7043;
      text-decoration: none;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>회원가입</h1>
    <form>
      <label for="name">이름</label>
      <input type="text" id="name" placeholder="홍길동" required />
      
      <label for="email">이메일</label>
      <input type="email" id="email" placeholder="you@example.com" required />
      
      <label for="password">비밀번호</label>
      <input type="password" id="password" placeholder="••••••••" required />
      
      <button type="submit">회원가입</button>
    </form>
    <div class="link">
      이미 계정이 있으신가요? <a href="login.html">로그인</a>
    </div>
  </div>
</body>
</html>

```

## 구현 화면
<img width="1280" alt="image" src="https://github.com/user-attachments/assets/4e5a0e49-241f-4ee5-8750-b84432e006d9" />

<img width="1280" alt="image" src="https://github.com/user-attachments/assets/7ac3bddb-13a2-4b3b-b2d6-8183abd07add" />





