# 아파치 웹 서버 연동하기
## 배포 
- 장고를 사용하여 웹 애플리케이션을 개발 한 후 서비스 하기 위해서는 운영환경에 개발한 프로그램을 배포하고 실행해야함.
- 개발환경에서 운영환경으로 옮겨가기 위해서는 개발 시 지정했던 설정사항을 몇가지 변경해야하고 운영환경의 웹 서버에서도 개발한 애플리케이션을 인식할 수 있도록 설정사항 변경이 필요함.
## mod_wsgi 확장 모듈
### Apache
- 아파치 웹 서버의 프로그램 명 : httpd
- 추가로 필요한 기능을 모듈로 만들어 동적 로딩 방식으로 기능을 확장할 수 있다.
### mod_wsgi
- mod_wsgi는 파이썬 웹 애플리케이션을 실행 할 수 있는 확장 모듈 중 하나이다.
- 파이썬의 웹 애플리케이션 표준 규격인 WSGI를 구현한 확장 모듈이며 파이선 웹 애플리케이션을 아파치에 실행하는데 사용함.
- C언어로 구현되어있어 내부적으로 아파치가 직접 파이썬 API와 동작하므로 상대적으로 적은 메모리를 사용함. CGI에 비해 좋은 성능을 가짐.
### mod_wsgi를 사용해 웹 애플리케이션(WSGI 애플리케이션)을 실행하는 실행모드
#### 내장모드
- 내장모드로 WSGI 애플리케이션을 실행하는 방식
- 아파치 자식 프로세스 컨텍스트 내에서 애플리케이션이 실행됨.
- 같은 아파치 웹 서버에서 실행되는 다른 웹 애플리케이션과 같은 아파치 자식 프로세스를 공유하게 됨.
- 단점으로 WSGI 애플리케이션의 소스가 변경되어 다시 적용하려면 아파치 전체를 재 기동을 해야하고 재 기동으로 인해 다른 서비스에도 영향을 받게되며 경우에 따라서 아파치 재 기동 권한이 없을 수 도 있음.
#### 데몬모드
- 유닉스 기반의 아파치 2.0 이상에서 지원
- WSGI 애플리케이션의 전용 프로세스에서 애플리케이션이 실행됨.
- WSGI 애플리케이션을 구현할때 별도의 프로세스 관리자나 WSGI 어댑터가 필요하지 않고 모든 처리는 mod_wsgi가 관리.
- WSGI 애플리케시연이 데몬모드로 동작할 시 다른 아파치 자식 프로세스와 별도의 프로세스에서 동작하기 때문에 정적인 파일을 서비스하는 프로세스와 PHP,Perl 등 아파치 모듈로 서비스하는 다른 애플리케이션에 미치는 영향이 미미함.
- 필요하다면 WSGI 애플리케이션 간에도 서로 영향을 주지 않도록 실행 유저를 달리하여 데몬 프로세스를 기동하는 것이 가능하다.

장고는 안정성을 고려해 내장모드보단 데몬모드로 실행할 것을 권장함.

## 장고 웹 서버 연동 원리
### wsgi.py
- wsgi.py 파일은 startpoject 명령으로 만들어짐.
- 장고와 웹 서버를 연결하는데 필요한 파일
- WSGI 규격에 따라 호출 가능한(callable) 애플리케이션 객체를 정의함.
    - 이 객체 명은 반드시 application 이여야한다. (장고가 자동으로 만들어줌)
    - application 객체는 아파치 같은 상용 웹 서버뿐만이 아니라 장고 개발용 웹 서버 runserver에서도 같이 사용하는 객체임.
    - 아파치는 http.conf 설정파일에서 WSGIScriptAlias 지시자를 통해 지정하고 개발용 runserver는 setting 모듈에서 WSGI_APPLICATION 변수로 지정한다.
#### 설정 정보를 담고있는 setting 모듈의 위치를 지정하는 방법
- 웹 서버는 application 객체를 호출하여 장고의 애플리케이션을 실행함.
    - application 객체를 호출하기 전 현재의 프로젝트 및 프로젝트에 포함된 모든 애플리케이션들에 대한 설정 정보를 로딩하는 작업이 필요함.
- 아파치 등 웹 서버는 wsgi.py 파일에서 지정
```python
# mysite/wsgi.py
import os
os.environ['DJANGO_SETTING_MODULE'] = 'mysite.settings'
```    
- 개발용 웹 서버는 실행옵션으로 지정, default는 프로젝트명.settings 사용
```
python manage.py runserver --settings=mysite.settings
```
## 상용 서버 적용 전 장고 설정 변경하기
상용 서버에 적용하기 위해서는 보안,성능 등을 고려하여 상용 환경에 맞는 설정으로 변경해야할 사항들이 있다.
### DEBUG = False
- 개발 모드에서 에러 발생시 디버깅을 위해 브라우저에 여러가지 정보를 출력하여 보여주었는데, 이런 디버그 정보는 장고 프로젝트에 관한 중요 정보이므로 debug 설정 값을 False로 셋팅하여 디버그 정보가 노출되지 않도록 해야함.
- 이 설정이 되어있으면 반드시 ALLOWED_HOSTS 항목을 설정해야함.
해커가 HTTP Host 헤더를 변조하여 CSRF 공격을 할 수 있기 때문에 이를 방지
```python
DEBUG = False
ALLOWED_HOSTS = ['localhost'] # (실제 배포일 시 ip주소)
```
### 정적파일(static)설정
- 개발 서버는 정적파일(이미지,자바스크립트,css 등)들을 알아서 찾아주었지만 상용 모드는 웹 서버가 정적파일이 어디있는지 알 수 있도록 해주어야함.
- settings 모듈의 STATIC_ROOT : 장고의 collectstatic 명령 실행 시 정적파일을 한 곳에 모아주는 디렉토리 
```python
# settings.py
STATIC_ROOT = os.path.join(BASE_DIR,'www_static') # mysite/www_static
```
```
$ python manage.py collectstatic
```
- 주의 할 점으로 settings 모듈의 STATICFILES_DIRS 항목에 STATIC_ROOT 항목에서 정의된 디렉토리가 포함되선 안된다. 
    - Why? STATICFILES_DIRS 항목에 정의된 디렉토리에서 정적파일을 찾아 STATIC_ROOT 디렉토리에 복사해주기 때문.
