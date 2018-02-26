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
    >> 콜렉션 : 다수의 객체를 한 곳에 모아서 각 객체를 동일한 방식으로 다룰 수 있도록 해주는 데이터 구조.쉽게 말해서 여러 객체의 모임.
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
## 템플릿
- 로직을 표현하는 것이 아닌 사용자에게 어떻게 보여줄 지 화면 구현을 하는 것
- 템플릿 문법으로 작성된 템플릿 코드를 해석하여 템플릿 파일로 결과물을 만듬.(랜더링)
- 결과물인 템플릿 파일 : html,xml 등

### 템플릿 문법
#### 변수
```python
{{ variable }}
```
- {{ foo.bar }} 변수를 해석하는 법
    1. foo가 사전타입인지 확인, 맞다면 foo['bar']로 해석
    2. foo의 속성을 찾음, bar라는 속성이 있으면 foo.bar로 해석
    3. 위 두개가 아니라면 리스트인지 확인, 맞다면 foo[bar]로 해석

#### 템플릿 필터
> 객체나 처리 결과에 추가적으로 명령을 적용하여 해당 명령에 맞게 최종 결과를 변경하는 것
- 파이프(|)문자를 사용함.
- 체인 형태로 연결 가능함.
```python
{{ name|lower }} # name 변수 값의 모든 문자를 소문자로 바꿈

{{ text|escape|linebreaks }} # 특수문자를 escape 해주고 그 결과 스트링을 html <p>태그에 붙여줌

{{ bio|trunateword:30 }} # 앞의 30개의 단어만 보여주고 줄바꿈 문자는 모두 없앰

{{ list|join:'//' }} # 각 리스트를 연결해줌.
# list = ['a','b','c'] 
# a // b // c

{{ vlaue|default:"nothing" }} # value 변수 값이 false 이거나 없는 경우 "nothing"을 출력

{{ value|length }} # value의 길이를 출력함

{{ value|striptags }} # value 값에서 html 태그를 모두 없애줌(100% 보장은 아님)

{{ value|pluaralize }} # 복수 접미사 필터
# es,ies를 붙일 경우
{{ value|plualize:"es" }} {{ value|pluralize:"y,ies"}} 

{{ value|add:2 }} # 더하기 필터 value에 2를 더해줌
# 타입이 허용한느 문법에 따라 더하기를 시도함, 실패할 경우 빈 문자열을 반환
# {{ fisrt|add:second }}
# 문자열 더하기 first="python" second="django " , "pythondjango"
# 리스트 더하기 first=[1,2,3] second=[4,5,6] , [1,2,3,4,5,6]
# 자동 형변환 first="5" second="10" , 15
```
#### 탬플릿 태그
```python
{% for %}
```
- 리스트에 담겨있는 항목들을 순회하며 출력할 수 있다.
- for 태그에 사용되는 변수
    - {{ forloop.counter }} - 현재까지 루프를 실행한 루프카운트(1부터 카운트)
    - {{ forloop.counter0 }} - 위와 동일(0부터 카운트)
    - {{ forloop.revcounter }} - 루프 끝부터 현재가 몇번째인지 카운트 (1부터 카운트)
    - {{ forloop.revcounter0 }} - 위와 동일,(0부터 카운트)
    - {{ forloop.first }} - 루프에서 첫번째 실행이면 True 값을 가짐
    - {{ forloop.last }} - 루프에서 마지막 실행이면 True 값을 가짐
    - {{ forloop.parentloop }} - 중첩된 루프에서 현재의 루프 바로 상위의 루프를 의미
```python
{% if %}
```
- {% if athlete_list|length > 1 %} 같은 필터와 연산자 사용가능(length 필터는 숫자를 반환하기 때문에 가능)
```python
{% csrf_token %}
```
- POST 방식의 form을 사용하는 템플릿 코드에서 CSRF 공격을 방지하기 위하여 사용하며 form 앨리먼트 첫줄 다음에 넣어줌.
    - csrf(cross-site-request-forgery)
    > 사이트간 요청 위조 공격, 특정 웹사이트에서 이미 인증을 받은 사용자를 이용하여 공격을 시도하며
    인증을 받은 사용자가 공격 코드가 삽입된 페이지를 열면 공격 대상이 되는 웹사이트는 공격 명령이 믿을 수 있는 사용자로부터 발송하게 된 것으로 판단하여 공격을 받게되는 방식
