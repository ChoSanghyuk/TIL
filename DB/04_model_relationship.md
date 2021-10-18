# Model Relationship1



## Foreign Key

### 개념

- 외래 키

- db에서 한 테이블의 필드 중 다른 테이블의 행을 식별할 수 있는 key

  : 참조되는 측 테이블의 기본키를 가리킴

- 참조하는 테이블의 행 여러 개가, 참조되는 테이블의 동일한 행을 참조 할 수 있임

  : 1:N



### 특징

- 키를 사용하여 부모 테이블의 유일한 값을 참조 (**참조 무결성**)

- 외래 키의 값이 반드시 부모 테이블의 기본키일 필요는 없지만 유일한 값이어야 함



### *ForeignKey field

- A many-to-one relationship
- 2개의 위치 인자가 반드시 필요
  1. 참조하는 model class
  2. on delete 옵션

```python
class Comment(models.Model):
    article = models.ForeignKey(Article, on_delete=models.CASCADE)
    ......
```

- 변수 이름은 참조하려는 모델 이름으로 설정.

  =>  article_id 란 이름으로 자동 저장



### ForeignKey arguments = 'on_delete'

- 외래 키가 참조하는 객체가 사라졌을 때 외래 키를 가진 객체를 어떻게 처리할 지를 정의
- **데이터 무결성**을 위해 매우 중요한 설정
- on_delete 옵션에 사용 가능한 값들
  - CASCADE :	부모 객체가 삭제 됐을 때 이를 참조하는 객체도 삭제
  - PROTECT
  - SET_NULL
  - SET()
  - DO_NOTHING
  - RESTRICT



### *shell_plus 실습

- Article과 Comment model이 존재

```shell
article = Article.objects.create(title="title", content="content")
comment = Comment()
comment.content = "content"
comment.article = article
```

- 이 처럼 comment에 article_id 값을 넣어주는 것이 아닌, article 속성에 Article 객체를 넣음

  => 이러면 comment.article_id 도 자동 사용 가능



### 1:N 관계 related manager

- 역참조('comment_set')
  - Article(1) => Comment(N)
  - article.comment 형태로 사용 x. `article.comment_set` manger가 생성됨
  - 게시글에 몇 개의 댓글이 작성되었는지 Django ORM이 보장할 수 없기 때문
    - 실제로 Article 클래스에는 Comment 와의 어떠한 관계도 작성되어 있지 않음
  - 역참조 매니저 이름을 변경할 수 있음 (comment_set => comments)

  ```python
  class Comment(models.Model):
      article = models.ForeignKey(Article, on_delete=models.CASCADE, related_name="comments")
  ```

  

- 참조('article')

  - Comment(N) => Article(1)
  - comment.article과 같이 접근 o





## Comment Create Read Delete



### *comments_create views

```python
# articles/views.py
@require_POST
def comments_create(request, pk):
    if request.user.is_authenticated:
        article = get_object_or_404(Article, pk=pk)
        comment_form = CommentForm(request.POST)
        if comment_form.is_valid():
            comment = comment_form.save(commit=False)
            comment.article = article
            comment.save()
        return redirect('articles:detail', article.pk)
    return redirect('accounts:login')
```

- save(commit=False)
  - Create, but don't save the new instance
  - 아직 db에 저장되지 않은 인스턴스 반환



### *comments_read views

- 댓글은 article의 detail 페이지에서 확인

```python
# articles/views.py

@require_safe
def detail(request, pk):
    article = get_object_or_404(Article, pk=pk)
    comment_form = CommentForm()
    comments = article.comment_set.all()
    context = {
        'article': article,
        'comment_form': comment_form,
        'comments': comments,
    }
    return render(request, 'articles/detail.html', context)
```



### *comments_delete views

```python
@require_POST
def comments_delete(request, article_pk, comment_pk):
    if request.user.is_authenticated:
        comment = get_object_or_404(Comment, pk=comment_pk)
        comment.delete()
    return redirect('articles:detail', article_pk)
```











