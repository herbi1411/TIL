# HTML / CSS

# 웹사이트

---

- 브라우저를 통해 접속하는 웹페이지(문서)들의 모음
- 글, 그림, 동영상 등 여러 정보를 담고 있으며, 마우스로 클릭하면 다른 웹 페이지로 이동하는 ‘링크’들이 있음

## 웹사이트의 구성 요소

---

- HTML → 구조 (건물)
- CSS → 표현 (인테리어)
- JavaScript → 동작 (엘리베이터)

## 브라우저

---

- 웹사이트는 브라우저를 통해 동작함
- **`deprecated` : 중요도가 떨어져 더이상 사용하지 않는 것 (ex. Explorer)**
- 초기에는 브라우저마다 동작이 약간 달라서 문제가 생기는 경우가 많았음(파편화)
- 해결책으로 `웹 표준`이 등장

## 웹표준

---

- W3C
    - 웹 표준을 만드는 회사
- 어떤 브라우저든 웹페이지가 동일하게 보이도록 함 (`크로스브라우징`)
- 실행기(javascript 런타임) + 문서(document) 두개 모두 표준 지켜야함!!
- HTML 요소를 브라우저에 활용할 수 있는 지 확인할 수 있는 사이트
    
    [Can I use... Support tables for HTML5, CSS3, etc](https://caniuse.com/)
    

## Hyper Text

---

- 참조(하이퍼링크)를 통해 사용자가 한 문서에서 다른 문서로 즉시 접근할 수 있는 텍스트

## MarkUp Language

---

- 태그를 이용하여 문서나 데이터의 구조를 명시하는 언어
- ex) HTML, Markdown
- 마크업 스타일 가이드 → `2 space`

## HTML 기본 구조

---

- **html** : 문서의 최상(root) 요소
- **head**: 문서의 메타데이터 요소
    - 문서제목, 인코딩, 스타일, 외부 파일 로딩 등
    - 일반적으로 브라우저에 나타나지 않는 내용
    - ex) title, meta, link, script, style
- **body**: 문서 본문 요소
    - 실제 화면 구성과 관련된 내용

## Open Graph Protocol

---

- 메타 데이터를 표현하는 새로운 규약
    - HTML 문서의 메타 데이터를 통해 문서의 정보를 전달
    - 메타정보에 해당하는 제목, 설명 등을 쓸 수 있도록 정의
    - ex) 썸네일
    - 노션에 링크 북마크 불러올때 불러지는 정보

## 요소

---

- HTML 요소는 시작 태그와 종료 태그 그리고 태그 사이에 위치한 내용으로 구성
- 내용이 없는 태그들도 존재(닫는 태그가 없음)
    - ex) br, hr, img, input, link, meta
- 요소는 중첩(nested)될 수 있음
- 여는 태그와 닫는 태그의 쌍을 잘 확인해야함
    - 태그 쌍이 맞지 않더라도 브라우저는 에러를 표출하지 않음

## 속성

---