- 장고는 내부적으로 CSRF 토큰 값의 유효성을 검증하며 검증 실패시 사용자에게 403 에러를 보여줌.
- 외부 url로 보내는 form에는 CSRF 토큰 값이 유출될 수 있으므로 사용하면 안된다.
```python
{% url %}
```
- URLConf 모듈을 참조하여 원하는 url을 추출함.
```python
{% with %}
# ex
{% with total = business.employees.count %}
    {{ total }} people works at business
{% endwith%}
```
- 특정 값을 변수에 저장해두는 기능
- 변수의 유효범위는 with 구문 내
- 데이터베이스를 조회하는 것 처럼 부하가 큰 동작의 결과를 저장해두고 다시 동일한 동작이 필요한 경우 저장해둔 결과를 활용하여 부하를 줄인다.
```python
{% load %}
```
- 사용자 정의 태그 및 필터를 로딩해줌.
```
{# 템플릿 한 줄 주석 문 #}
{% commet "note" %}
<p>템플릿 여러 줄 주석 문</p>
{% endcomment%}
```
- 템플릿의 한 줄 주석 문과 여러 줄 주석문. 
- 중첩하여 사용하지 못한다.
```python
{% autoescape off %}
{% endautoescape %}
```
- 장고의 자동 이스케이프를 비활성화 한다.
    - html 자동 이스케이프
        - 사용자가 입력한 데이터를 그대로 랜더링하지 않고 예약 문자들을 예약 의미를 제거한 문자로 변경해준다.
        ```
        < (less then) - &lt; 
        ```
- safe 필터로 템플릿 변수에만 비활성화 할 수 있다.
    ```
    this will not be escaped : {{ data|safe }}
    ```
#### 템플릿 상속
- 상속을 통하여 템플릿 코드를 재활용 할 수 있다.
```python
{% extends %} # 상속을 받는 다는 것을 표시하기 위하여 사용한다.
# ex) {% extends 'base.html' %}
{% block %} # 부모 템플릿에서 block 부분을 지정하면 그 부분을 자식 템플릿이 오버라이딩한다.
```
- 부모 템플릿의 block 내용을 그대로 사용하고 싶은 경우 {{ block.super }} 를 사용한다.

## 장고의 폼 처리
- 장고는 폼 처리를 위하여 3가지 기능을 제공함.
    1. 폼 생성에 필요한 데이터를 폼 클래스로 구조화하기
    2. 폼 클래스의 데이터를 랜더링하여 html 폼 만들기
    3. 사용자로부터 제출된 폼과 데이터를 수신하고 처리하기
### 폼 클래스
- 폼클래스는 폼을 기술하고 폼이 어떻게 보이는지 결정함.
- 폼 클래스의 html 폼의 input 앨리먼트에 매핑되고 폼 데이터를 저장하고 있으며 폼이 제출되면 자신의 타입에 따라 자신의 데이터에 대한 유효성 검사를 실시함.
- ex) DateField와 FileField 필드타입은 서로 다른 데이터를 처리하고 유효성 검사 방식도 다르며 디폴트 위젯 클래스도 다름.
### 장고의 폼 객체 랜더링 과정
- 폼은 객체면서 템플릿의 일부이므로 템플릿 코드에 포함되어 랜더링 절차를 거침.
    1. 랜더링할 객체를 뷰로 가져옴
    2. 그 객체를 템플릿 시스템에 넘겨줌
    3. 템플릿 문법을 처리하여 html 마크업 언어로 변환
- 폼 객체는 보통 랜더링 이후 사용자가 데이터를 채우기 때문에 뷰 함수에서 폼 객체를 생성할 때 데이터 없이 만들 것인지, 데이터를 채워서 만들 것인지 구분하여 코딩해야함.
    - 언바운드 폼 : 랜더링 되어 사용자에게 보여질 때 폼은 비어있거나 디폴트로 채워짐
    - 바운드 폼 : 제출된 데이터를 갖고 있어 데이터의 유효성을 검사하는데 사용

### 폼클래스 생성
```python
from django import forms

class NameForm(forms.Form):
	your_name = forms.CharField(label='Your name',max_length=100)

```
위 코드가 랜더링 되면
```html
<label for="your name">Your name:</label>
<input type="text" name="your_name" max_length="100">
```
요로케 된다.
- 폼 필드는 위젯 클래스를 갖고있고 위젯 클래스는 <input type="text> 와 같은 html 폼 위젯으로 대응된다.
```python
# 위젯 변경하기
your_name = forms.CharField(label="Your name",max_length=100,widget=forms.Textarea)
```

