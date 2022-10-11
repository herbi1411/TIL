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