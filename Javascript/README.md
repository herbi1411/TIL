# JavaScript

## Var

---

- ES6 이전에 사용한 자료형 타입
- 호이스팅이 가능해서 현재는 안쓰임
- 호이스팅 : 변수를 선언 전에 사용가능

```jsx
console.log(nam) // undefined

var name = "윤민수"
```

## 변수 타입

---

- primitive타입 (int, string)
- object 타입 (array, function)

## Template Literal

---

- 파이썬의 f-string과 같이 문자열 안에 변수 삽입 가능
- ES6부터 사용
- ``를 통해 문자열 작성, `${}` 를 통해 변수 삽입

## undefiend 🆚 null

---

- 개발자의 설계실수(둘의 차이 없음)

null

- 의도적으로 값이 없음을 나타냄 → 개발자가 의도적 삽입
- null은 object 타입

undefined

- 값이 정의돼있지 않음을 표현 → 프로그램이 값이 안들어있을때 삽입

## 동등연산자 ( == )

---

- 다른 언어와 다르게 비교를 형변환을 한다음에 함
- 1 == ‘1’ → true
- 예상치 못한 결과가 나올 수 있으므로 특별한 경우를 제외하고 잘 사용하지 않음

## 일치연산자(===)

---

- 암묵적 형변환 X
- 같은 객체를 가리키거나 같은 타입이면서 같은 값인지 비교

## for…of

---

- 반복가능한 객체를 순회
- 반복가능한 객체 : Array, Set, String
- for .. in 은 속성 이름을 통해 반복
- for .. of 는 속성 값을 통해 반복
- for .. of는 object는 순회 X (최상위에 있는 object)
- for..of 에 사용하는 변수는 반복할 때마다 재할당을 해서 사용하는 것이므로 const 가능
- 일반 for문에서는 let 써야함

## 함수 선언 방법

---

- 선언식
    - 일반적인 선언 구조
    - 익명함수 사용 불가
    - 호이스팅 발생
        
        ```jsx
        add(2, 7) // 9
        
        function add (num1, num2) {
        	return num1 + num2
        }
        
        ```
        
- 표현식
    - 함수를 변수에 할당
    - 익명함수 사용 가능
    - 호이스팅 X
    
    ```jsx
    add(2, 7) // error
    
    function add (num1, num2) {
    	return num1 + num2
    }
    
    ```
    

※ 표현식 사용 권장

## Arrow Function

---

- 함수를 비교적 간결하게 정의할 수 있는 문법
- fucnction 키워드를 중괄호를 이용한 구문을 짧게 사용하기 위해 탄생
- 간결화 단계
    1. function 키워드 생략 가능
    2. 함수의 매개변수가 하나라면 () 생략 가능
    3. 함수의 내용이 한줄이라면 {} 와 return 생략 가능
    
    ```jsx
    const greeting = (name) -> `Hi ${name}`
    ```
    

## 즉시 실행 함수

---

- 함수 선언 끝에 `()`를 추가해서 선언되자마자 실행하는 형태
- 재호출 불가
- 일회성 함수이므로 익명함수로 사용하는 것이 일반적

## 배열

---

- 대괄호로 생성, 0을 포함한 양의 인덱스로 접근
- 길이는 array.length로 접근 가능
- 메서드
    
    

- .join은 구분자 생략시 쉼표가 default

## Array Helper Methods

---

- 배열을 순회하며 특정 로직을 수행하는 메서드
- 메서드 호출시 인자로 callback함수를 받는 것이 특징

`foreach 함수`

- 콜백함수가 3가지 매개변수로 구성
    - element : 배열의 요소
    - index : 배열 요소의 인덱스
    - array : 배열 자체
- 반환값 없음

`reduce 함수`

- sum을 구할떄 효과적 (add할 변수 하나를 더 받음)

## 객체(Object)

---

- key는 문자열만 가능 (공백있으면 따옴표로 묶어야 함)
- value는 모든 타입 가능(함수 포함)

객체 관련 문법

1. 속성명 축약
2. 메서드명 축약
3. 계산된 속성 → 키 이름을 변수로 하고 동적으로 변경 가능 (이때 object에 키 정의할 때는 [key] 형태로)
4. 구조분해할당 → { } 를통해서 한번에 할당 (ex. const {val, setVal} = useState())
5. Spread syntax(…) → 얕은 복사를 해서 객체 전부 옮길 수 있음 

