## SPA

---

- Single Page Application
- 사용자 요청에 대해 적절한 페이지 별 template 반환
- 서버에서 최초 1장의 HTML만 전달받아 모든 요청에 대응하는 방식

## SSR

---

- Server Side Rendering
- 서버에서 HTML을 렌더링 해서 제공

## CSR

---

- Client Side Rendering
- 장고와 다르게 프론트엔드 Vue는 CSR
- 각 요청에 대한 대응을 JavaScript를 사용해서 필요한 부분만 다시 렌더링
- 필요한 페이지를 서버에 AJAX로 요청
- 서버는 화면을 그리기 위해 필요한 데이터를 JSON으로 전달
- JSON을 JavaScript로 처리후 DOM 트리에 반영(렌더링)

## CSR을 사용하는 이유

---

- 모든 HTML을 서버로부터 받는 것이 아니다.
- 매번 새 문서를 받아 새로고침 하는 것이 아니라 필요한 부분만 고쳐 나가므로 각 요청이 끊김없이 진행
- 요청이 자연스럽게 진행돼 UX 향상이 가능하다
- BE와 FE 작업 영역을 명확히 분리할 수 있다. (협업이 용이해짐)

## CSR 단점

---

- 첫 구동시 필요한 데이터가 많을수록 최초 작동 시작까지 오랜 시간 소요
- 검색엔진최적화(SEO)가 어렵다.
    - 단지 해당 url로 가면 텅빈 HTML이고, 내용을 채우는 것은 AJAX 요청으로 얻은 JSON 데이터로 클라이언트(브라우저)가 진행하기 때문에

※ 따라서 CSR과 SSR을 적절히 사용해야 하고, 최근 FE 프레임워크에서도 SSR을 지원하는 프레임워크들이 있음 (ex. React의 Next) 

## Vue의 특징

---

- 다른 Framework보다 쉽다. (입문용)
- FE Framework를 빠르고 쉽게 학습하고 활용 가능
- Vue가 React와 Angular 이후에 나왔으므로 이후 다른 프레임워크를 학습하기가 수월하다.
- `템플릿 - 스크립트 - 스타일` 3부분으로 이루어짐

## Vue Docs (Vue 2)

---

