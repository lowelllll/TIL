

# Media Files

> 유저가 업로드한 모든 정적인 파일 ( image,etc)

## Media file 사용을 위한 설정

`project/settings.py` 설정

#### MEDIA_URL

> 현재 요청이 media 파일 요청인지 구분하는 url

STATIC_URL과 유사.

#### MEDIA_ROOT

> 업로드 된 파일을 저장할 디렉토리 경로

STATICFILES_DIR과 유사.



뷰는 `HttpRequest.FILES`를 통해 파일을 전달 받고, 적절히  검증을 거친 다음 `MEDIA_ROOT` 하단에 저장함.

## Media

#### FileField / ImageField

파일 저장을 지원하는 모델 필드.

파일은 `settings.MEDIA_ROOT` 경로의 <b>File System</b>에 저장하며

DB 필드에는 `settings.MEDIA_ROOT` 내 저장된 하위 경로를 저장. (문자열)

#### 파일 업로드시 Form enctype

파일 업로드시 form enctype은 `multipart/form-data`이여야 한다.

(`application/x-www-form/urlencoded`로 지정된 경우 파일 명만 전송함)

#### 뷰 작성

File을 뷰에서 받아 DB에 저장하기 위해선 Form을 받을 때 `request.POST` 메소드 뿐만이 아닌 `request.FILES` 메소드도 받아야 한다.

#### 템플릿 내에 서 Media URL 처리

```django
{% if post.photo %} 
	<img src="{{ post.photo.url }}"> {# photo가 없을 경우 에러가 나기 때문에 if문으로 제어.#}
{% endif %}
```



### 개발 환경에서의 media 서빙

static file과 다르게 media file은 개발 서버에서 서빙을 미지원한다.

개발 편의성 목적으로 직접 서빙 Rule을 추가할 수 있다.

`settings.DEBUG = False`일 경우 static 함수에선 빈 리스트를 리턴.

```django
# project/urls.py
..
from django.conf import settings
from django.conf.urls.static import static
urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

### upload_to

> 파일 저장 경로 커스텀

한 디렉토리에 파일들을 너무 많이 몰아둘 경우 os 파일찾기 성능이 저하됨.

디렉토리 depth가 깊어지는 것은 성능에 큰 영향이 없기 대문에 업로드 시간대 별로 다른 디렉토리에 저장하는 방법은 매우 효율적.

```django
# app/models.py
...
	photo = models.ImageField(upload_to='photo/%Y/%m/%d') # 연/월/일 별로 저장
	# ex) photo/2018/9/8/image.jpg
```





 