# Django

## 기본설정

---

[django 사전 준비사항](https://www.notion.so/django-d740640e4fb6472da39c1ba948b004b4)

```bash
python -m venv venv

source venv/Scripts/activate

pip list

pip install django==3.2.13

#ORM관련 라이브러리
pip install ipython
pip install django-extensions

pip freeze > requirements.txt

django-admin startproject firstpjt .

python manage.py runserver

python manage.py startapp articles

	

```

## Django도메인

---

- 여러 앱을 사용하게 되면 2가지 문제 발생
    1. html파일명 겹칠때 버그
    2. url 이름 겹칠때 버그

### 해결방안

1. url문제
    - html파일에서 url을 쓸때 `app_name`을 지정
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b11a5dca-adf6-4868-86ec-cb14302afa8c/Untitled.png)
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/616b44f0-4126-498a-8b0e-d446bccf1e0b/Untitled.png)
    
2. html문제
    - 기본적으로 Djnago는 모든 app의 `template` 폴더를 가지고 와서 html을 매칭시킴. 때문에 앱 간에 사용하는 html파일이 동일하다면 의도치않게 다른 app의 html을 가져오는 경우가 있음
    - 그래서 app마다 template안에 폴더를 하나 더 둬서 개발하는 방식이 필요
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b9e8e19c-264d-4b41-bb60-27dd68c3933e/Untitled.png)
        

## Migration

---

Django에서 model을 수정하면 이를 `DB` 에 적용하기 위해 `migration` 과정이 필요함

명령어

```bash
python manage.py makemigrations
python manage.py migrate
```

기타 명령어

- `python manage.py showmigrations`
    
    migrations들이 migrate 됐는지 안됐는지 여부를 확인하는 용도
    
- `python manage.py sqlmigrate articles 0001`
    
    해당 migrations 파일이 SQL 문으로 어떻게 해석될 지 볼 수 있음
    

## DB에 필드를 새로 추가하고 migration할 때 옵션

---

 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cdacf401-7568-442c-91b8-84b71f44b719/Untitled.png)

1. Django가 기본으로 기본 레코드들의 새 필드에 빈 값을 넣어줌
2. 명령어를 종료하고 사용자가 집접 코드에 기본값 입력

## Field

---

### DateTimeField

- `auto_now_add`
    
    최초 생성 일자 (Useful for creation of timestamps)
    
    데이터가 실제로 만들어질 때 현재 날짜와 시간으로 자동으로 초기화되도록 함
    
- `auto_now`
    
    최종 수정일자
    
    데이터가 수정될 때마다 현재 날짜와 시간으로 자동으로 갱신됨
    

## ORM

---

`Object-Relational-Mapping`

- 객체 지향 프로그래밍 언어를 사용하여 호환되지 않는 유형의 시스템 간에 (Django ↔ DB) 데이터를 변환하는 프로그래밍 기술
- 객체 지향 프로그래밍과 데이터베이스를 연동할 때, 호환되지 않는 데이터를 변환하는 프로그래밍 기법
- Django에는 내장 Django ORM이 있다.
- 결국 빠른 `생산성`을 위해 ORM을 사용

### ORM 장점

- SQL을 몰라도 DB조작 가능
- 객체 지향적 접근으로 인한 높은 생산성

### ORM 단점

- ORM만으로 세밀한 DB조작을 구현하기 어려운 경우가 있다.

## Django shell 사용

---

- ORM 사용할때 `ipython` , `django-extensions` 설치하고, 후자는 app에 등록해줘야함
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8dc78e01-f55a-44b5-9056-7c2cc24e8a62/Untitled.png)
    

```bash
python [manage.py](http://manage.py) shell_plus
```

위 명령어를 통해 shell 실행

## Query Set Api

---

ex) Article.objects.all()

- Article: `Model class`
- objects: `Manager`
- all: `QuerySet API`

### Object manager