[Introduction - Vue.js](https://v2.vuejs.org/v2/guide/)

## Vue 시작하기

---

CDN추가, script에 Vue 객체 선언

```jsx
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    // CODE HERE
    new Vue({
      el: '#app',
      data : {
        message : '',
      },  
    })
  </script>
```

html에 message 설정 추가

```html
<body>
  <div id="app">
    <p id="name">name : {{ message }} </p>
    <input id="inputName" type="text" v-model="message">
  </div>
```

## 프레임 워크 사용할 때의 이점

---

- message가 바뀔 때 변경돼야 하는 부분이 많으면 vanilla js에서는 event listener를 통해 다 바꿔줘야 하는데, Vue에서는 v-model이 돼있으니까 안바꿔도 됨

## Vue3가 있는데 왜 Vue2를 쓰나요?

---

- Vue3가 `22년2월`에 나옴, 아직 안정성👎, 레퍼런스할만한 자료가 적음
- 공식문서 볼때 조심!!
- 공식문서에 한글화가 잘 안돼있으므로 영문으로 보기

## ⚡MVVM⚡

---

- `Model + View + Model View` 로 이루어진 디자인 패턴
- Model → 실제 데이터
- View → 눈에 보이는 부분(DOM)
- View Model → VUE
    - View와 연결 돼서 Action을 주고받음
    - Model이 변경되면 View Model도 변경되고 바인딩된 View도 변경
    - View에서 사용자가 데이터를 변경하면, View Model 데이터가 변경되고, 바인딩된 다른 View도 변경됨

## 생성자 함수

---

- 함수를 생성자 형태로 호출해서 내부 변수를 설정할 수 있음
- 대문자로 시작해야함
- 반드시 `new` 연산자를 사용해야 함

## el 속성

---

- Dom 요소와 연결
- Dom과 mount 한다고 표현
- element 약자

## data 속성

---

- mvvm에서 model 담당
- DOM과 값을 연결
- 데이터 객체는 반드시 기본객체 `{}(Object)` 여야 함
- 객체 내부의 아이템들은 value로 모든 타입의 객체를 가질 수 있음
- 정의된 속성은 `interpolateion {{}}` 을 통해 view에 렌더링 가능함
- 추가된 객체의 값들은 this로 접근 가능

## methods 속성

---

- 올 수 있는 method들 정의
- 정의 하고 나면 `객체.method이름`으로 접근 가능
- method 선언할때 ⚠️⚠️⚠️**화살표함수 사용하면 안됨**⚠️⚠️⚠️
    - 화살표 함수 쓰면 this가 window 가리킴
    - function 키워드 써야 this가 vue 객체를 가리킴
    - 정의할 때 말고 다른 경우는 화살표함수 써도 됨

## v-html

---

- Vue 객체의 data에 작성한 HTML을 그대로 적용해서 보여주는 HTML 속성
- RAW HTML을 그대로 표현할 수 있음
- 사용자가 입력하거나 제공하는 컨텐츠에는 **절대사용금지**
    - XSS 공격에 취약함

## Directives

---

- v-접두사가 있는 특수 속성에 값을 할당할 수 있는 것
- directive는 표현식의 값이 변경될 때 반응적으로 DOM에 적용

구성


## v-show

---

- 표현식에 따라 element를 보여줄 것인지 결정
- boolean 값이 변경될 때마다 반응
- 렌더링은 항상 돼있고, 보여줄지 말지를 결정 (display-none으로 됨)

## v-if

---

- v-show와 사용방법은 동일하지만 값이 false일 때 DOM에서 사라짐
- v-if, v-else-if, v-else 형태로 사용 가능
- v-show는 초기 로딩이 비싸지만 toggle은 싸다. ↔ v-if는 초기로딩이 싸고 toggle은 비쌈 (DOM에서 삭제했다가 다시 만들어야 하므로)
- 따라서 토글이 많으면 v-show 쓰고, 초기 렌더링 비용이 중요하면 v-if 사용

### v-for

---

- 객체를 받아서 for문 실행
- v-for 사용시 key 속성을 각 요소에 작성해야 함
    - key는 중복되면 안됨! (for문 뿐만 아니라 HTML 내에서 중복되면 안됨)
- vue 화면 구성시 이전과 달라진 점을 확인하는 용도로 활용
- 문자열 예시
    - index가 2번째로 온다.
    
    ```html
    <div id="app">
        <h2>String</h2>
    		<!-- 방법 1 -->
        <div v-for="char in myStr">
          {{ char }}
        </div>
    		<!-- 방법 2 -->
    		<!-- index가 2번째로 온다. -->
        <div v-for="(char, index) in myStr" :key="index">
          <p>{{ index }}번째 문자열 {{ char }}</p>
        </div>
    </div>
    
    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
      <script>
        const app = new Vue({
          el: '#app',
          data: {
            // 1. String
            myStr: 'Hello, World!',
    			}
    		})
    	</script>
    ```
    
- 객체 예시
    - 키가 2번째로 온다.
    
    ```html
    <body>
      <div id="app">
        <h2>Object</h2>
        <div v-for="value in myObj">
          <p>{{ value }}</p>
        </div>
    		
    		<!-- 키가 2번째로 옴!! -->
        <div v-for="(value, key) in myObj"  :key="key">
          <p>{{ key }} : {{ value }}</p>
        </div>
      </div>
    
      <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
      <script>
        const app = new Vue({
          el: '#app',
          data: {
            // 3. Object
            myObj: {
              name: 'harry',
              age: 27
            },
          }
        })
      </script>
    </body>
    ```
    

## v-on

---

- 이벤트리스너 부착 가능
- `@` shortcut 제공
- count 예시

```html
<body>
  <div id="app">
    <button v-on:click="number++">increase Number</button>
    <p>{{ number }}</p>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        number: 0,
      },
    })
  </script>
</body>
```

- toggle 예시

```html
<body>
  <div id="app">
    <button v-on:click="toggleActive">toggle isActive</button>
    <p>{{ isActive }}</p>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        isActive: false,
      },
      methods: {
        toggleActive: function () {
          this.isActive = !this.isActive
        },
			},
    })
  </script>
</body>
```

- @shortcut 예시

```html
<body>
  <div id="app">
    
		<button v-on:click="toggleActive">toggle isActive</button>
    <p>{{ isActive }}</p>

    <button @click="checkActive(isActive)">check isActive</button>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        number: 0,
        isActive: false,
      },
      methods: {
        toggleActive: function () {
          this.isActive = !this.isActive
        },
        checkActive: function (check) {
          console.log(check)
        },
      }
    })
  </script>
</body>
```

## v-bind

---

- HTML 속성에 Vue data 연결
- class의 경우 다양한 형태로 연결 가능
- html 속성값에 js 속성을 쓰기 위한 것이 bind임☀️
- `:` 로 축약 가능
    
    ```html
    <p v-bind:class="redTextClass">빨간 글씨</p>
    <p v-bind:class="{ 'red-text': true }">빨간 글씨</p>
    <p v-bind:class="[redTextClass, borderBlack]">빨간 글씨, 검은 테두리</p>
    <p :class="theme">상황에 따른 활성화</p>
    <button @click="darkModeToggle">dark Mode {{ isActive }}</button>
    
    <script>
    	const app2 = new Vue({
    	      el: '#app2',
    	      data: {
    	        url: 'https://www.google.com/',
    	        redTextClass: 'red-text',
    	        borderBlack: 'border-black',
    	        isActive: true,
    	        theme: 'dark-mode'
    	      },
    				methods: {
    	        darkModeToggle() {
    	          this.isActive = !this.isActive
    	          if (this.isActive) {
    	            this.theme = 'dark-mode'
    	          } else {
    	            this.theme = 'white-mode'
    	          }
    	        }
    				},
    	})
    </script>
    ```
    

## v-model

---

- Vue instance와 DOM의 양방향 바인딩
- input에 v-on 썼던 것을 대체 가능(v-model에 input관련 데이터를 연결하면 됨)
    
    ```html
    <div id="app">
        <h2>1. Input -> Data</h2>
        <h3>{{ myMessage }}</h3>
        <input @input="onInputChange" type="text">
        <hr>
    
        <h2>2. Input <-> Data</h2>
        <h3>{{ myMessage2 }}</h3>
        <input v-model="myMessage2" type="text">
        <hr>
    </div>
    
    <script>
        const app = new Vue({
          el: '#app',
          data: {
            myMessage: '',
            myMessage2: '',
          },
          methods: {
            onInputChange: function (event) {
              this.myMessage = event.target.value
            },
          }
        })
    </script>
    ```
    
- 한글을 입력할 때는 한 글자가 완성돼야 하나가 뜸
    - 한글이나 중국어,  일본어처럼 여러 키보드 입력의 조합이 한 글자가 되는 경우(조합형 문자)에 문제 발생
    - 따라서 한글 써야할 떄는 위의 방법대로 해야함
    

## Computed (🆚 methods)

---

- Computed에 정의된 함수는 최초 렌더링될 때만 호출하여 계산
- html 내에 여러번 쓰여도 한번 계산한 값을 재활용해서 씀 (계산된 값은 캐싱돼있음)
- 함수 안에서 사용하는 값이 변할 때 재계산됨

## Watch

---

- 특정 데이터의 변화를 감지하는 기능
- 첫번째 인자는 변동 전 data, 두번째 인자는 변동 후 data
- 속성으로 객체를 지정하고 handler에 이미 정의된 함수이름을 사용할 수 도 있음
- 배열([])이나 객체({})를 추적하려면 depp 속성을 true로 해야한다.
- 함수명을 data의 속성 이름하고 같이 사용하면 해당 data가 변할 때 함수를 실행
    
    ```html
    <body>
      <div id="app">
        <h3>Increase number</h3>
        <p>{{ number }}</p>
        <button @click="number++">+</button>
        <hr>
    
        <h3>Change name</h3>
        <p>{{ name }}</p>
        <input type="text" v-model="name">
        <hr>
    
        <h3>push myObj</h3>
        <p>{{ myObj }}</p>
        <button @click="itemChange">change Item</button>
      </div>
    
      <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
      <script>
        const app = new Vue({
          el: '#app',
          data: {
            number: 0,
            name: '',
            myObj: {completed: true}
          },
          methods: {
            nameChange: function () {
              console.log('name is changed')
            },
    
            itemChange: function () {
              this.myObj.completed = !this.myObj.completed
            }
          },
          watch: {
            number: function (val, oldVal) {
              console.log(val, oldVal)
            },
    
            name: {
              handler: 'nameChange'
            },
    
            myObj: {
              handler: function (val) {
                console.log(val)
              },
              deep: true
            },
          }
        })
      </script>
    </body>
    ```
    

## Filter

---

- 텍스트 형식화를 적용할 수 있는 필터
- interpolation 혹은 v-bind를 이용할 때 사용 가능
- 필터는 자바스크립트 표현식 마지막에 `|` (파이프)와 함께 추가되어야 함
- 이어서 사용 가능(Chaining)
    
    ```html
    <body>
      <div id="app">
        <p>{{ numbers | getOddNums | getUnderTenNums }}</p>
      </div>
    
      <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
      <script>
        const app = new Vue({
          el: '#app',
          data: {
            numbers: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15],
          },
          filters: {
            getOddNums: function (nums) {
              const oddNums = nums.filter((num) => {
                return num % 2
              })
              return oddNums
            },
            
            getUnderTenNums: function (nums) {
              const underTen = nums.filter((num) => {
                return num < 10
              })
              return underTen
            }
          }
        })
      </script>
    </body>
    ```
    ## Vue CLI 설치

---

****`npm install -g @vue/cli`****

## Vue 프로젝트 생성

---

`npm create vue-cli`

⇒ git bash에서는 interactive(대화형) shell을 지원하지 않으므로 vscode에서 실행하기

## vue 프로젝트 실행

---

`npm run serve`

## Babel

---

- ES6+ 코드를 구버전으로 번역/변환 해주는 도구
- JS의 파편화, 표준화의 영향으로 작성된 코드의 스펙트럼이 매우 다양함

## Webpack

---

- static module bundler
- 모듈간의 의존성 문제를 해결하기 위한 도구
- 프로젝트에 필요한 모든 모듈을 매핑하고, 내부적으로 종속성 그래프를 빌드함
- 즉 번들러 중 하나
    - 다른 번들러 : snowpack, parcel, rollup.js

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8f1950f9-7643-4ec2-9fef-605c07ff75aa/Untitled.png)

