# django-rest-framework
> Django 안에서 RESTful API 서버를 쉽게 구축할 수 있도록 도와주는 오픈 소스 라이브러리.

rest한 api를 쉽게 만들 수 있다.

## 간단한 api 만들기

1. django-rest-framework 설치  
가상환경을 만들고 장고와 DRF를 설치해준다. `pip install djangorestframework`
2. app에 등록  
settings.py에 DRF를 등록.
```python
ALLOWED_HOSTS = ['*'] # 접근 가능한 호스트 지정

INSTALLED_APPS = [
    ...
    'rest_framework',
]
```
3. model 생성
```python
from django.db import models


class Movie(models.Model):
    title = models.CharField(max_length=30)
    genre = models.CharField(max_length=15)
    year = models.IntegerField()

    def __str__(self):
        return self.title

```
4. serializer 생성  
Serializer란 queryset과 모델 인스턴스와 같은 복잡한 데이터를 json,xml 또는 다른 콘텐츠 유형으로 쉽게 변환할 수 있음.  
받은 데이터의 유효성을 검사한 다음, 복잡한 타입으로 형 변환할 수 있도록 serializeation을 제공.
```python
# app/serializers.py

from rest_framework import serializers
from .models import Movie

class MovieSerializer(serializers.ModelSerializer):
    class Meta:
        model = Movie
        fields = ('id','title','genre','year')
```
5. views.py 작성  
<b>viewset</b> 자주 사용하는 공통적인 view 로직을 그룹화한 것.  
CRUD 로직을 직접 구현하지 않아도 됨.
```python
from rest_framework import viewsets
from .serializers import MovieSerializer
from .models import Movie

class MovieViewSet(viewsets.ModelViewSet):
    queryset = Movie.objects.all()
    serializer_class = MovieSerializer
```
6. urls.py 작성
<b>router</b> viewset을 router에 연결하면 자동으로 url을 맵핑해줌.
```python
from django.conf.urls import url,include
from django.contrib import admin
from rest_framework import routers
from movies.views import MovieViewSet

router = routers.DefaultRouter()
router.register('movies',MovieViewSet)

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^',include(router.urls)),
]
```
이렇게 router를 설정하면 아래와 같이 url 맵핑이 됨.
```
URL pattern: ^movies/$ Name: 'movie-list' 
URL pattern: ^movies/{pk}/$ Name: 'movie-detail'
```
7. 실행하기!

## refer
http://yunhookim.tistory.com/7
https://www.buzzvil.com/ko/2016/12/26/how-to-use-django-rest-framework-buzzvil/