### 뷰에서 폼 클래스 처리
- 폼을 처리하는 뷰는 폼을 보여주는 뷰, 제출된 폼을 처리하는 뷰 2개가 필요하는데 장고는 두개의 뷰를 하나의 뷰로 통합하여 폼을 처리하는 것을 권장한다.
- HTTP 메소드로 사용자에게 보여주는 폼(GET), 사용자가 데이터를 입력한 후 제출된 폼(POST)을 구분.
#### is_valid()
- 모든 필드에 대해 유효성 검사 루틴을 실행시킨다. 
- is_valid() 메소드가 호출되어 유효성 검사를 하고 그 결과 모든 필드가 유효할 시 하는 일
    - True 반환
    - 폼 데이터를 cleaned_data 속성에 넣는다.
```python
# views.py
from django.http import HttpResponse,HttpResponseRedirect
from django.shortcuts import render

def get_name(request):
    if request.method=='POST': # 데이터가 담긴 제출된 폼일 경우
        form = NameForm(request.POST) # request에 담긴 데이터로 클래스 폼을 생성
        if form.is_valid(): # 폼의 데이터가 유효한지 체크
            new_name = form.cleaned_data['name'] #폼 데이터가 유효하면 데이터는 cleaned_data로 복사됨
            return HttpResponseRedirect('/thanks/')
    else: # GET 요청일 경우
        form = NameForm() # 빈 폼을 사용자에게 보여줌.
    return render(request,'name.html',{'form':form})
```
### 폼 클래스를 템플릿으로 변환
```python
{{ form }}
```
- form 변수는 뷰에서 컨텍스트 변수를 포함하여 템플릿 시스템으로 넘겨줌.
- 위의 변수를 html 태그에 감싸는 기능도 제공한다.
```python
{{ form.as_table }} # <tr> 태그로 감싸 테이브 셀로 렌더링됨
{{ form.as_p }} # <p> 태그로 감싸도록 렌더링
{{ form.as_li }} # <li> 태그로 감싸도록 렌더링 
```
- table,ul 태그는 개발자가 직접 추가해야한다.
## 클래스형 뷰
- 클래스로 만들어진 뷰로,상속과 믹스인 기능을 사용하여 코드를 재사용할 수 있고 뷰를 체계적으로 구성함.
### 클래스형 뷰 사용
```python
# urls.py
urlpatterns = [
    url(r'^about/',MyView.as_view())
]
```
장고의 url 해석기는 요청과 관련된 파라미터들을 클래스가 아닌 함수에 전달하기 때문에 클래스 형 뷰는 클래스로 진입하기 위한 as_view() 클래스 메소들르 제공함.(진입 메소드)
- as.view()
    - 클래스의 인스턴스를 생성하고 그 인스턴스의 dispatch() 메소드 호출
    - dispatch() 메소드는 요청을 검사하여 GET,POST 등 어떤 HTTP 메소드로 호출되었는지 알아내 인스턴스 내에서 해당 이름을 갖는 메소드로 요청을 중계함.

```python
from django.http import HttpResponse
from django.views.generic import View

class MyView(View): # View 클래스 상속
    def get(self,requset): # get 메소드로 요청 시 result 반환
        return HttpResponse('result')
```
- View 클래스에는 as_view(),dispatch() 메소드가 정의되어있음.

### 클래스형 뷰 장점
- 효율적인 메소드 구분
    - HTTP 메소드에 따른 처리 기능을 코딩할때 if 함수를 사용하지 않고 메소드 명으로 구분하므로 코드 구조가 깔끔해짐.
```python
# 함수형 뷰
def index(request):
    if request.method=='POST':
        ...
# 클래스형 뷰
class index(View):
    def get(self,request): # dispatch() 메소드가 어떤 메소드로 요청되었는지 확인한 다음 해당 이름의 메소드로 요청.
        ...
```
- 상속 기능 가능
    - 다중 상속과 같은 객체지향기술이 가능함. 
    - 코드의 재 사용성이나 개발 생산성을 획기적으로 높여줌.
- 제네릭 뷰
    - 뷰 개발 과정에서 공통적으로 사용할 수 있는 기능들을 추상화 하고 이를 장고에서 미리 만들어 기본적으로 제공해주는 클래스형 뷰.
    - 요청 request 객체를 분석하고 템플릿 시스템에 넘겨줄 컨텍스트 변수를 구성하는 것은 제네릭 뷰에서 처리함.
```python
from django.views.generic import TemplateView

class AboutView(TemplateView): # 장고가 제공해주는 TemplateView 제네릭뷰를 상속받아 사용
    template_name = 'about.html'
```
#### 제네릭 뷰 분류
- Base View : 뷰 클래스를 생성, 부모 클래스를 제공하는 기본 제네릭 뷰
- Generic Display View : 객체의 리스트, 특정 객체의 상세정보를 보여줌.
- Generic Edit View : 폼을 통해 객체를 생성,수정,삭제 기능을 제공.
- Generic Date View : 날짜 기반 객체의 년/월/일 페이지로 구분하여 보여줌.

