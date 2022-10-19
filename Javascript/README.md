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