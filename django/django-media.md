
# django-media(사진 업로드)
django에서 사진 업로드 기능을 구현하기 위해서는 기능을 위한 패키지와 media에 대해 알아야함. 
### 1.1 media file
> 유저가 업로드한 모든 정적인 파일(image,pdf etc)이며 프로젝트 단위로 저장한다.
#### media file의 전달/저장과정 
- HttpResponse.FILES 를 통해 파일 전달
- settings.MEDIA_ROOT 디렉토리 하단에 파일 저장.
### 1.2 Pillow 패키지
파이썬이 제공하는 사진 업로드 패키지.
## 사진 업로드
### 2.1 Pillow 설치
```python
pip install Pillow
```
### 2.2 settings.py 설정
- MEDIA_URL
- MEDIA_ROOT 
    - 제출 된 파일,이미지는 이 경로에 저장이 됨.
```python
# settings.py
...
MEDIA_URL = '/media/' # 항상 /로 끝나야함.

MEDIA_ROOT = os.path.join(BASE_DIR,'media') # media 파일이 저장될 경로
```
### 2.3 models.py 설정
- 이미지 필드,파일 필드 지정
    - models.FileField
    > 파일 저장을 지원하는 모델 필드
    - models.ImageField
    > 이미지 저장을 지원하는 모델 필드(FileField 상속)
```python
# models.py
class POST(models.Model):
    photo = models.ImageField(blank=null)
```
#### 파일 저장 경로
- 제출된 파일,이미지는 settings.MEDIA_ROOT 경로에 저장됨.
- DB 필드에는 settings.MEDIA_ROOT 내 저장된 하위 경로를 string으로 저장됨.

#### 파일 저장 경로 수정
- upload_to 속성으로 파일 저장 경로 수정
```python
    photo = models.ImageField(upload_to='profile')
    # 저장 경로 MEDIA_ROOT/profile/xxx.jpg 경로에 이미지 저장
    # DB 필드 'MEDIA_ROOT/profile/xxx.jpg' string 저장 
```
한 디렉토리에 파일이 몰릴 경우, OS 파일 찾기 기능이 저하됨.  
디렉토리 depth가 깊어지는 것은 성능에 영향이 없음.  
해결:업로드 시간 별로 다른 디렉토리에 저장하는 것이 성능에 좋음
```python
    photo = models.ImageField(upload_to='profile/%Y/%m/%d')
    # 저장 경로 MEDIA_ROOT/profile/2018/03/02 경로에 이미지 저장
    # DB 필드 'MEDIA_ROOT/profile/2018/03/02' string 저장
```

### 2.4 urls.py 설정
- 개발 환경에서 media 파일 서빙은 static 파일과 다르게 개발서버에서 기본 서빙을 지원하지 않음.
- 개발 편의성 목적으로 서빙 rule 추가가 가능함.
- settings.DEBIG = False일 때, static 함수에서 빈 리스트를 리턴함
```python
# project/urls.py 
from django.conf import settings
from django.conf.urls.static import static

urlpattern += static(settings.MEDIA_URL,document_root=settings.MEDIA_ROOT)
```
- 기존 url 패턴에 static() 함수가 반환하는 URL 패턴을 추가한 것
    > static(prefix,view=django.views.static.serve,**kwargs)
- settings.MEDIA_URL로 지정된 /media/ url 요청이 오면 django.views.static.serve() 뷰 함수가 처리하고, 이 함수에 따라 document_root = settings.MEDIA_ROOT 키워드 인자가 전달됨.

### 2.5 template 설정
#### 파일 업로드시 form enctype
파일 업로드시 항상 method는 post, enctype="multipart/form-data" 속성을 지정해주어야 한다.
```html
<form method="post" action="" enctype="multipart/form-data">
```
#### media url 처리
```html
{% if post.photo %}
path : {{ post.photo.path }} <!--/Users/nickname/documents/practice/media/jeju_pic.png-->
url : {{ post.photo.url }} <!--/media/jeju_pic.png-->

<!-- url 속성을 사용하여 이미지를 출력한다.-->
<img src="{{ post.photo.url }}" alt='photo'>
{% endif %}
```


 