## Json

---

object → json (string)

`JSON.stringify(v)`

json (string) → object

`JSON.parse(v)`

## DOM

---

- Browser APIs 의 한 부분
- Document Object Model (문서 객체 모델)
- 문서의 구조화된 표현을 제공, 프로그래밍 언어가 DOM 구조에 접근할 수 있는 방법을 제공
    - 문서 구조, 스타일, 내용등을 쉽게 변경할 수 있게 도움
    - HTML 콘텐츠를 추가, 제거, 변경, 동적으로 페이지에 스타일을 추가
    - HTML / CSS를 조작할 수 있음
- HTML 문서를 구조화해서 각 요소를 객체(Object)로 취급
- 단순한 속성 접근, 메서드 활용 뿐 아니라 프로그래밍 언어적 특성을 활용한 조작이 가능
- 문서를 논리 트리로 표현
- DOM 메서드를 사용하면 프로그래밍적으로 트리에 접근할 수 있고, 이를 통해 문서의 구조, 스타일, 컨텐츠를 변경할 수 있음
- 웹페이지는 일종의 문서(Document)
- 이 문서는 웹 브라우저를 통해 그 내용이 해석돼서 웹 브라우저 화면에 나타나거나 HTML 코드 자체로 나타나기도 함
- DOM은 동일한 문서를 표현하고, 저장하고, 조작하는 방법을 제공
- DOM은 웹 페이지의 객체 지향 표현이며, JavaScript와 같은 스크립트 언어를 이용해 DOM을 수정할 수 있음
- 모든 웹브라우저는 웹페이지 요소 접근을 위해 DOM 구조 사용

## DOM 객체

---

- window
    - DOM을 표현하는 창
    - 가장 최상위 객체
    - 탭 기능이 있는 브라우저에서는 각각의 탭을 각각의 window 객체로 나타냄
    - 주요 메서드
        - window.open
        - window.alert
        - window.print

- document
    - 문서 전체를 나타냄
    - 윈도우의 하위타입 (원래는 window.document  임)
    - 주요 객체
        - documnet.title
    

## 파싱

---

- 브라우저가 문자열을 해석해서 DOM Tree로 만드는 과정
- Parse → Style → Layout
    
    

## DOM 조작 종류

---

- 선택(Select)
- 조작(Manipulation)

## DOM 선택 관련 메서드

---

`document.querySelector(selector)`

- 제공한 선택자와 일치하는 element 한개 선택
- 제공한 CSS selector를 만족하는 첫번째 element 객체를 반환 (없다면 null 반환)

`document.querySelectorAll(selector)`

- 제공한 선택자와 일치하는 모든 element 선택
- 매칭할 하나 이상의 셀렉터를 포함하는 유효한 CSS selector를 인자로 받음
- 제공한 CSS selector를 만족하는 NodeList 반환

※ querySelector를 통해 나온 객체는  HTML 요소 값이 변하는 것을 실시간으로 반영하지 않는다.

## DOM 조작 관련 메서드

---

생성

`document.createElement(tagName)`

- 작성한 tagName의 HTML 요소를 생성해서 반환

입력

`Node.innerText`

- Node 객체와 그 자손의 텍스트 컨텐츠(DOMString)을 표현
- 해당 요소 내부의 raw text
- 사람이 읽을 수 있는 요소만 남김

추가

`Node.appendChild()`

- 한 Node를 특정 부모 Node의 자식 NodeList 중 마지막 자식으로 삽입
- 한번에 오직 하나의 Node만 추가할 수 있음
- 추가된 Node 객체를 반환
- 만약 주어진 Node가 이미 문서에 존재하는 다른 Node를 참조한다면 현재 위치에서 새로운 위치로 이동

삭제

`Node.removeChild()`

- DOM에서 자식 Node를 제거
- 제거된 Node를 반환

속성 조회 및 설정

`Element.getAttribute(attributeName)`

- 해당 요소의 지정된 값(문자열)을 반환
- 인자(attributeName)는 값을 얻고자 하는 속성의 이름

`Element.setAttribute(name, value)`

- 지정된 요소의 값을 설정
- 속성이 이미 존재하면 값을 갱신(덮어쓰기), 존재하지 않으면 지정된 이름과 값으로 새 속성을 추가

