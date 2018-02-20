# 장고 핵심 기술
## Admin
- 데이터베이스에 들어있는 데이터를 쉽게 관리할 수 있도록 데이터의 생성,조회,변경,삭제 등의 기능을 제공함.
```python
# admin.py
class QuestionAdmin(admin.ModelAdmin):
    fields = ['pub_date','qeustion_text'] # 화면에 보여지는 필드를 정하거나 필드의 순서를 변경할 때 AdminClass를 지정함.

admin.site.register(Question,QuestionAdmin) # 어드민 사이트에 모델 객체를 표시.
```
## python shell
```python
python manage.py shell
```
- 위 명령어를 사용하여 파이썬 쉘에서 데이터 처리기능(CRUD)을 할 수 있음.
- SQL 쿼리 문장을 사용하지 않고 데이터베이스 처리를 할 수 있음.
### Create
- 데이터 생성 (SQL-insert)
- 테이블에 레코드를 생성하기 위해 필드 값을 지정하여 객체 생성 후 save() 메소드를 호출함.
    + save() 명령이 실행되기 전에는 메모리에서만 변경된 것이므로 데이터베이스에 반영하기 위해 save()명령을 해야함
```python
>>> from polls.models import Question,Choice
>>> from django.utils import timezone
>>> q = Question(question_text="what's new?",pub_date=timezone.now())
>>> q.save()
```
### Read 
- 데이터 조회 (SQL-select)
- 필터를 가질 수 있고 필터를 사용하여 검색 결과에서 주어진 파라미터로 조건에 맞는 레코드만 추출함.
    + (SQL-where,limit)
- 데이터베이스로부터 데이터를 조회하기 위해서는 QuerySet 객체를 사용함.
    > QuerySet : 데이터베이스 테이블로부터 꺼내온 객체들의 콜렉션
        >> 콜렉션 : 다수의 객체를 한 곳에 모아서 각 객체를 동일한 방식으로 다룰 수 있도록 해주는 데이터 구조.쉽게 말해서 여러 객체의 모임
- 조회 결과를 담는 QuerySet을 얻으려면 objects 객체를 사용함
    > objects : 테이블의 정보를 담고있는 객체
#### 모든 레코드 조회
```python
>>> Question.objects.all() # 테이블에서 모든 Question 객체를 담고있는 QuerySet 콜렉션을 반환함.
<QuerySet [<Question: what are you dogin?>, <Question: what's new?>]> # QuerySet 콜렉션 반환
```
#### 조건에 맞는 레코드를 조회
- filter() 
    + 주어진 조건에 맞는 객체들을 담고 있는 QuerySet 콜렉션 반환.
- exclude()
    + 주어진 조건에 맞지 않는 객체들을 담고 있는 QuerySet 콜렉션 반환.
- get()
    + 한개의 요소만 검색하는 경우
- QuerySet 요소의 개수를 제한하기 위하여 파이썬의 배열 슬라이싱 문법을 사용할 수 있음.(SQL-offset)
```python
>>> Question.objects.all()[:3]
```

### Update
- 데이터 수정(SQL-update)
- 이미 존재하는 객체의 필드 값을 수정하는 경우 필드 속성 값을 수정한 후 save() 메소드를 호출함.
```python
>>> q = Question.objects.get(pk=2)
>>> q.question_text = 'What is your hobby?'
>>> q.save()
```
- 여러개의 객체를 한꺼번에 수정하는 경우 update() 메소드를 사용함.
```python
>>> Question.objects.filter(pub_date__year=2018).update(question_text='Everything is the same')
2
>>> Question.objects.all()
<QuerySet [<Question: Everything is the same>, <Question: Everything is the same>]>
```

### Delete 
- 데이터 삭제(SQL-delete)
- delete() 메소드를 사용한다.
```python
>>> Question.objects.get(pk=3).delete()
(1, {u'polls.Choice': 0, u'polls.Question': 1})
```

### 속성에 접근하기
```python
>>> q = Question.objects.get(pk=1)
>>> q.id
1
>>> q.pub_date
datetime.datetime(2018, 2, 20, 4, 44, 5, 248000, tzinfo=<UTC>)
>>> q.question_text
u'What do you like best?'
```

### python shell 실습
```python
>>> Question.objects.filter(question_text__startswith='Everything')
<QuerySet [<Question: Everything is the same>]>
>>> from django.utils import timezone # datetime 보다 좋음
>>> current_year = timezone.now().year
>>> current_year
2018
>>> Question.objects.filter(pub_date__year=current_year)
<QuerySet [<Question: Everything is the same>, <Question: What do you like best?>, <Question: where do tou live?>, <Question: what>]>
>>> Question.objects.filter(pk=2)
<QuerySet [<Question: Everything is the same>]>
>>> Question.objects.filter(id=2)
<QuerySet [<Question: Everything is the same>]> # id == pk
# Question과 Choice 테이블은 1:N 관계 FK choice_set api 제공
>>> q = Question.objects.get(pk=2)
>>> q
<Question: Everything is the same>
>>> q.choice_set
<django.db.models.fields.related_descriptors.RelatedManager object at 0x0583C410>
>>> q.choice_set.all()
<QuerySet []>
>>> q.choice_set.create(choice_text='Sleeping',votes=0)
<Choice: Choice object>
>>> q.choice_set.create(choice_text='Playing',votes=0)
<Choice: Choice object>
>>> c=q.choice_set.create(choice_text='Eating',votes=0)
>>> c.question
<Question: Everything is the same>
>>> q.choice_set.all()
<QuerySet [<Choice: Choice object>, <Choice: Choice object>, <Choice: Choice object>]>
>>> q.choice_set.count()
3
>>> Choice.objects.filter(question__pub_date__year=current_year)
<QuerySet [<Choice: Choice object>, <Choice: Choice object>, <Choice: Choice object>]>

>>> c = q.choice_set.filter(choice_text__startswith='Play')
>>> c.delete()
(1, {u'polls.Choice': 1})
>>> q.choice_set.all()
<QuerySet [<Choice: Choice object>, <Choice: Choice object>]>
>>>
```