- 태그안에 쓰는 것 (ex. href)
- 태그마다 사용할 수 있는 속성이 다르다.
- 속성을 사용할 때는 공백을 넣으면 안된다. (ex \<a href=”https://www.naver.com”/\>)
- global attribute
    - id : 문서 전체에서 유일한 고유 식별자 지정
    - class: 공백으로 구분된 해당 요소의 클래스 목록(CSS, JS에서 요소를 선택하거나 접근)
    - data-* : 페이지에 개인 사용자 정의 데이터를 저장하기 위해 사용
    - style: inline 스타일
    - title: 요소에 대한 추가 정보 지정

## 시맨틱 태그

---

- html 태그가 특정 목적, 역할 및 의미적가치(semantic value)를 가지는 것
- 예를 들어 `h1`은 `최상위 제목` 인 텍스트를 감싸는 역할(또는 의미)
- Non semantic 태그로는 `div, span` 등이 있으며, `a, from, table` 도 시맨틱 태그로 볼 수 있음
- `검색엔진최적화(SEO)`를 위해 메타태그, 시맨틱 태그 등을 통한 마크업을 효과적으로 활용 해야함

## 렌더링

---

- 웹사이트 코드를 사용자가 보게 되는 웹 사이트로 바꾸는 과정
- DOM트리 → 텍스트 파일인 HTML 문서를 브라우저에 렌더링 하기 위한 구조

## HTML 문서 구조화

---

- HTML요소는 인라인/ 블록요소로 나눔
- `인라인` 요소는 글자처럼 취급
- `블록` 요소는 한 줄 모두 사용

## form

---

- 정보(데이터)를 서버에 제출하기 위해 사용하는 태그
- 기본 속성
    - action: url
    - method: form을 제출할 때 사용하는 HTTP메서드 (GET 또는 POST)
    - enctype: method가 post인 경우에 데이터의 유형

## CSS(Cascading Style Sheet)

---

- 선택자를 통해 element를 선택하고, 스타일을 지정한다.
- 속성(Property), 값(Value) 쌍으로 이루어짐

## CSS 정의 방법

---

- 인라인
- 내부 참조 ( `<style>`)
- 외부 참조(link file) → 분리된 css파일

## CSS 선택자

---

- 클래스선택자 (`.`)
- id선택자 (`#`)
- 자식결합자(`.box > p`) → 딱 자식만
- 자손결합자(`.box p`) → 모든 자손

## CSS 상속

---

- 속성이 상속되는 것이 있음
    - text 관련 요소 → font, color, text-align, opacity, visibility
    - 상속되지 않는 요소(box) → margin

## 글자 크기 단위

---

- px
- %
- 상대단위
    - em: 바로 위 부모 요소에 대한 상대크기
    - rem: 최상위 부모요소(html)에 대한 상대크기
- viewport
    - 현재 창 크기에 대한 상대크기

## CSS 원칙

---

- 모든 요소는 BOX다.
    - 모든 요소는 왼쪽→오른쪽, 위→아래로 쌓인다.

## Content-box 🆚 border-box

---

- content-box : 콘텐츠 만의 크기
- border-box : 경계선까지 포함한 크기
- CSS는 기본적으로 Content-box

## display

---

- block → 화면 전체 크기의 가로 폭 차지
    - 블록레벨 요소 안에 인라인 레벨 요소 들어갈 수 있음
- inline → content 너비 만큼 폭 차지 (**글자처럼취급**)
    - width, height, margin 등을 지정할 수 없다
    - 상하여백은 line-height로 지정
- None 🆚 Hidden
    - None은 아예 안보이고 공간차지 안함
    - hidden은 안보이지만 공간차지
    

## CSS Position

---

- Relative
    - 상대위치 (원래 있어야 할 위치(normal flow)에서의 위치)
    - 위치차지⭕
- Absolute
    - 절대위치 (static이 아닌(Relative인) 첫번째 조상에서의 기준으로 위치 (못만나면 브라우저 기준))
    - 하늘로 붕 뜸
    - 위치차지❌
- fixed
    - 고정위치 → Viewport 기준의 위치
- sticky
    - fixed 였다가 Relative로 바뀜

---

---

---

## CSS Layout techniques

---

- display
- position
- float
- flexbox
- grid

## float

---

- 요즘은 잘 안쓰임

## Flex box

---

- IE에서는 지원 제대로 안됨
- 축
    - main axis : 컨텐츠가 쌓이는 방향
    - cross axis: 반대 방향
- 구성요소
    - Flex Container (부모 요소)
    - Flex Item (자식 요소)
- 배치설정
    - flex-direction
    - flex-wrap
- 공간 나누기
    - justify-content (main axis)
    - align-content (cross axis)
- 정렬
    - align-items(모든 아이템을 cross axis 기준으로)
    - align-self (개별 아이템)
    

## BootStrap

---

- CSS 스타일, 템플릿을 미리 지정해서 사용할 수 있게 해줌
- 모든 CSS설정이 important로 돼있어서 사용자가 고쳐쓸 수 없음
- class 가져다 쓸 때 bootstrap에 정의된 이름이랑 안겹치게 주의해야함⚠️

## BootStrap 사용 방법

---

- Spacing
    - `{property}{sides}-{size}`
        - ex) class가  mt-3 → margin top 3
        - size는 0~5까지 있음 (1 → 0.25rem)
- 자주 쓰이는 클래스

```html
<h2>Spacing</h2>
  <div class="mt-3 ms-5 box">margin top3 ms-5</div>
  <div class="m-4 box">margin 4</div>
  <div class="mx-auto box">가운데정렬</div>
  <div class="ms-auto box">ms-auto 오른쪽 정렬</div>
  
  <hr>
  
  <h2>Color</h2>
  <div class="bg-primary">이건 파랑</div>
  <div class="bg-secondary">이건 회색</div>
  <div class="bg-danger">이건 빨강</div>
  <div class="text-success">이건 초록색</div>
  <div class="text-danger">이건 빨강</div>

  <hr>

  <p class="text-start">text-start</p>
  <p class="text-center">text-center</p>
  <p class="text-end">text-end</p>
  <a href="#" class="text-decoration-none text-dark">None-Underline-Link</a>
  <p class="fw-bold">Bold text</p>
  <p class="fw-normal">Normal Weight text</p>
  <p class="fst-italic">Italic Text</p>
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0/dist/js/bootstrap.bundle.min.js" integrity="sha384-A3rJD856KowSb7dwlZdYEkO39Gagi7vIsF0jrRAoQmDKKtQBHUuLZ9AsSv4jD4Xa" crossorigin="anonymous"></script>
  
  <hr>
  <h2>Position</h2>
  <div class="box fixed-top">fixed-top</div>
  <div class="box fixed-bottom">fixed-bottom</div>
  <div class="bigbox position-relative">
    <div class="box position-absolute top-0 start-0">top 0 and start 0</div>
    <div class="box position-absolute top-0 end-0">top 0 and end 0</div>
    <div class="box position-absolute bottom-0 start-0">bottom 0 and start 0</div>
    <div class="box position-absolute bottom-0 end-0">bottom 0 and end 0</div>
  </div>
  <hr>
  <h2>Display</h2>
  <div class="d-inline p-2 text-bg-primary">d-inline</div>
  <div class="d-inline p-2 text-bg-dark">d-inline</div>
  <div class="d-none p-2 text-bg-dark">d-none</div>
  <div class="box bg-warning d-sm-none d-md block">보이냐? 안보이냐??</div>
  <div class="box bg-success d-md-none d-xl block">보이냐? 안보이냐??</div>
```

## Grid 시스템

---

- 반응형 웹을 만들기 위한 디자인
- 기본 요소
    - Column: 실제 컨텐츠를 포함하는 부분
    - Gutter: 컬럼과 컬럼 사이의 공간(간격)
    - Container: Column들을 담고 있는 공간