- Django 모델이 데이터베이스 쿼리 작업을 가능하게 하는 인터페이스
- 기본적으로 모든 Django모델 클래스에 대해 objects라는 Manager 객체 자동으로 추가
- Manager를 통해 특정 데이터 조작 가능
- DB를 Python class로 조작할 수 있도록 여러 메서드 제공

쿼리를 통해 반환되는 객체가 여러개면 `QuerySet` 으로, 한개면 `instance` 로 반환됨

## ORM명령어

---

`Article.objects.all()` : Article 모델의 모든 객체 불러오기

## ORM을 통한 CRUD

---

※ DB에 시간이 저장될 때는 `UTC` 로 저장됨! 후에 장고에서 파일을 읽을 때 다시 설정된 시간으로 변형되서 오게 됨

### CREATE

- 객체 생성 방법 3가지
- 방법1: (인스턴스 생성 후 값 설정, save 호출)
    - `article = Article()` : 클래스를 통한 인스턴스 생성
    - `article.title` : 클래스 변수명과 같은 이름의 인스턴스 변수를 생성 후 값 할당
    - `article.save()` : 인스턴스로 save 메서드 호출
- 방법2: (인스턴스 생성시 초기값을 같이 할당, save호출)
    - `article = Article(title='second', content='django!')`
    - `article.save()`
- 방법3: (create method 활용)
    - `Article.objects.create(title='third', content='django!')`

### READ

- `all()` : 모든 객체를 `Query Set` (단일객체일시 instance)로 반환
- `get()`: 단일 객체 조회 (객체가 없거나 2개이상이면 에러남)
    - 객체 없을 때 : `DoesNotExist`
    - 둘 이상 객체가 있으면 : `MultipleObjectsReturned`
    
    → 때문에 primary key와 같은 고유성(Uniqueness)를 보장하는 조회에서 사용해야 함
    
- `filter()` : 조건에 맞는 객체들 queryset으로 반환
    - 객체가 없어도 빈 쿼리셋, 하나여도 쿼리셋,  여러개여도 쿼리셋!
    - 하지만 filter 조건에 맞는 객체가 여러개 있을 때 사용하는 것이 좋음(2개이상)
- `Field lookups` : 조회 조건에 일부 문자패턴과 일치하는 것 찾기
    - ex) Article.objects.filter(content__contains=’ja’)
    - __contains 를 사용하면 패턴을 찾을 수 있음
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7b2c7332-564c-4703-9671-f1e5313e78a6/Untitled.png)
        
    

### UPDATE

※ 수정을 하려면 먼저 조회(READ)를 해야함

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/67c6b7ea-2572-4622-93f9-d89b5674e1aa/Untitled.png)

- read를 통해 데이터를 불러와서 클래스 변수 값을 수정하고 다시 `save`하면 됨

### DELETE

※삭제를 할려고 해도 먼저 조회(READ)를 해야함

- `delete()` 메서드를 통해 삭제

### 참고

- `__str__()` 메서드를 모델 클래스에 오버라이딩하면 변수를 출력할 때 어떻게 출력될 지 지정할 수 있음

## CSRF 공격

---

- 사용자가 자신의 의지와 무관하게 공격자가 의도한 행위(수정, 삭제)를 웹사이트에 요청하게하는 공격
- 2008년 옥션 사례
    - 이미지 크기가 0으로 하고, src에 관리자 계정의 비밀번호를 바꾸는 링크를 넣음(url의 query를통해)

## CSRF 방어수단

---

- 사용자에게 hash토큰을 발급해서 매번 검증함

## URL 태그를 사용해서 `variable routing` 하고 연결하기

---

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/58a68e48-9f6e-4bda-8976-fdd684281ac3/Untitled.png)

한칸 띄우고 쓰면 됨

## Admin

---

`python [manage.py](http://manage.py/) createsuperuser` 으로 관리자계정 생성

이후 admin 로그인 가능

`admin.site.register(Article)` 으로 DB 등록

