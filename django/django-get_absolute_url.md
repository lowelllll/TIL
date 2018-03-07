# get_absolute_url
django에서 model 클래스를 지정할 때, 그 모델에 대한 DetailView를 작성할 경우 get_absolute_url 변수를 설정해주면 코드가 간결해짐.
```python
# models.py
class Post(models.Model):
    title = models.CharField(max_length=30)

    def get_absolute_url(self):
        return reverse('post:post_detail',args=[self.id])
```
## 활용
### url template tag
```html
<a href='{{ post.get_absolut_url }}'>{{ post.title }}</a>
<!--{% url 'post:post_detail' post.id %}-->
```
### resolve_url, redirect를 통한 활용
resolve_url(모델 인스턴스),redirect(모델 인스턴스)를 통해 모델 인스턴스의 get_absolute_url() 함수를 자동으로 호출. 
```python
from django.shortcuts import resolve_url
from django.shortcuts import redirect

resolve_url('post:post_detail',post.id)
resolve_url(post)
# resolve_url : 가장 먼저 get_absolute_url 함수의 존재 여부를 체크하고 존재하면 호출하며 그 리턴 값으로 URL 사용
# 인자의 인스턴스 메소드로 get_absolute_url이 있는지 체크하여 리턴

print(redirect('post:post_detail',post.id))
print(redirect(post))
# <HttpResponseRedirect status_code=302,'text/html; charset=utf-8',url='/post/105/'>
```

### CBV 사용
CreateView,UpdateView에 success_url을 제공하지 않는 경우 해당 model instance의 get_absolute_url 주소로 이동이 가능한지 체크함.  
(생성,수정 뒤 Detail 화면으로 이동하는 것은 일반적임)

## refer
https://wayhome25.github.io/django/2017/05/05/django-url-reverse/