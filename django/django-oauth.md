# Django OAuth
## OAuth
- 인증을 위한 오픈 스탠더드 프로토콜
- facebook , 트위터 같은 인터넷 서비스의 기능을 다른 애플리케이션에서도 사용할 수 있게 한 것
### OAuth와 로그인
사원증을 이용하여 출입할 수 있는 회사에서 회사 사원이 사원증으로 건물에 출입하는 것은 로그인  
방문증을 수령한 후 회사에 출입하는 것은 OAuth
- 로그인한 사용자와 OAuth를 이용해 권한을 인증 받은 사용자는 할 수 있는 일이 다름.
### OpenID,OAuth
#### OpenID
- 인증을 위한 프로토콜
OAuth와 OpenID의 목적은 다름.
- OpenID의 주요 목적 : 인증(OpenID를 사용한다는 것은 본질적으로 로그인 하는 행동과 같음)
- OAuth의 주요 목적 : 허가 
## Django에서 OAuth 사용하기
다른 앱에서 Authentication을 획득하여 Django app에 적용하는 법
### 1.1 install python-social-auth
django에서 쉽게 Authentication을 얻게 해주는 패키지 python-social-auth를 설치함.
```python
pip intall python-social-auth
```
만약 social_django가 없다는 오류가 난다면 social-auth-app-django를 설치해줌.
```python
pip install social-auth-app-django
```
### 1.2 Setting settings.py 
settings.py에서 아래와 같은 설정.
1. INSTALLED_APPS
    - 새롭게 설치한 python-social-auth app을 설정.
    - 새로운 User social auths 테이블이 생성되어 가입한 사용자를 관리함.
2. TEMPLATE_CONTECT_PROCESSORS에 항목 추가
    - social.apps.django_app.context_processors.backends
    - social.apps.django_app.context_processors.login_redirect
3. AUTHENTICATION_BACKENDS 새롭게 추가 
    - 인증 체계에 사용될 backend를 등록하는 항목
    - 기본으로 django.contrib.auth.backends.ModelBackend
    - python-social-auth의 앱에 따른 백엔드를 추가
        - facebook - social.backends.facebook.FacebookOAuth2 
        - google - social.backends.google.GoogleOAuth2
        - twitter - social.backends.twitter.TwitterOAuth
4. OAuth 관련 변수 설정
    - SOCIAL_AUTH_LOGIN_REDIRECT_URL
        - 로그인 후 되돌아올 URL
    - SOCIAL_AUTH_URL_NAMESPACE
        - 인증 URL의 Namespace
    - SOCIAL_AUTH_APP_KET/SECRET
        - 앱에 따른 인증 키(ID),시크릿 키
    - SESSION_SERIALIZER
        - 세션 객체를 직렬화하는 처리기

```python
INSTALLED_APPS = [
    ...
    'social.apps.django_app.default',
]
TEMPLATES = [
    {
        ...
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
                'social.apps.django_app.context_processors.backends',
                'social.apps.django_app.context_processors.login_redirect',
            ],
        },
    },
]
# 인증 체계에 사용될 backend를 등록하는 항목
AUTHENTICATION_BACKENDS = (
    'social_core.backends.google.GoogleOAuth2',
    # social.backends.facebook.FacebookOAuth2 
    # social.backends.twitter.TwitterOAuth
    'django.contrib.auth.backends.ModelBackend',
)

SOCIAL_AUTH_LOGIN_REDIRECT_URL = '/blog' 

SOCIAL_AUTH_URL_NAMESPACE = 'social' # 인증 URL의 namespace
# SOCIAL_AUTH_FACEBOOK_KEY 
# SOCIAL_AUTH_FACEBOOK_SECRET
SOCIAL_AUTH_GOOGLE_OAUTH2_KEY = 'client id'
SOCIAL_AUTH_GOOGLE_OAUTH2_SECRET = 'secret key'

SESSION_SERIALIZER = 'django.contrib.sessions.serializers.PickleSerializer'
```
> Service Provider(open api를 제공하는 곳)에서 client id를 얻는 방법
- [google](http://judev.tistory.com/5)
    - 구글은 google+API 라이브러리 등록해야 정상적으로 작동함.
    - 리디렉션 url 에는 http://localhost:8000/complete/google-oauth2/ 등록해주어야 함! 
- [facebook](http://initialkommit.github.io/2015/04/27/django-newbie-adding-facebook-authentication-to-a-django-app/#24getclientidsforthesocialsites)

### 1.3 urls.py
프로젝트 urls.py 설정
```python
url(r'', include('social.apps.django_app.urls', namespace='social')),  
```
템플릿에서의 사용
```python
<a href="{% url 'social:begin' 'google-oauth2' %}?next={{ request.path }}">Login with Google</a>
# facebook url 'social:begin' 'facebook' 
# twitter url 'social:begin' 'twitter' 
```
## refer
[How to add SNS authentication to a Django app.](http://initialkommit.github.io/2015/04/27/django-newbie-adding-facebook-authentication-to-a-django-app/#24getclientidsforthesocialsites)

[홈페이지에 페이스북 로그인 연동하기](https://dreamyoungs.github.io/tip/facebook-login-connect)

https://stackoverflow.com/questions/42545981/no-such-table-social-auth-usersocialauth

http://d2.naver.com/helloworld/24942


