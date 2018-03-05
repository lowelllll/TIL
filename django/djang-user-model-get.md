# User 모델 클래스 획득 방법
django.contrib.auth 앱의 User 모델을 얻는 방법은 3가지가 있다.
1. 직접 User 모델 Import(비추)
```python
from django.contrib.auth.models import User
User.objects.all()
```
2. get_user_model helper 함수로 모델 참고(추천)
```python
from django.contrib.auth import get_user_model
User = get_user_model()
User.objects.all()
```
3. settings.AUTH_USER_MODEL을 통한 모델 클래스 참조(추천)
- settings.py 설정
```python
# settings.py
...
AUTH_USER_MODEL = 'auth.User'
```
- models.py 설정
```python
# models.py
...
from django.conf import settings
class Post(models.Model):
    author = models.ForeignKey(settings.AUTH_USER_MODEL)
```

# refer
[사용자 인증 (django.contrib.auth)](https://wayhome25.github.io/django/2017/05/18/django-auth/#user-%EB%AA%A8%EB%8D%B8-%ED%81%B4%EB%9E%98%EC%8A%A4-%ED%9A%8D%EB%93%9D-%EB%B0%A9%EB%B2%95-%EC%A4%91%EC%9A%94)