## Node modules의 의존성

---

- 2016년 left-pad 사태 (라이브러리 하나 없어졌더니 웹 서버 상당수가 다운)
- package.json에 설치된 node 모듈들이 나열 돼있고
- package-lock.json에 모듈들 간의 의존성 정보가 나타나있음

## Component

---

- UI를 독립적이고 재사용 가능한 조각들로 나누는 것
    - 즉, 기능별로 분화한 코드 조각
- CS에서는 다시 사용할 수 있는 범용성을 위해 개발된 소프트웨어 구성 요소를 의미
- 하나의 app을 구성할 때 중첩된 컴포넌트들의 tree로 구성하는 것이 보편적임
- 유지보수가 쉬워지고 재사용성이 높아진다.

## SFC

---

- Single File Component
- `.vue` 파일 하나를 하나의 컴포넌트로 처리

## Vue Component 구조

---

1. 템플릿
    - HTML의 Body 부분
    - 눈으로 보여지는 요소 작성
    - 다른 컴포넌트를 HTML 요소처럼 추가 가능
2. 스크립트
    - Javascript 코드가 작성되는 곳
    - 컴포넌트 정보, 데이터, 메서드 등 vue 인스턴스를 구성하는 대부분이 작성 됨
3. 스타일
    - CSS가 작성되며 컴포넌트의 스타일을 담당