db 보여질 필드 정하기

```python
from django.contrib import admin
from .models import Article
# Register your models here.

class article_display(admin.ModelAdmin):
    list_display = ('pk','title','content','created_at', 'updated_at') # admin에서 보여줄 field
    list_display_links = list_display # admin에서 누르면 수정/삭제가 가능하게할 필드 목록
admin.site.register(Article, article_display)
```

## Django Form

---

- 지금까지는 `HTML FORM` 으로 온 모든 요청을 받았는데, 요청중에는 형식에 맞지 않거나 공격도 있음
- Django Form을 통해 유효성 검사를 단순하고 자동화 할 수 있는 기능 제공
- Django는 Form에 관련한 작업의 세 부분을 처리
    - 렌더링을 위한 데이터 준비 및 재구성
    - 데이터에 대한 HTML forms 생성
    - 클라이언트로부터 받은 데이터 수신 및 처리

## Django From 사용법

---

- Form 클래스를 선언함 (Model과 마찬가지로 상속관계를 통해 선언)
    - 클래스 안에 필드들을 선언(Model과 방식 동일)
    - forms에서는 charfield의 `max_length` 속성이 필수❌
    - forms에서는 `textfield`가 없음
    - `choice field`를 통해 선택하는 창을 만들 수 있음
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e314a722-bc2d-4340-8612-f7cb6f4d5c14/Untitled.png)
        
- View에서 Context에 클래스 넣어줌
- HTML에 form 사용
    - `as_p` 쓰면 각 필드가  p로 감싸져서 렌더링
- widget 속성을 통해 html에 textarea로 표시 가능

## Model Form

---

- Form을 Model과 연결
- Model Form 안의 meta클래스에서 정보 기입
    - `model` : 사용할 모델
    - `fields` : 사용할 필드 `__all__` 을 통해 모든 필드 가져올 수 있음
    - `exclude` : 뺄 필드 정함

## Model Form을 통한 데이터 저장, 수정

---

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/702dd9fb-86f2-41b9-8de4-34d9c755fc42/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e15cca6b-5471-42bd-86ac-60ebab467337/Untitled.png)

## Widget을 통한 데이터 입력

---

- Widget은 데이터유효성 검사와 관련❌
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cfbf5dfe-d5a7-46df-85c9-850a369f0107/Untitled.png)
    

## HTTP Method에 따라서 (NEW, CREATE) 합치기

---

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9059a884-265f-44a6-ae4f-75ab76865929/Untitled.png)

## HTTP Method에 따라서 (EDIT, UPDATE) 합치기

---

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/47e6c940-d22f-463c-967f-763ec4812580/Untitled.png)

## Detail 페이지 template does not found → 404

---

```python
from django.shortcuts import render, redirect, get_object_or_404
movie = get_object_or_404(Movies, pk=pk)
```

## 데코레이터(Decorator)

---

- 기존에 작성된 함수에 기능을 추가하고 싶을 때, 해당 함수를 수정하지 않고, 기능을 추가해주는 함수
- get만 허용
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/25f753b7-f857-48ee-91e8-418149ea9c73/Untitled.png)
    
    - 이렇게하면 GET만 됨, 다른 method는 `405(Not Allowed)` 에러
    - `require_get` 도 있지만 `require_safe` 권장
- post만 허용
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9e7bcbec-aa83-4dde-8bb9-3e0675332dd5/Untitled.png)
    
- 여러 메서드 허용하는 방법
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/84b6a95b-ac3a-4f63-96ce-9d9795b85875/Untitled.png)
    

## 사용자 인증

---

`Authentication(인증)` : 신원 확인, 사용자 자신이 누구인지 확인하는 것

`Authorization(권한, 허가)` : 권한부여, 인증된 사용자가 수행할 수 있는 작업을 결정

- accounts앱 생성
- Django에서 기본으로 제공하는 `abstractUser` 를 상속받아서 사용