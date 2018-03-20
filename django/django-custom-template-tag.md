# django 커스텀 템플릿 태그
django에서는 템플릿 태그를 직접 커스텀 할 수 있는 기능을 제공한다.
## 1.1 custom_tags.py 설정
app/templatetags 디렉토리 안에 custom_tags.py 파일을 생성한다.
```
app/
    templatetags/
        ...
        custom_tags.py
```
template 커스텀 태그는 앱의 위치와 상관없이 모듈 이름을 import 하기 때문에 모듈 이름이 겹쳐선 안됨.
### custom tag 종류
- filter  
앞의 값에 뒤의 값을 연산하는 태그. filter이기 때문에 여러개의 필터를 붙여 사용가능함.
```python
# custom_tags.py 
@register.filter
def add_str(left,right):
    return left+right

# template
{{ prefix | add_str:url }} # prefix+url text 반환
```
- simple_tag  
단순히 어떤 특정 값을 출력함.
```python
# custom_tags.py
@register.simple_tag
def today():
    return datetime.now().strftime("%Y-%m-%d")

# template
{{ today }}
```
- assignment_tag
템플릿에서 사용 가능한 변수에 결과를 저장하는 역할.  
with 태그와 유사하지만 {% endwith %} 처럼 끝을 맺는 태그가 없어도 됨.  
간편하게 변수 설정을 할 수있음.
```python
# custom_tags.py
@register.assignment_tag
def max_int(a,b):
    return max(int(a),int(b))

# template
{% max_int first_count second_count as max_count %}
```
- inclustion_tag  
데코레이션의 첫번째 파라미터는 html 템플릿을 호풀해 부모 템플릿에 출력함.  
이 때 호출되는 템플릿에 부모 템플릿의 각종 파라미터 전달 가능  
데코레이터의 takes_context=True 설정 => 부모 템플릿의 context의 값을 가져와  호출하는 템플릿으로 전달 가능함
```python
# custom_tags.py
@register.inclustion_tag(div.html,takes_context=True)
def include_div(context):
    return {
        'div_param':context['param']
    }

# template
parent.html 
{{ include_div }}

div.html
{{ div_param }}
```

### custom_tags.py 
```python
from django import template

register = template.Library()

@register.filter
def add_str(left,right):
return left+right
{{ prefix | add_str:url }}
```
## 2.1 template 파일 설정
custom_tags.py에서 설정한 태그를 사용하기 위해 모듈을 로드해주고 태그를 사용한다.
```html
{% load custom_tags.py %}
{{ prefix | add_str:url }}
```

## refer 
https://blueshw.github.io/2016/03/03/django-using-custom-templatetags/
https://docs.djangoproject.com/en/2.0/howto/custom-template-tags/