### 클래스형 뷰에서의 폼 처리
폼 처리과정
- 최초의 GET 폼은 비어있거나 미리 채워진 데이터를 가짐.
- 유효한 데이터를 가진 POST 데이터를 처리, 리다이렉트 처리됨.
- 유효하지 않은 데이터는 POST 에러메시지와 폼이 다시 출력됨.

## 로그
로그란?
- log, 애플리케이션의 상태를 관찰할 수 있도록 애플리케이션이 제공하는 정보
- 현재 우리의 프로그램이 어떤 상태를 가지고 있는지 외부 출력을 하게 만들어 개발자들이 눈으로 직접 확인하는 것

장고의 로깅은 기본적으로 파이썬의 로깅 체계를 그대로 따르면서 일부만 추가되었음.

장고의 runserver나 웹 서버에 의해 장고가 실행될 때 장고는 settings.py 파일에 정의된 LOGGING_CONFIG,LOGGING 항목을 참고하여 로깅에 관련된 설정을 처리하고 
settings.py 파일에 관련 항목이 없더라도 디폴트 로깅 설정으로 처리됨.
장고의 로깅은 실행되는 시점부터 준비되어있어 항상 로그를 기록하는 것이 가능.
### 파이썬의 로깅 모듈
로거,핸들러,필터,포맷터 4가지의 주요 컴포넌트로 구성됨.
#### 로거
- 로깅 시스템의 시작점, 로그 메시지를 처리하기위해 메시지를 담아두는 저장소
- 모든 로거는 이름을 가지고 있으며 로그 메시지의 중요도에 따라 자신이 어느 레벨 이상의 메시지를 처리할지에 대한 로그레벨을 가짐.

로그레벨
- DEBUG : 디버그 용도로 사용되는 정보, 로그레벨 최하위
- INFO : 일반적이고 보편적인 정보
- WARNING : 문제점 중에서 덜 중요한 문제점이 발생 시 이에 대한 정보
- ERROR : 문제점 중에서 주요 문제점이 발생 시 이에 대한 정보
- CRITICAL : 치명적인 문제점이 발생시 이에 대한 정보, 로그레벨 최상위

로그레코드
- 로거에 저장되는 메시지 로그
- 레코드도 로그레벨을 가짐
- 스택 트레이스 정보, 에러코드 등의 로그 이벤트에 대한 유용한 메타장보를 가지고 있음.

메시지가 로거에 도착할 때 로그레코드의 로그레벨,로거의 로그레벨 비교 방법
- 로그 레코드 로그레벨 >= 로거 로그레벨 => 메시지 처리 계속 진행
- 로그 레코드 로그 레벨 < 로거 로그레벨 => 메시지 무시
메시지 처리를 진행하는 것으로 결정되면 로거는 메시지를 핸들러에게 넘겨줌.

#### 핸들러
- 로거에 있는 메시지에 무슨 작업을 할지 결정하는 엔진
- 메시지를 화면이나 파일,네트워크 소켓 등 어디에 기록할 것인지와 같은 로그 동작을 정의.
- 핸들러도 로그레벨을 가지고 있음(로그 레코드의 로그레벨이 핸들러의 로그레벨보다 더 낮으면 핸들러는 메시지를 무시)
- 로거는 핸들러를 여러개 가질 수 있고 각 핸들러는 서로 다른 로그레벨을 가질 수 있음 => 메시지의 중요도에 따라 다른 방식의 로그처리가 가능
    - ex) ERROR,CRITICAL 메시지는 표준출력으로 보내는 핸들러를 만들고 차 후 분석을 위해 모든 메시지를 파일에 기록하는 다른 핸들러를 만들 수 있음.

#### 필터
- 로그 레코드가 로거에서 핸들러로 넘겨질 때 필터를 사용하여 로그 레코드에 추가적인 제어를 할 수 있음.

기본 처리방식
- 로그 레벨을 지정하여 그 로그 레벨에 해당하면 관련 로그 메시지를 출력함.
- 필터를 적용하면 로그 처리 기준을 추가할 수 있음.
- 필터를 추가하여 ERROR 메시지 중에서 특정 소스로부터 오는 메시지만 핸들러로 넘길 수 있음.
- 필터를 사용하면 로그 레코드를 보내기 전 수정하는 것이 가능
    - ex) 어떤 조건에 맞으면 ERROR 로그 레코드를 WARNING 로그레벨로 낮춰주는 필터를 만들 수 있음.
