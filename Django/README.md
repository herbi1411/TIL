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