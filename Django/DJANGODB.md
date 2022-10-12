# Django Database

## ForeignKey 속성 삽입

---

```python
class Comment(models.Model):
    article = models.ForeignKey(Article, on_delete=models.CASCADE)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.content
```

## on_delete

---

- 외래 키가 참조하는 객체가 사라졌을 때, 외래 키를 가진 객체를 어떻게 처리할 지를 정의

`CASCADE` : 부모가 삭제됐을 때 이를 참조하는 객체도 삭제

`PROTECT` , `SET_NULL` , `SET_DEFAULT` 등 여러 옵션 존재

## 데이터 무결성

---

- 개체 무결성 (Entity Integrity)
- 참조 무결성 (Referential Integrity)
- 범위 무결성 (Domain Integrity)

## Related manager

---

`comment_set.method()`

- `1:N` 관계에서 N 모델이 1 모델을 역참조 할 때 사용

`comment_set.all()`

- 모든 게시글을 출력

`related_name` 속성을 통해 역참조시 사용되는 매니저 이름(model_set manager)를 변경할 수 있다.

```python
class Comment(models.Model):
    article = models.ForeignKey(Article, on_delete=models.CASCADE, realated_name = 'comments')
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.content
```

## Commit 전에 외래키 주입하기

---

- Comment에 Article 정보를 주입하려면 views.py에 와서 해야하므로, `commit` 옵션을 False로 지정해서 사용자가 article 정보를 주입후 저장될 수 있도록 해야함

```python
@require_POST
def comments_create(request, pk):
    article = Article.objects.get(pk=pk)
    comment_form = CommentForm(request.POST)
    if comment_form.is_valid():
        comment = comment_form.save(commit=False)
        comment.article = article
        comment.save()
        return redirect("articles:detail", article.pk)
```

## 댓글 목록 detail에 넣기

---

```python
@require_safe
def detail(request, pk):
    article = Article.objects.get(pk=pk)
    comment_form = CommentForm()
    comments = article.comment_set.all()  
    context = {
        'article': article,
        'comment_form' : comment_form,
        'comments' : comments,
    }
    return render(request, 'articles/detail.html', context)
```

## 댓글 갯수 출력하기

---

방법1 : DTL filter - length 사용

`{{ comments|length }}`

`{{ article.comment_set.all | length }}`

방법2: Queryset API - count() 사용

`{{ comments.count }}`

`{{ article.comment_set.count }}`

※ 댓글이 없을 때 할 일은 `for - empty - endfor` 구문에서 empty를 이용해 코드 삽입 가능

## 게시글에 User 속성 넣기

---

```python
from django.db import models
from django.conf import settings

# Create your models here.
class Article(models.Model):
	  user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE) # User 모델에 바로 접근하지 않고, settings.py 의 AUTH_USER_MODEL을 통해 접근!
    title = models.CharField(max_length=10)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.title
```

