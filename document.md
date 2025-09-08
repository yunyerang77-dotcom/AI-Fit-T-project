## 간단한 투두리스트 웹앱 기술 명세서 (Beginner Friendly)

이 문서는 HTML/CSS/JS만으로 만든 단일 파일 투두리스트(`index.html`)의 구조와 동작 방식을 바이브코딩 입문자도 이해하기 쉽게 설명합니다.

---

### 1) 프로젝트 개요
- **목표**: 입력창에 할 일을 적고 "추가" 버튼(또는 Enter)으로 리스트에 항목을 추가하는 간단한 앱
- **기능**:
  - 항목 추가
  - 체크박스로 완료 표시(완료 시 텍스트에 취소선)
  - 항목 삭제
  - 모바일 친화적(가운데 정렬, 터치에 적합한 크기)

---

### 2) 파일 구조
- `index.html`: HTML, CSS, JavaScript가 한 파일에 모두 포함된 실행 파일

---

### 3) 실행 방법
1. 폴더에서 `index.html`을 더블 클릭하여 브라우저로 열기
2. 또는 브라우저 창에 `index.html` 파일을 드래그 앤 드롭

추가 설치가 전혀 필요 없습니다.

---

### 4) 화면 구성(HTML)
- 상단 입력 폼: `#todo-form`
  - 텍스트 입력: `#todo-input`
  - 추가 버튼: `#add-btn`
- 리스트 영역: `#todo-list`

예시 (일부):
```html
<form id="todo-form" class="input-row" autocomplete="off">
  <input id="todo-input" class="input" type="text" placeholder="할 일을 입력..." />
  <button id="add-btn" class="btn" type="submit">추가</button>
  
</form>
<ul id="todo-list" class="list"></ul>
```

---

### 5) 스타일 가이드(CSS)
- 모바일에서도 보기 좋게 전체를 **가운데 정렬**
- 카드 형태 컨테이너와 부드러운 그림자
- 버튼은 심플한 **파란색(Primary)** 톤, 누름 효과 포함
- 완료된 항목에는 `.text.completed` 클래스 → **취소선 + 흐린 색**

핵심 클래스:
- `.container`, `.card`: 레이아웃
- `.input-row`, `.input`, `.btn`: 입력/버튼
- `.list`, `.item`: 리스트/항목
- `.text.completed`: 완료 표시 스타일

---

### 6) 동작 원리(JavaScript)
모든 스크립트는 `index.html` 하단 `<script>`에 포함되어 있습니다.

- 폼 제출 이벤트로 항목 추가
- 각 항목은 체크박스(완료 토글)와 삭제 버튼을 가짐

핵심 흐름(요약 코드):
```js
const form = document.getElementById('todo-form');
const input = document.getElementById('todo-input');
const list = document.getElementById('todo-list');

function createItem(text) {
  const li = document.createElement('li');
  li.className = 'item';
  const checkbox = document.createElement('input');
  checkbox.type = 'checkbox';
  checkbox.className = 'checkbox';
  const span = document.createElement('span');
  span.className = 'text';
  span.textContent = text;
  const delBtn = document.createElement('button');
  delBtn.className = 'delete-btn';

  checkbox.addEventListener('change', () => {
    span.classList.toggle('completed', checkbox.checked);
  });
  delBtn.addEventListener('click', () => li.remove());

  li.append(checkbox, span, delBtn);
  return li;
}

function addTodo(text) {
  const trimmed = text.trim();
  if (!trimmed) return;
  list.appendChild(createItem(trimmed));
}

form.addEventListener('submit', (e) => {
  e.preventDefault();
  addTodo(input.value);
  input.value = '';
  input.focus();
});
```

포인트:
- `form`의 기본 제출을 막고(`e.preventDefault()`), 입력값을 읽어 항목 생성
- 체크박스 `change` 이벤트로 완료 상태를 토글 → 스타일 클래스 변경
- 삭제 버튼 클릭 시 해당 항목을 DOM에서 제거

---

### 7) 접근성/사용성 처리
- 스크린 리더용 레이블(`aria-label`, `sr-only` 클래스) 적용
- 키보드 사용자를 위해 Enter로 즉시 추가 가능
- 체크박스는 기본 OS 접근성 지원을 활용

---

### 8) 커스터마이징 가이드
- 기본 색상 변경: CSS `:root`의 색상 변수(`--primary`, `--bg` 등) 수정
- 초기 예시 항목 제거: 스크립트의 `samples` 배열을 삭제하거나 주석 처리
- 항목 구조 수정: `createItem()` 함수에서 요소 구성 변경

예: 초기 예시 제거
```js
// const samples = ['예: 운동하기', '예: 우유 사기', '예: 공부 30분'];
// for (const s of samples) addTodo(s);
```

---

### 9) 자주 묻는 질문(FAQ)
- Q. 새로고침하면 리스트가 사라져요.
  - A. 현재는 브라우저 저장소를 사용하지 않습니다. 간단함을 위해 메모리에만 보관합니다. 원한다면 `localStorage`를 이용해 저장 기능을 쉽게 추가할 수 있습니다.
- Q. 입력이 비어 있을 때 추가를 누르면?
  - A. `trim()` 후 빈 문자열이면 무시하도록 처리했습니다.

---

### 10) 다음 단계(확장 아이디어)
- `localStorage`에 리스트 저장/불러오기
- 항목 편집 기능(더블 클릭 → 내용 수정)
- 필터(전체/미완료/완료)
- 드래그 앤 드롭 정렬

---

### 11) 요약
- 단일 `index.html`로 실행되는 가벼운 투두 앱
- 추가/완료/삭제의 기본 기능과 모바일 친화적 UI 제공
- 초보자도 구조를 쉽게 이해하고 원하는 부분만 수정 가능


