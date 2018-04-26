# PythonanyWhere
> Python을 올려 사용할 수 있는 PaaS 서비스 , 방문자가 아주 많지 않은 소규모 애플리케이션을 위한 무료 서비스를 제공함

Django를 배포할 수 있당.

#### PaaS? (서비스 형태의 플랫폼)
>서비스 형태의 플랫폼(Platform-as-a-service, PaaS)은 서비스 제공업체가 고객에게 플랫폼을 제공함으로써 고객이 일반적으로 소프트웨어 개발 프로세스에 필요한 인프라를 구축하고 유지할 필요 없이 비즈니스 애플리케이션을 개발, 실행, 관리할 수 있도록 하는 클라우드 컴퓨팅 유형이다.

## pythonanywhere 사용법
1. github에 코드 배포 
2. pythonanywhere 계정 생성    
    - www.pythonanywhere.com  
    무료 계정 'beginner'로 회원가입.
3. BASH git clone
    ```
    $ git clone <giturl>
    ```
4. BASH virtualenv 생성,패키지 다운
    ```
    $ virtualenv --python=python3.6 venv
    $ source venv/bin/activate # 가상환경 진입
    
    $ (venv) pip install -r requirements.txt
    ```
5. BASH 정적파일,db생성
    ```
    $ python manage.py collectstatic
    $ python manage.py migrate
    $ python manage.py createsuperuser
    ```
6. WEB app 생성
    - WEB 메뉴에서 add a new web 을 선택하고 <b>수동설정(manual configuration)</b> 선택하여 웹 앱 생성 
7. WEB APP 패널에서 virtualenv 설정
    - virtualenv 설정하는 form에 virtualenv의 path 설정
    ```
    /home/<your-id>/<virtualenv-path>
    # ex) /home/lowellll/prj/myenv
    ```
8. WEB APP에서 wsgi파일 설정 (Code에 있음)
    ```python
    import os
    import sys

    path = '/home/<your-id>/<your-project-path>'  # PythonAnywhere 계정으로 바꾸세요.
    # ex) /home/lowellll/prj/djangoprj
    if path not in sys.path:
        sys.path.append(path)

    os.environ['DJANGO_SETTINGS_MODULE'] = '<your-project>.settings'

    from django.core.wsgi import get_wsgi_application
    from django.contrib.staticfiles.handlers import StaticFilesHandler
    application = StaticFilesHandler(get_wsgi_application())
    ```
9. WEB APP에서 RELOAD 
    - 맨 위 초록색 버튼 누르고 링크 클릭! 완성 

## Debugging
- 사이트에 접속 시 오류가 날때는 <b>error log</b>를 확인한다.(Log files)  
    error log 를 볼때는 맨 밑의 오류를 확인함.

### Invalid HTTP_HOST header: <your-site-name> . You may need to add <your-site-name> to ALLOWED_HOSTS. 오류 해결
project의 settings.py에서 ALLOWED_HOSTS 설정
```python
#project/settings.py

...
ALLOWED_HOSTS = ['localhost', '127.0.0.1', '[::1]', '.pythonanywhere.com'] #  추가
```

### Secretkey 분리
Secret.json을 사용해 secretkey를 분리하고 깃에 올리지 않았을 때는 bash에서 vi 편집기로 secret.json을 생성해준다. 

```
$ vi secret.json # secret.json 파일 생성


# vi
# secret.json
{
    "SECRET_KEY":"~!@!@$!@~$~!"
}
``` 
### media 설정
Debug=False 일 때,media 파일들이 보이지 않는 오류 해결.  

- settings.py 설정
    ```python
    # proejct/settings.py
    ...
    MEDIA_URL = '/media/'
    MEDIA_ROOT = '/home/<your-id>/<project-path>/media'
    ```
- WEB APP의 static files 설정
    - url에 설정한 MEDIA_URL 입력.
    - directory에 설정한 MEDIA_ROOT 입력.

이 방법으로 Debug=False일 때 media 파일을 서빙할 수 있다.
## refer
http://www.itworld.co.kr/news/106437#csidx59998154441e30d841a9ea9879ac611 