※ 이렇게 컴포넌트들이 tree 구조를 이루어 하나의 페이지를 만들게 됨. 즉, index.html 파일 하나만을 rendering

## Vue Component 네이밍 규칙

---

`Pascal Case` : 단어별 첫글자대문자로 (ex. MyComponent)

## Vue Template 작성 규칙

---

- Template 안에 하나의 태그만 작성가능(ex. div)
    
    ```html
    <template>
      <div>
        <h1>이거는 내가 만든 새로운 컴포넌트다!</h1>
      </div>
    </template>
    ```
    

## Component 등록 3단계

---

- 불러오기
    
    ```
    // 1. 불러오기
    import MyComponent from './components/MyComponent.vue'
    import MyComponent from '@/components/MyComponent'
    // @ : 절대경로(src) , .vue는 생략 가능
    ```
    
- 등록하기
    
    ```jsx
    export default {
      name: 'App',
      components: {
        HelloWorld,
        //2. 등록하기
        MyComponent,
      }
    }
    ```
    
- 보여주기
    
    ```html
    <template>
      <div id="app">
        <img alt="Vue logo" src="./assets/logo.png">
        <HelloWorld msg="Welcome to Your Vue.js App"/>
        <!-- 보여주기 -->
        <MyComponent/>
      </div>
    </template>
    ```
    

## Vue에서 컴포넌트 사용할 때 에러

---

- 컴포넌트를 impor하고 사용하지 않으면 에러남

## Vue2 한글 docs

---

