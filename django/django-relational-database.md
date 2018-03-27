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
## select_related , prefetch_related
> 하나의 QuerySet을 가져올 때, 미리 related objects 까지 불러와주는 함수.
### select_related 
one_to_one , foreignKey에서 사용
```python
# models.py 
class City(models.Model):
    name = models.CharField()
class Person(models.Model):
    city = models.ForeignKey(City)
class Pet(models.Model):
    person = models.ForeginKey(Person)

pet = Pet.objects.get(pk=1)
person = pet.person
city = person.city # DB에 3번 접근함.

pet = Pet.objects.select_related('person__city').get(pk=1)
# related objects까지 뽑아와 캐시에 저장함 person,city를 접근하기 위해 db를 접근하지 않고 캐시에서 꺼내쓰면 됨.
person = pet.person
city = person.city 

```
### prefetch_related
모든 relationships 에서 사용 가능함.
```python
# models.py
class Language(models.Model): # person 모델과 N:M 관계를 가짐
    person_set = models.ManyToManyField(Person)

# Person.objects.all() 안에있는 person마다 person.language_set.all() 쿼리가 실행됨 
# person이 100개 있다면 person.language_set.all() 이라는 쿼리가 DB로 100번 날라감.
people = Person.objects.all()
for person in people:
    for language in person.language_set.all():
        ...

# Person.objects.all() query가 실행되고 동시에 self.language_set.all()이라는 별도의 query가 실행되어 data들이 cache에 저장됨.
# person 수만큼 person.language_set.all()이 실행되더라도 DB에 접근하지 않고 cache에서 찾아서 쓰게됨. 2번의 쿼리

people = Person.objects.all.prefetch_related('language_set')
for person in people:
    for language in person.language_set.all():
        ...
```
### 차이점
prefetch_related는 원래의 main query가 실행 된 후 별도의 query를 따로 실행됨.  
select_related는 하나의 query 만으로 related objects를 다 가져옴.
```python
Pet.objects.prefetch_related('person') # 2 queries
Pet.objects.select_related('person') # 1 queries 
```
### 적절한 사용 방법
```pyton
Pet.objects.prefetch_related('person__language_set')
```
총 3번의 query가 이루어짐 
1. Pet의 모든 instance를 가져오기 위한 query
2. 그 Pet instance들의 person을 가져오기 위한 query
3. 그 person instance들의 language_set을 가져오기 위한 query  

##### select_related를 적절히 사용하여 쿼리 수를 줄인다.
```python
Pet.objects.select_related('person').prefetch_related('person__language_set')
```
select_related에서 person에 대한 data를 모두 가져오고 prefetch_related에서 person은 cache를 통해 가져오고 language_set 만 DB에서 fetch 해오면 됨.  
총 2번의 query가 이루어짐
1. Pet의 모든 instance와 그 instance의 person을 가져오는 query
2. 그 person들의 language_set을 가져오기 위한 query (manytomany이기 때문)

## refer
http://freeprog.tistory.com/55  
[TypeError: int() argument must be a string or a number, not 'User' 해결](https://code.djangoproject.com/ticket/23454)  
https://docs.djangoproject.com/en/2.0/topics/db/examples/many_to_many/  
https://stackoverflow.com/questions/13403010/django-manytomany-from-string-in-createview  
https://stackoverflow.com/questions/1226290/django-manytomany-relation-add-error  
https://stackoverflow.com/questions/4959499/how-to-add-multiple-objects-to-manytomany-relationship-at-once-in-django  
http://jupiny.tistory.com/entry/selectrelated%EC%99%80-prefetchrelated