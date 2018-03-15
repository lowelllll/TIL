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
django에서 쉽게 Authentication을 얻게 해주는 패키지 social-auth-app-django를 설치함.  
+추가로 python-social-auth도 같이 설치함.
```python
pip install social-auth-app-django
```
### 1.2 Setting settings.py 
settings.py에서 아래와 같은 설정.
1. INSTALLED_APPS
    - 새롭게 설치한 social-auth-app-django app을 설정.
        - social-auth-app-django를 추가한 후에는 migrate를 해준다. (auth-user 테이블 생성)
        - 새로운 User social auths 테이블이 생성되어 가입한 사용자를 관리함.
2. TEMPLATE_CONTECT_PROCESSORS에 항목 추가
    - social.apps.django_app.context_processors.backends
    - social.apps.django_app.context_processors.login_redirect
3. AUTHENTICATION_BACKENDS 새롭게 추가 
    - 인증 체계에 사용될 backend를 등록하는 항목
    - 기본으로 django.contrib.auth.backends.ModelBackend
    - python-social-auth의 앱에 따른 백엔드를 추가
        - facebook - social_core.backends.facebook.FacebookOAuth2
        - google - social_core.backends.google.GoogleOAuth2,social_core.backends.open_id.OpenIdAuth,social_core.backends.google.GoogleOpenId,
        - twitter - social_core.backends.twitter.TwitterOAuth
        - github - social_core.backends.github.GithubOAuth2
4. OAuth 키,시크릿 키 설정
    - 앱에 따른 인증 키(ID),시크릿 키
5. LOGIN_REDIRECT_URL 설정
    - Django Login 및 Social Auth에서 인증 한 후 사용자를 리디렉션하는 데 사용됨.

#### Service Provider(open api를 제공하는 곳)에서 client id를 얻는 방법
- [google](http://judev.tistory.com/5)
    - 구글은 google+API 라이브러리 등록해야 정상적으로 작동함.
    - 리디렉션 url 에는 http://localhost:8000/complete/google-oauth2/ 등록해주어야 함! 
- [facebook](http://initialkommit.github.io/2015/04/27/django-newbie-adding-facebook-authentication-to-a-django-app/#24getclientidsforthesocialsites)

```python
INSTALLED_APPS = [
    ...
    'social_django',
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
    'social_core.backends.open_id.OpenIdAuth',  # for Google authentication
    'social_core.backends.google.GoogleOpenId',  # for Google authentication
    'social_core.backends.google.GoogleOAuth2',  # for Google authentication
    #'social_core.backends.github.GithubOAuth2',  # for Github authentication
    'django.contrib.auth.backends.ModelBackend',
)


# SOCIAL_AUTH_FACEBOOK_KEY 
# SOCIAL_AUTH_FACEBOOK_SECRET
SOCIAL_AUTH_GOOGLE_OAUTH2_KEY = 'client id'
SOCIAL_AUTH_GOOGLE_OAUTH2_SECRET = 'secret key'
```
### 1.3 urls.py
프로젝트 urls.py 설정
```python
url(r'', include('social_django.urls', namespace='social')),  
```
템플릿에서의 사용
```python
<a href="{% url 'social:begin' 'google-oauth2' %}">Login with Google</a>
# facebook url 'social:begin' 'facebook' 
# twitter url 'social:begin' 'twitter' 
```
### 오류 해결
#### No module social 
```
No module named 'social'
```
- python-social-auth를 설치한다 
```
pip install python-social-auth
```

## refer
[How to add Google and Github OAuth in Django](https://fosstack.com/how-to-add-google-authentication-in-django/ )

[How to add SNS authentication to a Django app.](http://initialkommit.github.io/2015/04/27/django-newbie-adding-facebook-authentication-to-a-django-app/#24getclientidsforthesocialsites)

[홈페이지에 페이스북 로그인 연동하기](https://dreamyoungs.github.io/tip/facebook-login-connect)

https://stackoverflow.com/questions/42545981/no-such-table-social-auth-usersocialauth

http://d2.naver.com/helloworld/24942

https://beomi.github.io/2017/02/08/Setup-SocialAuth-for-Django/