[Vue.js](https://vue2.hphk.io/)

## Data in Components

---

- 컴포넌트는 부모-자식 관계에서만 데이터를 주고받음
- 부모 ⇒ 자식으로의 데이터 흐름: pass props
- 자식 ⇒ 부모: emit event

## Pass Props

---

- `props` : 부모 컴포넌트의 정보를 전달하기 위한 사용자 지정 특성
- 자식 컴포넌트는 props 옵션을 사용하여 수신하는 props를 명시적으로 선언해야 함
- 정적인 데이터를 전달하는 경우 static props 라고 명시하기도 함
- 속성의 이름은 html쪽은 `kebab-case` , js쪽은 `CamelCase`를  사용해야함 (😒)

```jsx
// MyComponent.vue
<template>
  <div class="border">
    <h1>이거는 내가 만든 새로운 컴포넌트다!</h1>
    <MyComponentItem static-props="MyComponent에서 보낸 DATA"/>
  </div>
</template>

<script>
import MyComponentItem from '@/components/MyComponentItem'
export default {
    name: 'MyComponent',
    components: {
      MyComponentItem
    }
}
</script>

<style>
  .border {
    border : solid;
  }
</style>
```

```jsx
// MyComponentItem.vue
<template>
  <div>
    <h3>나는 MyComponent의 하위 컴포넌트</h3>
    <p>{{ StaticProps }}</p>
  </div>
</template>

<script>
export default {
    name: 'MyComponentItem',
    props: {
        StaticProps: String,
    }
}
</script>

<style>

</style>
```

## 동적 props

---

- vind를 통해서 js 객체를 전달할 수 있음
- `:` 를 사용해 바인딩 가능

## CLI에서의 data

---

- 함수의 return 형태로 data를 연결해야함

```
export default {
  name: "MyComponent",
  components: {
    MyComponentItem,
  },
  data: function () {
    return {
      
    }  
  }
};
```

이유 : 각 Vue 인스턴스가 data 객체를 공유하므로

## 단방향 데이터 흐름

---

- 모든 props는 부모에서 자식으로, 단방향 바인딩을 형성
- 부모 속성이 업데이트되면 자식으로 흐르지만 반대방향은 아님
- 부모 컴포넌트가 업데이트 될 때마다 자식 컴포넌트의 모든 prop이 최신값으로 새로고침 됨

이유 : 하위 컴포넌트가 실수로 상위 컴포넌트 상태를 변경하여 앱의 데이터 흐름을 이해하기 힘들게 만드는 것을 방지

 

## Emit Event

---

- 자식 컴포넌트에서 부모 컴포넌트로 데이터를 전달할 때는 이벤트를 발생시킴
    1. 데이터를 이벤트 리스너의 콜백함수의 인자로 전달
    2. 상위 컴포넌트는 해당 이벤트를 통해 데이터를 받음
- emit은 한 단계 위의 부모에게만 할 수 있음. 그 이상으로 가려면 함수를 계속 선언해서 전파시켜 줘야 함

```jsx
//MyComponentItem.vue
<template>
  <div>
    <h3>나는 MyComponent의 하위 컴포넌트</h3>
    <p>{{ staticProps }}</p>
    <p>{{ dynamicProps }}</p>
    <button @click="childToParent">클릭!</button>
  </div>
</template>

<script>
export default {
    name: 'MyComponentItem',
    props: {
        staticProps: String,
        dynamicProps : String,
    },
    methods: {
      childToParent: function() {
        this.$emit('child-to-parent', "나는 자식이 보낸 데이터다.")
      }  
    },
}
</script>

<style>

</style>
```

```jsx
//MyComponent.vue
<template>
  <div class="border">
    <h1>이거는 내가 만든 새로운 컴포넌트다!</h1>
    <MyComponentItem static-props="MyComponent에서 보낸 DATA" 
    :dynamic-props="dynamicProps"
    @child-to-parent="parentGetEvnet"/>
  </div>
</template>

<script>
import MyComponentItem from "@/components/MyComponentItem";
export default {
  name: "MyComponent",
  components: {
    MyComponentItem,
  },
  data: function () {
    return {
      dynamicProps: "이건 동적인 데이터",
    }
    
  },
  methods: {
    parentGetEvnet : function (childData) {
      this.dynamicProps = childData
      console.log(childData)
    }
  }
};
</script>

<style>
.border {
  border: solid;
}
</style>
```

## Array.splice 메서드

---

배열에서 특정 인덱스에서 N개만큼 데이터를 지우고 다시 배열을 반환해줌

```
const index = this.todos.indexOf(todo)
this.todos.splice(index, 1)
```