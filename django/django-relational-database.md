# Django 관계형 데이터베이스
## ManytoMany(N:N)
> 다대다 관계를 정의하기 위해서 사용한다.
### models
다대다 관계를 사용하기 위해서는 관계를 성립할 두 모델 중 한 모델에 ManyToManyField를 정의한다.
```python
class Tag(models.Model):
    tag = models.CharField(max_length=20)
class Post(models.Model):
    content = models.CharField(max_length=200)
    tags = models.ManyToManyField(Tag) # N:N 관계 성립
```
### View에서 ManyToMany 관계 생성
```python
> post = Post('This is Post content')
> post.save()

> tag = Tag('hashTag')
> tag.save() # 꼭 save() 안하면 에러남

# tag 등록
> post.tags.add(tag) # post에 tag 등록
> new_tag = post.tags.create(tag='asdasd') # 태그 생성/등록하기 

# 여러 tag 등록
# list 형식으로 한꺼번에 많은 tag를 등록할 수 있다.
> tags = [tag1,tag2,tag3]
> post.tags.add(*tags)
```
### ManyToMany model 조회
```python
# Post(관계 정의한 모델)에서 조회
> post = Post.objects.get(pk=1)
> post.tags.all()
<QuerySet [<Tag: 태그>, <Tag: 테스트합니당>, <Tag: 하핫>]>

# Tag에서 조회
> tag = Tag.objects.get(pk=1)
> tag.post_set.all()
<QuerySet [<Post: 하핫 ㅋㅋㅋㅋㅋ 테스트>, <Post: 태그 테스트합니당 하핫>]
```
## 관계 매니저 _set
> 어떤  model에서 자신을 foreingKey로 가지고 있는 모델에 접근하기 위해 Manager를 이용함.
### Manager 사용법
```python
model.model2_set.all()
# model2 QuerySet 반환
```
###  _set 사용법
```python
# models 
class User(models.Model):
    username = models.CharField(max_length=25)
    ...
class Post(models.Model):
    user = models.ForeignKey(User)
    content = models.TextField()

> u = User.objects.get(pk=1)
> u.post_set.all() # user에 맞는 post를 모두 뽑음

> u.post_set.filter(content='This is content') # user에 맞고 post 조건에 맞는 post를 뽑아옴 
```

## refer
http://freeprog.tistory.com/55  
[TypeError: int() argument must be a string or a number, not 'User' 해결](https://code.djangoproject.com/ticket/23454)  
https://docs.djangoproject.com/en/2.0/topics/db/examples/many_to_many/  
https://stackoverflow.com/questions/13403010/django-manytomany-from-string-in-createview  
https://stackoverflow.com/questions/1226290/django-manytomany-relation-add-error  
https://stackoverflow.com/questions/4959499/how-to-add-multiple-objects-to-manytomany-relationship-at-once-in-django