## Event

---

- 프로그래밍하고 있는 시스템에서 일어나는 사건(action) 혹은 발생(occurrence)으로, 사용자가 원하는 행동에 응답할 수 있도록 시스템이 말하는 것
- ex) 키보드 키 입력, 브라우저 닫기, 데이터 제출, 텍스트 복사 등
- 수신 → 처리 → 부착 과정으로 작동
    - Event를 수신
    - eventlistner를 사용해 이벤트 처리기 작성
    - html요소에 부착
    - 

사용법

`EventTarget.addEventListener(type, listener[, options])`

- type
    - 반응할 Event 유형을 나타내는 대소문자 구분 문자열
    - ex) click, scroll, submit
- listener
    - 지정된 타입의 Event를 수신할 객체(콜백함수)

target

- 이벤트 객체에는 이벤트가 담긴 요소가 target에 담겨 나옴
- `event.target.value` 를 통해서 해당 태그의 값을 가져올 수 있음

## 이벤트 취소

---

`event.preventDefault()`

- 기존 event가 불필요한 경우 동작을 막음 ex) a태그 누르면 새로고침 되는 것, 복사 못하게하는 것 뚫기

## this

---

- 어떠한 object를 가리키는 키워드 (ex. 자바에서는 this, 파이썬에서는 self)
- JS는 함수 호출될 때 암묵적으로 this를 전달받음
- 하지만 JS는 함수 호출 방식에 따라 this에 바인딩되는 객체가 달라짐
- 즉, 함수를 선언할 때 this에 객체가 결정되는 것이 아니고, 함수를 호출할 때 함수가 어떻게 호출되었는지에 따라 동적으로 결정

## this 호출 종류

---

- 전역 문맥
    - 브라우저에서는 window를 가리킴 (nodejs에서는 global을 가리킴)
    
- 함수문맥
    - 단순 호출
        - 전역 객체를 가리킴
        
        ```jsx
        const myFunc = () => {
        	console.log(this) // window
        }
        ```
        
    
    - Method 호출
        - 메서드를 선언하고 호출한다면, 객체의 메서드이므로 해당 객체가 바인딩
        
        ```jsx
        const myObj = {
        	data : 1,
        	myFunc() {
        		console.log(this) // myObj
        		console.log(this.data) // 1
        	}
        }
        ```
        
    
    - Nested
        - forEach의 콜백함수에서으 this가 메서드의 객체를 가리키지 못하고 전역 객체 window를 가리킴
        
        ```jsx
        const myObj = {
        	numbers : [1],
        	myFunc() {
        		console.log(this) // myObj
        		this.numbers.forEach( function(number){
        			console.log(number) // 1
        			console.log(this) // window	
        		})
        	}
        }
        ```
        
        - 이 상황을 해결하기 위해 **화살표함수**가 등장
        
        ```jsx
        const myObj = {
        	numbers : [1],
        	myFunc() {
        		console.log(this) // myObj
        		this.numbers.forEach( (number) => {
        			console.log(number) // 1
        			console.log(this) // myObj
        		})
        	}
        }
        ```
        
        ⇒ 화살표 함수는 자동으로 한 단계 상위 scope의 context를 바인딩
        

## 화살표 함수

---

- 화살표 함수는 호출의 위치와 상관없이 상위 스코를 가리킴

`Lexical Scope`

- 함수를 어디서 호출하는 지가 아니라 어디에 선언하였는지에 따라 결정
- Static scope 라고도 하며 대부분의 프로그래밍 언어에 따르는 방식
- 함수 내의 함수 상황에서 화살표 함수를 쓰는 것을 권장

※ 하지만 addEventListener에서는 화살표 함수를 사용하면 this가 window에 바인딩 됨, 따라서 functin 키워들르 사용해야 this가 event.target을 가리킴 (function 키워드 사용 권장)


## this가 호출되는 순간(런타임)에 결정되는 것의 장/단점

---

`장점` 

- 함수(메서드)를 하나만 만들어서 여러 객체를 재사용할 수 있다.

`단점`

- 이런 유연함이 실수로 이어질 수 있다.

즉, this가 좋은지 나쁜지는 우리가 판단하는게 중요한 것이 아니다.