→ 게시글 데이터가 존재할 때 user 속성을 추가하고 migration을 하려고 하면 이미 있는 게시글들의user 값을 넣는 작업이 필요 (콘솔에서 장고의 도움 받기 or [models.py](http://models.py) 에 user 속성 default 값 넣기)

## Django에서 User model을 받을 때

---

- [models.py](http://models.py) 에서는 `settings.AUTH_USER_MODEL`  → 문자열로 받아옴
- 다른 모든 곳에서는 `get_user_model()` → 객체로 받아옴

💡 이유

- [models.py](http://models.py) 에는 user 모델이 생성되기 전이기 때문에 `get_user_model()`을 사용할 수 없음

## 게시글 작성할 때 User 포함해서 넣기

---

1. forms.py에 exclude 속성 추가

```python
class ArticleForm(forms.ModelForm):
    title = forms.CharField(
        label='제목',
        widget=forms.TextInput(
            attrs={
                'class': 'my-title',
                'placeholder': 'Enter the title',
                'maxlength': 10,
            }
        )
    )

    content = forms.CharField(
        label='내용',
        widget=forms.Textarea(
            attrs={
                'class': 'my-content',
                'placeholder': 'Enter the content',
                'rows': 5,
                'cols': 50,
            }
        ),
        error_messages={
            'required': '내용을 입력하세요.',
        }
    )

    class Meta:
        model = Article
        # fields = '__all__'
        exclude = ('user',) # user 속성 제외하고 보이게
```

1. views.py에서 commit하기 전에 user 속성 넣기

```python
@login_required
@require_http_methods(['GET', 'POST'])
def create(request):
    if request.method == 'POST':
        form = ArticleForm(request.POST)
        if form.is_valid():
            article = form.save(commit=False) # 커밋전에
            article.user = request.user # user 속성 넣기
            article.save() # 저장
            return redirect('articles:detail', article.pk)
    else:
        form = ArticleForm()
    context = {
        'form': form,
    }
    return render(request, 'articles/create.html', context)
```

## 게시글 삭제시 작성자 일치여부 확인

---

```python
@require_POST
def delete(request, pk):
    if request.user.is_authenticated:
        article = Article.objects.get(pk=pk)
        if request.user == article.user: # 같은 유저 일때만
            article.delete()
            return redirect('articles:index')
    return redirect('articles:detail', article.pk)
```

## detail 페이지에서 user가 작성자와 일치할 때만 수정, 삭제 버튼 보이기

---

detail.html

```html
{% if request.user == article.user %}
    <a href="{% url 'articles:update' article.pk %}">UPDATE</a>
    <form action="{% url 'articles:delete' article.pk %}" method="POST">
      {% csrf_token %}
      <input type="submit" value="DELETE">
    </form>
{% endif %}
```

## Comment에 user 속성 넣기

---

models.py

```python
class Comment(models.Model):
    article = models.ForeignKey(Article, on_delete=models.CASCADE)
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.content
```

## 📑import 순서

---

1. 표준 라이브러리
2. 써드파티 라이브러리 (pip install 한거)
3. 장고 라이브러리
4. 프로젝트 내장  파일

import 자동 정렬해주는 라이브러리 → isort

사용법

```bash
pip install isort

isort [filename.py]
```
## QuerySet 사용 심화

---

- 내림차순 정렬하기: `User.objects.oreder_by('-age')`
- 랜덤정렬하기: `User.objects.oreder_by('?')`
- 특정 필드만 가져오기 : `User.objects.oreder_by('age').values('first_name', 'age)`
- 여러기준으로 정렬하기: `User.objects.order_by('age', '-balance')` (order_by를 연속으로 쓰는것이 아님⚠️)
- 중복 없이 조회하기: `User.objects.distinct().values('country')`
- 오름차순 정렬하고 중복없이 모든 지역 조회하기:  `User.objects.distinct().values('country').order_by('country')`
- 나이가 30인 사람의 이름 조회: `User.objects.filter(age=30).values('first_name')`
- 나이가 30 이상인 사람들의 이름과 나이 조회: `User.objects.filter(age__gte=30).values('first_name', 'age')`
- 나이가 30이상이고 잔고가 50만원 초과인 사람의 이름, 나이, 계좌잔고 조회:`User.objects.filter(age__gte=30, balance_gt=500000).values(’first_name’,’age’,’balance’)`
- 이름에 ‘호’가 포함된 사람의 이름과 성 조회: `User.objects.filter(first_name__contains='호').values('first_name','last_name')`
- 핸드폰 번호가 011로 시작하는 사람들의 이름과 핸드폰 번호 조회: `User.objects.filter(phone__startswith='011-').values('first_name','phone')`
- 이름이 준으로 끝나는 사람들의 이름 조회: `User.objects.filter(first_name__endswith='준').values('first_name')`
- 경기도 혹은 강원도에 사는 사람들의 이름과 지역 조회: `User.objects.filter(country__in=['강원도','경기도']).values('first_name','country')`
- 경기도 혹은 강원도에 살지 않는 사람들의 이름, 지역 조회: `User.objects.exclude(country__in=['강원도','경기도']).values('first_name', 'country')`
- 나이가 가장 어린 10명의 이름과 나이 조회: `User.objects.order_by('age').values('first_name','age')[:10]`
- 나이가 30 이거나 성이 김씨인 사람들 조회: `User.objects.filter(Q(age=30) | Q(last_name='김')).values('last_name')`

`values()` 

- 모델 인스턴스가 아닌 딕셔너리 요소를 가진 QuerySet을 반환한다.

`Field lookups`

필드 뒤쪽에 언더바2개(__) 붙이고 쓸 수 있음

- gte : 이상

`QuerySet  API로 OR 사용하기`

방법

1. `from django.db.models import Q`
2. q객체사용 해서 조합 (or → | )

## Aggregation

---

- 전체 queryset에 대한 값 계산
- 평균 , 합계, 최소 와 같은 값
- 집계 함수를 import 해서 사용
    
    ```python
    from django.db.models import Avg, Max, Sum, Count
    ```
    

예시

- 나이가 30 이상인 사람들의 평균 나이:`User.objects.filter(age__gte=30).aggregate(Avg(’age’))`
- 가장 높은 계좌 잔액 조회하기: `User.objects.aggregate(Max('balance'))`
- 모든 계좌 잔액 총액 : `User.objects.aggregate(Sum('balance'))`

결과 이름 바꾸기

- `User.objects.filter(age__gte=30).aggregate(Avg(’age’))` → `{'age__avg' : 36.20}`
- `User.objects.filter(age__gte=30).aggregate(ssafy = Avg(’age’))` → `{'ssafy' : 36.20}`

Grouping data

- `annotate()` 사용
- 예시
    - 각 지역별로 몇명씩 살고 있는지 조회: `User.objects.values('contry').annotate(Count('country'))`
    - 각 지역별로 몇명씩 살고 있는지 + 지역별 계좌 잔액 평균: `User.objects.values('contry').annotate(Count('country'), avg_balance=Avg('balance'))`
    - 각 성씨가 몇명씩 있는지: `User.objects.values('last_name').annotate(Count('last_name'))`

## Target model / Source model

---

- target model : 관계 필드를 가지지 않은 모델
- source model : 관계 필드를 가진 모델

## M:N관계

---

- 예시) 의사 - 환자

방법1 :테이블을 하나 만들어 두 테이블을 각각 N:1로 연결 (ex. patient - reservation - doctor)

- models.py
    
    ```python
    from django.db import models
    
    # Create your models here.
    class Doctor(models.Model):
        name = models.TextField()
    
        def __str__(self):
            return f'{self.pk}번 의사 {self.name}'
    
    class Patient(models.Model):
        name = models.TextField()
    
        def __str__(self):
            return f'{self.pk}번 환자 {self.name}'
    
    class Reservation(models.Model):
        doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
        patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
    
        def __str__(self):
            return f'{self.doctor_id}번 의사의 {self.patient.id}번 환자'
    ```
    

방법2: many to many필드 사용

- models.py

```python
from django.db import models

# Create your models here.
class Doctor(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'

class Patient(models.Model):
    doctors = models.ManyToManyField(Doctor)
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'

# hospitals_patient_doctors 모델 자동 생성 (키는 id, doctor_id, patient_id)

# 데이터 추가하기
# 환자 -> 의사 (참조)
# patient1.doctors.add(doctor1)
# patient1.doctors.all()
# 
# 의사 -> 환자 (역참조)
# doctor1.patient_set.add(patient1)
# doctor1.patient_set.all()
```

- 이렇게 하면 Patient → Doctor 는 참조가 되고, Doctor → Patient는 역참조가 됨
- 서로 관계를 바꾸고 싶으면 Doctor에 ManytoManyField를 넣으면 됨

## related_name

---

- 속성 이름을 바꿀 수 있음
    
    ```python
    from django.db import models
    
    # Create your models here.
    class Doctor(models.Model):
        name = models.TextField()
    
        def __str__(self):
            return f'{self.pk}번 의사 {self.name}'
    
    class Patient(models.Model):
        doctors = models.ManyToManyField(Doctor, related_name='patients')
        name = models.TextField()
    
        def __str__(self):
            return f'{self.pk}번 환자 {self.name}'
    ```
    

## M:N 관계에 추가 정보 입력(ex. 날짜, 진료내용)

---

- ManyToMany 필드의 `through` 옵션을 사용해서 중개 테이블 지정
    
    ```python
    from django.db import models
    
    # Create your models here.
    class Doctor(models.Model):
        name = models.TextField()
    
        def __str__(self):
            return f'{self.pk}번 의사 {self.name}'
    
    class Patient(models.Model):
        doctors = models.ManyToManyField(Doctor, related_name='patients', through='Reservation')
        name = models.TextField()
    
        def __str__(self):
            return f'{self.pk}번 환자 {self.name}'
    
    class Reservation(models.Model):
        doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
        patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
        symptom = models.TextField()
        reserved_at = models.DateTimeField(auto_now_add=True)
    
        def __str__(self):
            return f'{self.doctor_id}번 의사의 {self.patient.id}번 환자'
    
    # 데이터추가 방법 1:
    # reservation1 = Reservation(doctor=doctor1, patient=patient1, symptom="headache")
    # reservation1.save()
    
    # 데이터 추가 방법 2:
    # patient2.doctors.add(doctor1, through_defaults={'symptom' : 'flu'})
    ```
    

## 예약 삭제

---

`doctor1.patients.remove(patient1)`

or

`patient1.doctors.remove(doctor1)`

## ManyToManyField

---

- 다대다 관계에서 사용
- add, create, remove 등 메서드 있음

속성

`through`

- M:N에 부가적 속성 넣을 테이블 지정할 때 사용

`symmetrical`

- 동일 모델(on self)를 가리키는 정의에서 사용(default = True)
- ex) Person 모델의 friends 필드 (M:N 재귀 다대다)
- 시메트리컬을 끄고 재귀로 관계를 맺으면 한명이 친구를 추가했을 때 그 친구에는 본인이 추가가 안됨
- 인스타 같은 경우 셀럽의 경우 팔로워가 팔로잉하는 사람보다 훨씬 많으므로 이 속성을 끄고 저장해야함⚠️

## M:N (Article -User ) 좋아요

---

models.py에 필드 추가

```python
from django.db import models
from django.conf import settings

# Create your models here.
class Article(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    like_users = models.ManyToManyField(settings.Auth_USER_MODEL) # 좋아요 추가
    title = models.CharField(max_length=10)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=Tru

class Comment(models.Model):
    article = models.ForeignKey(Article, on_delete=models.CASCADE)
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    content = models.CharField(max_length=200)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.content
```

- but 이방식은 migration 오류 발생
- 이유: `User.article_set` 을 할때 user와 like_users에서 둘다 참조를 했으므로 어떤 것을 역참조 해야할 지 알 수 없음
- 따라서 둘 중 하나에  `related_name` 속성을 지정해 줘야함
- 일반적으로 M:1 에다가 아니라 M:N에 지정하는 편

수정후

```python
from django.db import models
from django.conf import settings

# Create your models here.
class Article(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    like_users = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name="like_articles")
    title = models.CharField(max_length=10)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

class Comment(models.Model):
    article = models.ForeignKey(Article, on_delete=models.CASCADE)
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    content = models.CharField(max_length=200)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.content
```

이후 과정

1. urls.py에 like url 추가 
`path('int:article_pk/likes/', views.likes, name='likes')`
2. views.py에 like 메서드 추가
    
    ```python
    @require_POST
    def likes(request, article_pk):
        if request.user.is_authenticated:
            article = Article.objects.get(pk=article_pk)
    
            # 현재 게시글에 좋아요를 누른 유저 목록에 좋아요를 요청하는 유저가 있는지 없는지를 확인
            # 방법1 (테이블 크기 커지면 속도 느려짐)
            # if request.user in article.like_users.all():
            # 방법2 (방법 1보다 빠름)
            if article.like_users.filter(pk=request.user.pk).exists():
                # 좋아요 취소 (remove)
                article.like_users.remove(request.user)
            else:
                # 좋아요 추가 (add)
                article.like_users.add(request.user)
            return redirect('articles:index')
        else:
            return redirect('accounts:login')
    ```
    
3. index.html에 좋아요 누르는 코드 추가
    
    ```python
    {% extends 'base.html' %}
    
    {% block content %}
      <h1>Articles</h1>
      {% if request.user.is_authenticated %}
        <a href="{% url 'articles:create' %}">CREATE</a>
      {% endif %}
      <hr>
      {% for article in articles %}
        <p><b>작성자 : {{ article.user }}</b></p>
        <p>글 번호 : {{ article.pk }}</p>
        <p>제목 : {{ article.title }}</p>
        <p>내용 : {{ article.content }}</p>
        <div>
          <form action="{% url 'articles:likes' article.pk %}" method="POST">
            {% csrf_token %}
            {% if request.user in article.like_users.all %}
              <input type="submit" value="좋아요 취소">
            {% else %}
              <input type="submit" value="좋아요">
            {% endif %}
          </form>
        </div>
        <a href="{% url 'articles:detail' article.pk %}">상세 페이지</a>
        <hr>
      {% endfor %}
    {% endblock content %}
    ```
    

## exists 메서드

---

- QuerySet에 결과가 포함돼있으면 `True`, 아니면 `False`

# M:N (User-User)

---

- 자연스러운 흐름을 위해 Profile 페이지 먼저 작성

`순서`

1. url에 profile 페이지 추가 → `path('profile/str:username/', views.profile, name='profile')`
2. views함수에 profile메서드 추가
    
    ```python
    def profile(request, username):
        User = get_user_model()
        person = User.objects.get(username=username)
        context = {
            'person' : person,
        }
        return render(request, 'accounts/profile.html', context)
    ```
    
3. base.html에서 username 누르면 profile갈수있게 수정
    
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-gH2yIJqKdNHPEq0n4Mqa/HGKIhSkIHeL5AyhkYV8i59U5AR6csBvApHHNl/vI1Bx" crossorigin="anonymous">  <title>Document</title>
    </head>
    <body>
      <div class="container">
        {% if request.user.is_authenticated %}
          <a href="{% url 'accounts:profile' user.username %}">  
            <h3>{{ user }}</h3>
          </a>
            <form action="{% url 'accounts:logout' %}" method="POST">
            {% csrf_token %}
            <input type="submit" value="Logout">
          </form>
          <form action="{% url 'accounts:delete' %}" method="POST">
            {% csrf_token %}
            <input type="submit" value="회원탈퇴">
          </form>
          <a href="{% url 'accounts:update' %}">회원정보수정</a>
        {% else %}
          <a href="{% url 'accounts:login' %}">Login</a>
          <a href="{% url 'accounts:signup' %}">Signup</a>
        {% endif %}
        <hr>
        {% block content %}
        {% endblock content %}
      </div>
      <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0/dist/js/bootstrap.bundle.min.js" integrity="sha384-A3rJD856KowSb7dwlZdYEkO39Gagi7vIsF0jrRAoQmDKKtQBHUuLZ9AsSv4jD4Xa" crossorigin="anonymous"></script>
    </body>
    </html>
    ```
    
4. index.html에서 작성자 이름 누르면 profile 들어갈 수 있게 수정
    
    ```html
    {% extends 'base.html' %}
    
    {% block content %}
      <h1>Articles</h1>
      {% if request.user.is_authenticated %}
        <a href="{% url 'articles:create' %}">CREATE</a>
      {% endif %}
      <hr>
      {% for article in articles %}
        <p>
          <b>
            작성자 : <a href="{% url 'accounts:profile' article.user.username %}">{{ article.user }} </a>
          </b>
        </p>
        <p>글 번호 : {{ article.pk }}</p>
        <p>제목 : {{ article.title }}</p>
        <p>내용 : {{ article.content }}</p>
        <div>
          <form action="{% url 'articles:likes' article.pk %}" method="POST">
            {% csrf_token %}
            {% if user in article.like_users.all %}
              <input type="submit" value="좋아요 취소">
            {% else %}
              <input type="submit" value="좋아요">
            {% endif %}
          </form>
        </div>
        <a href="{% url 'articles:detail' article.pk %}">상세 페이지</a>
        <hr>
      {% endfor %}
    {% endblock content %}
    ```
    
5. profile.html 작성
    
    ```html
    {% extends 'base.html' %}
    
    {% block content %}
    <h1>{{ person.username }}님의 프로필</h1>
    <h1>{{ person.username }}님이 작성한 모든 게시글</h1>
    {% for article in person.article_set.all %}
    <div>
        {{ article.title }}
    </div>
    {% endfor %}
    
    <h1>{{ person.username }}님이 작성한 모든 댓글</h1>
    {% for comment in person.comment_set.all %}
    <div>
        {{ comment.content }}
    </div>
    {% endfor %}
    
    <h1>{{ person.username }}님이 좋아요 한 모든 게시글</h1>
    {% for article in person.like_articles.all %}
        <div>
            {{ article.title }}
        </div>
    {% endfor %}
    
    <a href="{% url 'articles:index' %}">BACK</a>
    {% endblock content %}
    ```
    
    `follow` 기능 추가 순서
    
    1. [models.py](http://models.py)에 followings 추가
    2. url에 follow 추가 → `path('int:user_pk/follow/', views.follow, name='follow'),`
    3. views에 follow 메서드 추가
        
        ```python
        def follow(request, user_pk):
            User = get_user_model()
            me = request.user
            you = User.objects.get(pk=user_pk)
            if me != you:
                if you.followers.filter(from_user_id=me.pk).exists():
                    you.followers.remove(from_user_id=me.pk)
                else:
                    you.followers.add(from_user_id=me.pk)
            return redirect("accounts:profile", you.username)
        ```
        
    4. profile.html에 follow 버튼 추가
        
        ```html
        {% extends 'base.html' %}
        
        {% block content %}
        <h1>{{ person.username }}님의 프로필</h1>
        <div>
            {% if request.user != person %}
                <form action="{% url 'accounts:follow' person.pk %}" method="POST">
                    {% csrf_token %}
                    {% if request.user in person.followers.all %}
                        <input type="submit" value="언팔로우">
                    {% else %}
                        <input type="submit" value="팔로우">
                    {% endif %}
                </form>
            {% endif %}
        </div>
        <h1>{{ person.username }}님이 작성한 모든 게시글</h1>
        {% for article in person.article_set.all %}
        <div>
            {{ article.title }}
        </div>
        {% endfor %}
        
        <h1>{{ person.username }}님이 작성한 모든 댓글</h1>
        {% for comment in person.comment_set.all %}
        <div>
            {{ comment.content }}
        </div>
        {% endfor %}
        
        <h1>{{ person.username }}님이 좋아요 한 모든 게시글</h1>
        {% for article in person.like_articles.all %}
            <div>
                {{ article.title }}
            </div>
        {% endfor %}
        
        <a href="{% url 'articles:index' %}">BACK</a>
        {% endblock content %}
        ```
        
        ## Fixtures
        
        ---
        
        - DJANGO 에서 쓴 데이터(DB)를 다른 컴퓨터에도 쓸 수 있도록 공유하는 방법
        - 생성과 로드 과정 필요
        - 데이터는 기본적으로 프로젝트 최상위 폴더에 생김
        - static, templates와 같이 앱별로 fixtures 폴더 만들어서 데이터 넣어줘야함
        
        `생성`
        
        dumpdata 사용
        
        사용예시)
        
        ```bash
         python manage.py dumpdata --indent 4 articles.article > articles.json
        ```
        
        `로드`
        
        loaddata 사용
        
        사용예시)
        
        ```bash
        python manage.py loaddata articles.json users.json comments.json
        ```
        
        ## Query 최적화
        
        ---
        
        방법1 : `articles = Article.objects.annotate(Count('comment')).order_by('-pk')`
        
        방법2 : `articles = Article.objects.select_related('user').order_by('-pk')`
        
        방법3 : `articles = Article.objects.prefetch_related('comment_set').order_by('-pk')`
        
        방법4 : 
        
        ```python
        articles = Article.objects.prefetch_related(
                Prefetch('comment_set', queryset=Comment.objects.select_related('user'))
            ).order_by('-pk')
        ```