- 필터는 로거,핸들러 어디에나 적용 가능하며 여러개의 필터를 체인 방식으로 동작 시킬 수 있음.

#### 포맷터
- 로그 레코드가 최종적으로 텍스트로 표현되는데 포맷터는 텍스트로 표현 시 정확한 포맷을 지정해줌.

### logging 사용
```python
import logging

logger = logging.getLogger(__name__) # 로거 객체를 얻음
logger.error("Someting wrong!") # error 로그 메시지
```
- __name__ : 로거를 담고있는 파이썬 모듈의 이름
- 로거를 계층화할 때 이름으로 각 로거를 구분함.
- 로깅 호출은 부모 로거에게 전파됨.
- 로거 객체는 각 로그 레벨 별로 로깅 호출 메소드를 가지며 메소드는 각 레벨의 로그 메시지를 생성함.
    - logger.debug()
    - logger.info()
    - logger.warning()
    - logger.error()
    - logger.critical()
- 이외 로깅 메소드
    - logger.log() : 원하는 로그 레벨을 정하여 로그 메시지 생성
    - logger.exception() : 익셉션 스택 정보를 포함하는 ERROR 레벨의 로그 메시지를 생성

### 로깅 설정
- 로깅 메소드를 호출할 때 로그 메시지를 원하는대로 기록하기 위해선 로거,핸들러,필터,포맷 등을 설정해야함.
- 장고는 사전형 설정 방식을 디폴트로 사용.(settings.py 파일에 LOGGING 항목을 지정하지 않으면 장고는 default 로그 설정을 사용함)
```python
# settings.py
LOGGING = {
    'version':1,
    'disable_existing_loggers':True, # 기존의 로깅 설정 비활성화
    'formatters':{ # 포맷터 정의
        'simple':{ # 로그 레벨 이름,로그 메시지만 출력
            'format':'%(levelname)s %(message)s'
        },
        'verbose':{ # 로그 레벨 이름,로그 메시지를 만들어준 시간, 모듈명, 프로세스ID,스레드ID,로그 메시지 출력
            'format':'%(levelname)s %(asctime)s %(module)s %(process)d %(thread)d %(message)s'
        },
    },
    'filters':{ # 필터 정의
        'special':{ # 별명: special , 
            '()':'project.logging.SpecialFilter',# project.logging.SpecialFilter 라는 필터 정의
            'foo':'bar',# 필터가 생성될 때 foo=bar 인자를 넘김
        }
    },
    'handlers':{ # 핸들러 정의
        'null':{ # null 핸들러 모든 DEBUG 및 그 이상의 메시지를 /dev/null 장치로 보내주는 NullHandler 클래스 사용
            'level':'DEBUG',
            'class':'logging.NullHandler',
        },
        'console':{ # console 핸들러 모든 DEBUG 및 그 이상의 메시지를 표준 에러로 출력해주는 StreamHandler 클래스  사용, simple 포맷터 사용
            'level':'DEBUG',
            'class':'logging.StreamHandler',
            'formatter':'simple'
        },
        'mail_admins':{ # mail_admins 핸들러 모든 ERROR 및 그 이상의 메시지를 사이트 관리자에게 이메일로 보내주는 AdminEmailHandler 사용 special 필터 사용 
            'level':'ERROR',
            'class':'django.utils.log.AdminEmailHandler',
            'filters':['special']
        }
    },
    'loggers':{ # 로거 정의
        'django':{  # django 로거는 INFO 및 그 이상의 메시지를 null 핸들러에게 보내줌
            'hanlders':['null'],
            'propagate':True,
            'level':'INFO', 
        },
        'django.request':{ # django.request 로거 모든 ERROR,CRITICAL 메시지를 mail_admins 핸들러에게 보내줌 
            'handlers':['mail_admins'],
            'level':'ERROR',
            'propagate':False, # 메시지를 전파하지 않도록 설정, django.request 로거에 쓰여진 로그메시지는 django 로거에서 처리되지 않음 
        },
        'myproject.custom':{ # myproject.custom 로거 INFO 및 그이상의 레벨이면서 special 필터를 통과한 모든 메시지를 console과 mail_admins 핸들러에게 보내줌 
            # INFO 그 이상의 모든 메시지들이 콘솔에 출력되고 ERROR 및 CRITICAL 메시지들은 콘솔/이메일로 송신
            'handlers':['console','mail_admins'],
            'level':'INFO',
            'filters':['special']
        }
    }
}

```