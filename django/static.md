# Static Files

> 개발 리소스로서의 정적인 파일 (js,css,image,etc)

- 앱 단위로 저장/서빙
- 프로젝트 단위로 저장/서빙



## Static file을 사용하기 위한 설정

`project/settings.py`에서의 설정.

### STATICFILES_DIRS

> 프로젝트 단위로 저장/서빙할 디렉토리를 설정해 주는 것.

프로젝트 단위에서 사용되는 static 파일을 관리하기 위해 설정함.

### STATIC_ROOT 

> 실 서비스 배포 전에 static 파일을 모으는 디렉토리를 설정.

개발 당시에는 의미가 없는 설정. 

실 서비스 배포 전에 `python manage.py collecstatic` 명령어로 프로젝트/앱 단위로 나누어져 있는 static 파일을 한 곳에 모아 배포 서버에 복사 후 서빙해야함.  

`STATIC_ROOT`로 정한 디렉토리는 static 파일을 모으는 디렉토리임.

### STATIC_URL

> 앱으로 들어오는 호출(url)에 대해서 뷰 함수 호출인지, static 파일 호출인지 할 수 있게 하는 것

blog/index - View 함수 호출

/static/blog/style.css - Static 파일 호출.



```python
# project/settings.py
..

STATIC_URL = '/static' # app/static 폴더의 static 파일 서빙

STATICFILES_DIR = (
	os.path.join(BASE_DIR,'projectname','static'), # projectname/static 폴더의 static 파일 서빙
)

STATIC_ROOT = os.path.join(BASE_DIR,'staticfiles') # python manage.py collectstatic 명령어 이후 												  # static 파일이 모아지는 디렉토리
```



### Static Files Finders

Template Loader와 유사.

#### Finder 종류

- AppDirectoriesFinder 

  앱 안의 static 파일을 찾음

- FileSystemFinder 

  프로젝트의 static 파일을 찾음

장고 서버 초기 시작시 finder를 통해 static 파일이 있을 후보 디렉토리 리스트를 작성함.



### 템플릿 내에서 static 파일 url 처리

- 수동으로 URL 설정

  ```
  <img src="/static/blog/logo.png">
  ```

  - 추천하지 않는 방법임.

- Template tag를 통해 prefix 붙이기

  ```django
  {% load static %} {# static tag load #}
  <img src="{% static 'blog/logo.png' %}">
  ```



## Static file 서빙

###  개발환경에서의 static 파일 서빙

- 개발 서버를 통해 `settings.DEBUG=True` 설정에 한해 서빙 지원.
- `project/urls.py`에 Rule이 명시되어있지 않아도 자동 Rule 추가.
- 장고를 통해 직접 static 파일 서빙하는 것은 개발 목적으로만 제공함. (배포시에는 직접 서빙)



### 실제 배포환경에서의 static 파일 서빙

```
python manage.py collecstaic
```

- 여러 디렉토리에 나눠 저장된 static 파일들의 위치는 현재 장고 프로젝트만이 알고있음.
- 외부 웹 서버는 전혀 알지 못함.
- 외부 웹 서버에서 finder의 도움 없이도 static 파일을 서빙하기 위해 finder를 활용하여 한 디렉토리로 (settings.STATIC_ROOT) 파일들을 복사. 복사된 파일을 서빙하면 finder가 필요없음.



#### 외부 웹서버에 의한 static/media 컨텐츠 서비스

- 정적인 컨텐츠는 외부 웹 서버에서 직접 응답하면 더 빠른 응답이 됨.
- WAS 보다 빠르고 효율적으로 동작함.
- 정적 컨텐츠와 동적 컨텐츠 분리를 통해, 그에 맞는 최적화 방법을 사용.
  - memcahe/redis 캐시 등



#### static 파일 모으는 순서, 적용 순서(배포)

1. `python manage.py collecstatic`
   - `settings.STATIC_ROOT`경로 아래로 static 파일들을 모두 복사
2. `settings.STATIC_ROOT` 경로에 모아진 파일을 배포 서버로 복사
3. `settings.STATIC_URL` 설정이 배포 서버를 가르키도록 수정.
4. 실 서비스 static  서빙 웹 서버 설정 (nginx,apache,S3,CDN 등)



## refer

askdjango