### database,log
- 개발 모드에서는 runserver를 실행시킨 사용자의 권한으로 데이터베이스 파일, 로그 파일을 엑세스하며, 상용 모드에서는 웹 서버 프로세스의 권한자인 apache 사용자 권한으로 해당 파일들을 엑세스해야함.
#### database
- 해당 디렉토리 및 파일의 엑세스 권한을 변경해주어야함. SQLite3 데이터베이스가 있는 파일에 apache 사용자가 접근/읽기/쓰기가 가능하도록 설정
```python
# settings.py
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db/db.sqlite3'), # 엑세스 권한 변경
    }
}
```
#### log
- settings 모듈의 LOGGING 항목에 로깅 관련 사항이 정의되어있으며 여기에 로그 파일의 위치가 설정되어있음. 로그 파일에 apache 사용자가 읽기/쓰기가 가능하도록 설정
```python
LOGGING = {
    ...
    'handlers':{ 
        'file':{
            'level':'DEBUG', 
            'class':'logging.FileHandler',
            'filename':os.path.join(BASE_DIR,'logs/logfile'), # 로그가 기록되는 파일의 위치 mysite/log/logfile 
            'formatter':'verbose'
        },
    },
}
```
## 내장 모드로 실행
- mod_wsgi 모듈이 아파치 프로세스에 내장되어 실행되는 방식
[아파치 설치](https://www.apachelounge.com/download/ http://kiwinote.tistory.com/75)
- 아파치와 장고를 연동하기 위해선 python과 apache의 비트가 같아야함.mod_wsgi의 모듈 비트 또한 ㅎㅎ(나는 이것 때문에 못했다..) window가 64비튼데 mod_wsgi는 32비트까지만 지원한다니..
[Windows에서 Apache2.4 + mod_wsgi 설치](http://dgkim5360.tistory.com/entry/install-apache24-mod-wsgi-for-windows)
### 아파치 설정
- 아파치 웹 서버에서 mod_wsgi 모듈을 이용해 파이썬 웹 애플리케이션을 실행할 수 있도록 아파치 설정 파일인 httpd.conf에 mod_wsgi 설정을 추가해줌.
- 아파치에 대한 설정 및 웹 서버 실행은 루트권한으로 작업
```
# Apache24/conf/httpd.conf
...
# LoadModule wsgi_module modules/mod_wsgi.so

WSGIScriptAlias / C:/Django/mysite/wsgi.py
WSGIPythonHome C:/Users/leeyejin/study/env/mysite # virtualenv 설정 시
WSGIPythonPath C:/Django/mysite

<Directory C:/Django/mysite/mysite>
<Files wsgi.py>
Require all granted
</Files>
</Directory>

Alias /static/ C:/Django/mysite/www_static/
<Directory C:/Django/mysite/www_static/>
Require all granted
</Directory>
```
### 실행
- 아파치를 실행한당.
```
$ httpd -k start
```

## 데몬 모드로 실행
-  WSGI 애플리케이션의 전용 프로세스에서 애플리케이션이 실행되는 방식
### 아파치 설정
```
# Apache24/conf/httpd.conf
...
WSGIScriptAlias / C:/Django/mysite/wsgi.py
WSGIPythonHome C:/Users/leeyejin/study/env/mysite 
WSGIDaemonProcess mysite python-path=C:/Django/mysite/
WSGIProcessGroup mysite

<Directory C:/Django/mysite/mysite>
<Files wsgi.py>
Require all granted
</Files>
</Directory>

Alias /static/ C:/Django/mysite/www_static/
<Directory C:/Django/mysite/www_static/>
Require all granted
</Directory>
```
### 실행
내장 모드와 동일

## 느낀점
와우 연동 너무 쟈증난당ㅎㅎ 책에서는 리눅스 기반의 설명이라 따라하다가 너무 막혀서 따로 구글링하여 모듈을 추가해야되는 것을 깨닫고 모듈 추가하니까 로드서부터 에러나는 거 보고 너무 지쳤다... 계속 알아본 결과 python,apache,mod_wsgi의 비트가 모두 같아야한다는 것을 보고 apache가 64비트이고 내 윈도우도 64비트니까^^* 파이썬만 32비트인 것을 확인하고 부랴부랴 파이썬을 재설치 했지만 너무 당연하게도 안됨!!! 다시 확인해 보니 모듈이 32비트ㅠㅠ mod_wsgi은 64비트 지원 안된대영 .. (나는 python2.7 환경에서 작업하는데 mod_wsgi는 2.7환경과 32비트를 동시에 지원하는게 없었다.[Windows에서 mod_wsgi 실행하기](https://github.com/GrahamDumpleton/mod_wsgi/blob/develop/win32/README.rst) )그래서 다시 32비트로 다시 맞추니 apache가 아예 실행이 안됨..OTL  나중에 천천히 다시 도전해볼게..

