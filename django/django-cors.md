# Django-CORS
### problem
django로 로 만든 메뉴봇 api(localhost:8888)에 같은 로컬의 다른 서버(localhost:8000)ajax 요청을 했을 때,   
` No 'Access-Control-Allow-Origin' header is present on the requested resource.`  
이러한 오류가 났다.   

예전에도 이 오류를 만나본 적이 있었는데 "이거 다른 도메인이나 호스트일때만 그런 걸로 알았는데..?" 해서 다시 알아 보게 되었다.

## 동일 출처 정책 (Same-Origin Policy)
> 웹 애플리케이션 보안 모델

이 정책에 의해 자바스크립트(XMLHttpRequest)로 다른 웹 페이지에 접근할 때는 <b>같은 출처</b>의 페이지만 접근이 가능하다.

같은 출처는 <i>프로토콜,호스트명,포트</i>가 같다는 것을 의미한다.

`<script></script>`로 둘러싸인 스크립트에서 <b>Cross-Site HTTP Request</b>는 제한되어있기 때문에 웹 페이지의 스크립트는 그 페이지와 같은 서버에 있는 주소로만 ajax요청을 할 수 있다.

### Cross-Site HTTP Request
> 처음 전송되는 리소스의 도메인과 다른 도메인으로부터 리소스가 요청될 경우 해당 리소스는 cross-origin HTTP 요청에 의해 요청됩니다

[출처](https://developer.mozilla.org/ko/docs/Web/HTTP/Access_control_CORS)

## CORS(Cross-origin Resouce Sharing)
> 웹 브라우저에서 외부 도메인 서버와 통신하기 위한 방식을 표준화한 스펙.

동일 출처 정책으로 ajax를 사용하는 사람들의 불만이 많아져 추가된 정책으로 외부 요청을 허용할 경우 ajax요청이 가능해진다!

그래서 허용은 어떻게 해?

https://brunch.co.kr/@adrenalinee31/1

이 곳에 다양한 허용 방법/해결 방법이 나와있다.
django에서 해결하는 방법만 따로 알아보자.

## solution
### django에서 cors해결 (서버 측)
1. `django-cors-headers` 설치
    ```
    pip install django-cors-headers
    ```
2. app 등록
    ```python
    INSTALLED_APPS = [
        ...
        'corsheaders',
    ]
    ```
3. 미들웨어 등록
    ```python
    MIDDLEWARE = [
        ...
        'corsheaders.middleware.CorsMiddleware',
    ]
    ```
4. Credentials를 수용
    ```python
    CORS_ALLOW_CREDENTIALS = True
    ```
    쿠키가 크로스 사이트 HTTP 요청에 포함하도록 허용하는 것 .. 이라고 한다.
    디폴트는 false
5. Origin 설정
    ```python
    CORS_ORIGIN_ALLOW_ALL = True # develop 모두 허락하기

    # CORS_ORIGIN_WHITELIST = ( 배포 시 요청가능한 IP/도메인 설정 
    #   'ip' # 지정된 호스트만 허락
    # )
    ```

## refer
http://makerj.tistory.com/225  
https://developer.mozilla.org/ko/docs/Web/HTTP/Access_control_CORS
http://webfortj.blogspot.com/2014/05/cors-cross-origin-resource-sharing.html
https://brunch.co.kr/@adrenalinee31/1

