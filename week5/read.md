# Week5 웹사이트 부족한 부분 보안, 개선

## 지역별화면에서 메인 화면으로 가는 버튼 생성
-추가 된 코드

```css
.main-button {
  position: fixed;
  top: 1rem;
  left: 1rem;
  padding: 0.75rem;
  background-color: #fff;
  color: #333;
  text-align: center;
  font-size: 1.2rem;
  border-radius: 0.5rem;
  text-decoration: none;
  transition: background-color 0.3s ease;
  z-index: 100;
}

.main-button:hover {
  background-color: #f0f0f0;
  color: #000;
}

<a href="index.html" class="main-button">메인 화면으로 돌아가기</a>

```

## 검색 기능 추가
사용자가 특정 맛집이나 지역을 더 빠르게 찾을 수 있도록 검색 기능을 추가하는 것이 좋습니다. 예를 들어, 사이트의 상단에 검색 바를 두어 사용자가 키워드로 맛집을 검색할 수 있게 하면, 방문자가 보다 쉽게 원하는 정보를 찾을 수 있습니다 
