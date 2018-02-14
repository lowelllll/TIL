# 파이썬 웹 표준 라이브러리
## 웹 라이브러리
- 파이썬은 웹 프로그래밍을 위한 라이브러리를 제공하며 이 라이브러리는 웹 서버 프로그래밍/웹 클라이언트 프로그래밍을 위한 라이브러리로 분류되어있다.
![파이썬 웹 표준 라이브러리 구성](http://cfile29.uf.tistory.com/image/213E724455D079E3038C3C)
- 웹 서버:web framework -> http.cookie,http.server
    + 웹 프레임워크는 사용자 프로그램과 저수준의 http.server 라이브러리 중간에 위치하여 웹 서버의 개발을 좀 더 편하게 해주면서 표준 라이브러리의 기능을 확장해주는 역할을 한다.
- 웹 클라이언트:urllib 패키지(고수준 api 제공) -> http.client,http.cookiejar (저수준 api 제공) 

# 웹 클라이언트 라이브러리
- 웹 클라이언트를 구현할 수 있도록 라이브러리를 제공한다.

## urlparse()
> url을 분해,조립,변경처리를 한다.

    >>>from urlparse imort urlparse
    >>> result  = urlparse("http://www.python.org:80/gudio/python.html;philosophy?overall=3#n10")
    >>> result
    ParseResult(scheme='http', netloc='www.python.org:80', path='/gudio/python.html', params='philosophy', query='overall=3', fragment='n10')

+ schem : 프로토콜
+ netloc : 네트워크 위치
+ path : 파일의 경로
+ params : 애플리케이션에 전달될 매개변수
+ query: 쿼리스트링
+ fragement : 문서내 앵커 등 조각 지정(?)

## urllib2
> 주어진 url에서 데이터를 가져오는 기본 기능을 제공
### urlopen(url,data=None,[timeout])
- url 인자로 지정한 url로 연결하고 유사 파일을 객체를 반환함.
- urlopen()만 잘다뤄도 클라이언트를 작성할 수 있음.
- url에 file 스킴을 지정하면 파일을 열 수 있음.
- url 인자는 문자열이나 request 클래스의 인스턴스.
- default 요청은 GET이며 POST 요청은 data인자에 질의문자열을 지정하면 됨.
- timeout : 응답을 기다리는 타임아웃 시간

#### urlopen()을 이용한 다양한 처리
- GET,POST 요청 처리 : urlopen()만 사용
    + GET 
        + www.naver.com 을 접속한 것과 같은 데이터를 웹서버로 가져옴
        + 브라우저는 html 형식의 데이터를 해석하여 보여주지만 파이썬 프로그램은 그대로 보여줌.

                >>> from urllib2 import urlopen
                >>> f = urlopen("http://www.naver.com")
                >>> f.read(500)
    
    + POST
        + data 인자를 지정해주면 함수는 자동으로 POST 방식으로 요청을 보냄.(실제 동작을 확인하려면 POST 요청을 처리할 수 있는 서버가 필요함)

                >>> f = urlopen("http://www.naver.com",data)
                >>> f.read(500)
    
- 요청 헤더 추가, 변경이 필요한 경우 : request 클래스 사용
    + url 인자에 문자열 대신 request 객체를 지정
    + request 객체를 생성하고 add_header()와 add_data() 메소드로 헤더와 데이터를 추가하여 웹 서버로 요청을 보냄

            >>> import urllib2
            >>> req = urllib2.Request("http://www.naver.com") #request 객체 생성
            >>> req.add_header("Content-Type","text/plain")
            >>> req.add_data("query=python") #POST method
            >>> f = urllib2.urlopen(req)
            >>> print f.read(300)


- 인증,쿠키,프록시 등 복잡한 요청 처리 : 인증,쿠키,프록시 해당 핸들러 클래스 사용
    + 각 기능에 맞는 핸들러 객체를 정의하고 그  핸들러를 build_opener() 함수를 사용하여 opener로 등록
    + opener를 urlopen() 함수로 열기 위해선 install_opener() 함수를 사용하여 default opener로 설정하면 됨.
    + 인증
        + HTTPBasicAuthHandler 클래스를 사용하여 인증 데이터를 같이 보냄
               
                >>> import urllib2
                >>> auth_handler = urllib2.HTTPBasicAuthHandler()
                >>> auth_handler.add_password(realm='PDQ Application',
                ...                             uri='https://mahler:8092/site-updates.py',
                ...                             user='lowell',
                ...                             passwd='opppennner!') # 객체 정의
                >>> opener = urllib2.build_opener(auth_handler) # 오프너 등록
                >>> urllib2.install_opener(opener) # 디폴드 오프너로 설정
                >>> u = urllib2.urlopen('http://www.example.com/login.html') # 정상적을 동작하기 위해선 인증요청을 받는 서버의 인증요청 api에 대한 url을 정확히 입력해야함

   + 쿠키
        + HTTPCookieProcessor 클래스를 사용하여 쿠키 데이터를 같이보냄

                >>> import urllib2
                >>> cookie_handler = urllib2.HTTPCookieProcessor() # 쿠키 핸들러 생성, 쿠키 데이터 처리느 디폴트로 CookieJar 객체를 사용함.
                >>> opener = urllib2.build_opener(cookie_handler)
                >>> urllib2.install_opener(opener)
                >>> u = urllib2.urlopen('http://www.example.com/login.html')  
    + 프록시 
        + ProxyHandler,ProxyBasicAuthHandler 클래스를 사용해 프록시 서버를 통과해 웹 서버로 요청을 보냄.

                import urllib2
                >>> proxy_handler = urllib2.ProxyHandler({'http':'http://www.example.com:3128/'})
                >>> proxy_auth_handler = urllib2.ProxyBasicAuthHandler()
                >>> proxy_auth_handler.add_password('realm','host','username','password')
                >>> opener = urllib2.build_opener(proxy_handler,proxy_auth_handler)
                >>> u = opener.open('http://www.example.com/login.html')
                # install_opener(),urlopen() 함수 대신에 직접 open 함수를 사용할 수 있음

### urllib2 모듈로 웹사이트에서 이미지 리스트 불러오기
- 특정 웹 사이트에서 이미지만을 검색하여 그 리스트를 보여주는 코드

        from urllib2 import urlopen
        from HTMLParser import HTMLParser

        class ImageParser(HTMLParser): # HTMLParser 클래스를 사용할땐 상속받는 클래스를 정의하고 필요한 내용을 오버라이드 함.
            def handle_starttag(self,tag,attrs): # image 태그를 찾기위해 handler_starttag() 함수 오버라이드
                if tag != 'img': 
                    return
                if not hasattr(self,'result'): 
                    self.result= []
                for name , value in attrs: # img src 속성을 찾으면 속성 값을 self.result 리스트에 추가함.
                    if name == 'src':
                        self.result.append(value)

        def parseImage(data): # HTML 문장이 주어지면 ImageParser 클래스를 사용해서 이미지를 찾고 그 리스트를 출력해주는 함수
            parser = ImageParser()
            parser.feed(data) # HTML 문장을 feed() 함수에 주면 바로 파싱하고 그 결과를 parser.result 리스트에 추가함
            dataSet = set(x for x in parser.result) # 파싱결과를 set타입의 dataSet으로 모아줌
            print '\n'.join(sorted(dataSet)) # dataSet으로 모은 파싱 결과를 정렬한 후에 출력함

        def main(): # 메인함수 구글의 사이트를 검색하여 이미지를 찾는 함수
            url = 'http://www.google.co.kr'

            f = urlopen(url)
            charset = f.info().getparam('charset') # 사이트에서 가져오는 데이터는 인코딩된 데이터이므로 인코딩 방식을 알아내어 그 방식으로 디코딩을 해줌
            data = f.read().decode(charset)
            f.close()

            print "\n>>>>>>>> Fetch Images from",url
            parseImage(data) # 이미지를 찾기 위해 parseimage 함수를 호출함


        if __name__ == '__main__':
            main()  # 프로그램을 시작하기 위해서 main함수를 호출함.

## httplib 
- urllib2 모듈에 없는 GET,POST 이외의 방식으로 요청을 보내거나 요청 헤더와 바디 사이에 타이머를 두어 시간을 지연시키는 등의 경우, http 프로토콜 요청에 대한 저수준의 더 세밀한 기능이 필요할때 사용
- urllib2도 httplib 모듈에서 제공하는 api를 사용하여 만든거임

### httplib 모듈 사용시 코딩 순서
1. 연결 객체 생성

    conn=httplib.HTTPConnection('www.python.org') #Host

2. 요청을 보냄 

    conn.request("GET","index.html")

3. 응답 객체 생성

    response = conn.getresponse()

4. 응답 데이터 읽음 

    data = response.read()

5. 연결을 닫음

    conn.close()

### http method에 따른 요청 방법
- GET 요청

        >>> import httplib
        >>> conn = httplib.HTTPConnection("www.example.com") # url이 아닌 host를 넣어야함 http://이거 넣으면 안됨
        >>> conn.request("GET",'/index.html') # request(method,url,body,headers)
        >>> r1 = conn.getresponse()
        >>> print r1.status,r1.reason # 응답 헤더 정보가 들어있음
        200 OK
        >>> data1= r1.read() # 데이터를 모두 읽어야 다음 request() 요청을 할 수 있음.
        >>> conn.request("GET","/parrot.spam")
        >>> r2 = conn.getresponse()
        >>> print r2.status,r2.reason
        404 Not Found
        >>> data2 = r2.read()
        >>> conn.close()

- HEAD 

        >>> import httplib
        >>> conn = httplib.HTTPConnection("www.example.com")
        >>> conn.request("HEAD","/index.html")
        >>> res = conn.getresponse()
        >>> print res.status,res.reason
        200 OK
        >>> data = res.read()
        >>> print len(data) 
        0
        >>> data == ''
        True
        # head 요청에 대한 응답에 헤더는 있지만 바디는 없으므로 길이는 0

- POST

        >>> import httplib,urllib
        >>> params = urllib.urlencode({'@number':1234,'@type':'issue','@action':'show'})
        # POST 요청으로 보낼 파라미터에 대해 URL 인코딩
        >>> headers = {"Content-type":'application/x-www-form-urlencode',"Accept":"text/plain"} 
        >>> conn = httplib.HTTPConnection("bugs.python.org")
        >>> conn.request("POST","",params,headers)
        >>> response = conn.getresponse()
        >>> print response.status,response.reason
        301 Moved Permanently
        >>> data = response.read()
        >>> data
        '<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">...
        >>> conn.close()

- PUT 

        >>> import httplib
        >>> conn = httplib.HTTPConnection("localhost",8888)
        >>> BODY = "***filecontents***"
        >>> conn.request("PUT","/file",BODY) 
        >>> res = conn.getresponse()
        >>> print res.status,res.reason
        200 ok # 웹 서버에서 PUT 요청을 정상적으로 처리하여 / file이라는 파일을 만들고 그 내용에 BODY에서 지정한 문자열을 기록함.

### httplib 모듈을 활용해 웹사이트에서 이미지 다운로드 하기
- 특정 웹사이트에서 이미지를 찾아 다운로드하는 프로그램

        import httplib
        from urlparse import urljoin,urlunparse
        from urllib import urlretrieve
        from HTMLParser import HTMLParser
        import os

        class ImageParser(HTMLParser):
            def handle_starttag(self, tag, attrs): # img 태그 찾기
                if tag != 'img':
                    return
                if not hasattr(self,'result'):
                    self.result= []
                for name,value in attrs:
                    print "attrs name:"+name+" value:"+value
                    if name == 'src': # img src 속성을 찾으면 속성 값을 self.result에 추가
                        self.result.append(value)


        def downloadImage(srcUrl,data): # html 문장이 주어지면 ImageParser 클래스를 사용하여 이미지를 찾고 그 이미지들을 다운로드하는 함수
            if not os.path.exists('DOWNLOAD'): # DOWNLOAD 폴더가 없을 시 만들어줌
                os.makedirs('DOWNLOAD')

        parser = ImageParser()
        parser.feed(data) # html 문장을 feed() 함수에 주면 바로 파싱하고 그 결과를 parser.result에 추가함
        resultSet = set(x for x in parser.result) # 파싱 결과를 set타입의 resultSet으로 모아줌 (집합자료형)

        for x in sorted(resultSet):# resultSet으로 모은 파싱결과를 정렬한 후에 하나씩 처리함
            src = urljoin(srcUrl,x)  # 다운로드 하기위해 소스 url과 타겟 파일명을 지정
            # urljoin() baseURL과 파일명을 합쳐 완전한 URL을 리턴하는 함수
            basename = os.path.basename(src)  # 입력받은 경로의 기본 이름(base name)을 반환합니다.
            targetFile = os.path.join('DOWNLOAD',basename) # 이미지 저장할 경로
            urlretrieve(src,targetFile) # urlretrieve() src로부터 파일을 가져와서 tagetFile 파일로 생성해줌.


      def main():
        host = "www.google.co.kr"

        conn = httplib.HTTPConnection(host)
        conn.request("GET",'')
        resp = conn.getresponse()

        charset = resp.msg.getparam('charset')
        data = resp.read().decode(charset)
        conn.close()

        print "\n>>>>>>>>> Download Images from",host
        url = urlunparse(('http',host,'','','','')) # url 요소 6개를 튜플로 받아 조립하여 url을 리턴함.
        downloadImage(url,data) # 이미지 다운로드 함수 호출


        if __name__ == "__main__":
            main()
- - - 
# 웹 서버 라이브러리
- 웹 서버 역할 : http 통신에서 클라이언트의 요청을 받고 이를 처리하여 그 결과를 되 돌려주는 것
- 웹 서버 개발자는 프레임워크를 활용하여 응용 로직만 개발하면 효율적이지만 웹 서버 라이브러리를 알면 웹 프레임워크가 어떻게 동작하고 웹 서버 라이브러리가 웹 프레임워크에 어떻게 사용되는지 기술을 파악할 수 있음.

## 웹 서버를 만드는 기본적인 방법

    from BaseHTTPServer import HTTPServer,BaseHTTPRequestHandler # 1

    class MyHandler(BaseHTTPRequestHandler): # 2
        def do_GET(self):
            self.wfile.write("Hello World!")

    if __name__ == "__main__":
        server = HTTPServer(('',8888),MyHandler) # 3
        print "Started WebServer on port 8888..."
        print "Press ^C to quit WebServer"
        server.serve_forever() # 4

1. BaseHTTPServer 모듈 import
2. BaseHTTPRequsetHandler를 상속받아 원하는 로직으로 핸들러 클래스를 정의
3. 서버의 IP,PORT 및 핸들러 클래스를 인자로 하여 HttpServer 객체를 생성
4. HttpServer 객체의 serve_forever()호출 


## 웹 서버를 만드는데 필요한 라이브러리
- BaseHTTPServer
    + 웹서버의 기반인 서버 클래스.
    + http 프로토콜 처리를 해주므로 따로 http 프로토콜 관련된 로직을 작성하지 않아도 됨.
    + 서버클래스 : HTTPServer 정의
    + 핸들러 클래스 : BaseHTTPRequestHandler 정의
    + 테스트용 웹 서버 실행 : test() 함수
- SimpleHTTPServer
    + 기반 서버클래스 HTTPServer를 임포트하여 사용
    + 핸들러 클래스 : SimpleHTTPRequestHandler 정의
    + 테스트용 웹 서버 실행 : test() 함수
    + do_GET,do_HEAD 메소드가 정의되어있어 GET,HEAD 방식 처리 가능
    + 간단한 핸들러가 구현되어있어 즉시 웹 서버를 실행할 수 있음(웹 서버를 만들기위해 직접 핸들러를 구현하지 않아도 됨)
    + SimpleHTTPServer 실행
        + linux,mac    

                $ python -m SimpleHTTPServer 8888 
        + windows

                python -m http.server 8888

- CGIHTTPServer 
    + 기반 서버클래스 HTTPServer를 임포트하여 사용
    + 핸들러 클래스 : CGIHTTPRequestHandler 정의
    + 테스트용 웹 서버 실행 : test() 함수
    + SimpleHTTPRequestHandler 클래스를 상속받고 있어 get,head 처리 가능
    + 즉시 웹서버 실행 가능
    + POST(do_POST)와 CGI 처리 가능
    + CGIHTTPServer 실행
        + linux,mac

                $ python -m CGIHTTPServer 8888
        + windows 

                python -m http.server 8888 --cgi # 정확하지 않음

    ### CGIHTTPServer 예시
    cgi_client.py

        import urllib2
        from urllib import urlencode

        url = "http://localhost:8888/cgi-bin/script.py"
        data = {
            'language':'python',
            'framework':'django',
            'email':'lowell2735@gmail.com'
        }

        encData = urlencode(data)

        f = urllib2.urlopen(url,encData)
        print f.read()
    
    cgi-bin/script.py 
        
        # -*- coding:UTF-8 -*-
        import cgi

        # cgi 스크립트 파일은 cgi-bin 디렉토리 하위에 위치해야함
        # 파일 엑세스 모드 755로 변경해야함
        form = cgi.FieldStorage() # 클라이언트 요청에 담겨진 질의 문자열을 엑세스 하기 위해서 fieldstorage() 인스턴스 생성
        language = form.getvalue('language')
        framework = form.getvalue('framework')
        email = form.getvalue('email')

        print "Content-type: text/plain"
        print '\r'
        print "Welcome, CGI Scripts"
        print "language is ",language
        print "framework is ",framework
        print "email is",email

## xxxHTTPServer 모듈 간 관계
![xxxHTTPServer 모듈의 클래스 간 상속도](http://cfile5.uf.tistory.com/image/236D6E3955D07D4E1AB506)
- 핸들러 클래스는 각 모듈마다 정의되며 상위 모듈의 핸들러 클래스를 상속받아 정의하고 있음. (기반 핸들러 BasicHTTPRequestHandler 확장)
- CGI 모듈에서 test()를 호출해도 결국엔 base모듈의 test()를 호출하게 되는것
- base 모듈의 tes()는 HTTPServer 객체를 참조하는데 HTTPServer 객체의 Serve_forever()를 호출하여 웹 서버 기동

## CGI/WSGI 라이브러리
- WSGI(Web Server Gateway Interface)는 웹 서버와 웹 애플리케이션을 연결해주는 규격으로 웹 프레임워크를 개발하거나 이런 웹 프레임워크를 웹 서버와 연동할 때 사용함.
- 파이썬 애플리케이션을 실행하고자 하는 웹 서버는 이 규격을 준수해야함.
- CGI 기술의 단점을 개선하고 파이썬 언어에 맞게 재구성 한 것
- WSGI 규격을 구현하기 위해 cgi모듈과 wsgiref 모듈이 존재함.(CGI 기술은 사라졌지만 라이브러리는 WSGI에 사용되고있음) 

### CGI 관련 모듈
- CGI : 웹 서버와 애플리케이션 간에 데이터를 주고 받기 위한 규격
- 파이썬에서는 CGI처리를 위한 CGIHTTPServer 모듈과 CGI 모듈 제공하며
WSGI 기술을 사용하여 CGI를 처리함.'

### WSGI 개요
- CGI 방식은 요청이 들어올 때 마다 처리를 위한 프로세스를 생성하는 방식이라 서버에 부하가 될 가능성이 높음

### WSGI 규격
- 웹 서버에 독립적인 웹 애플리케이션을 작성할
수 있도록 웹 서버와 웹 애플리케이션 간의 연동 규격을 정의한 것
- 웹 애플리케이션을 한번만 작성하면 다양한 웹 서버에서 동작할 수 있게됨.
- 파이썬의 웹 프레임워크는 대부분 WSGI 규격을 준수함.

### WSGI 서버 애플리케이션 처리 과정
- 파이썬 웹 프레임워크의 대부분은 WSGI 서버를 제공하며 애플리케이션 개발자는 WSGI 서버에 대한 API 규격만 맞추면 웹 서버와는 독립적으로 애플리케이션을 작성할 수 있어 생산성이 높아짐.

### WSGI 규격에 따라 애플리케이션을 개발할 때 중요한 사항
1. 개발이 필요한 애플리케이션을 함수/클래스의 메소드로 정의하여 애플리케이션의 인자는 다음과 같이 정의

        def application_name(environ,start_response):

    + environ 
        + 웹 프레임워크에 이미 정의되어있으며 HTTP_HOST,HTTP_USER_AGENT.. 같은 HTTP 환경변수를 포함함.
    + start_response
        + 애플리케이션 내에서 응답을 시작하기 위해 반드시 호출해야하는 함수

2. start_response 함수의 인자

        start_response(status,headers)

    + status 
        + 응답코드
    + headers 
        + 응답 헤더 포함

3. 애플리케이션 함수의 리턴 값은 응답 바디에 해당되는 내용으로 리스트나 제너레이터 같은 iterable 타입이어야 함.

### wsgiref.simple_server 
- 웹 프레임워크 개발자가 웹 서버와의 연동 기능을 개발할 수 있도록 제공하는 모듈
- WSGI 스펙을 준수하는 웹 서버(WSGI 서버)에 대한 참조 서버, 개발자가 참고가 될 수 있도록 미리 만들어 놓은 WSGIServer 클래스, WSGIRequestHandler 클래스를 정의하고 있다.
- 모든 프레임워크가 wsgiref 패키지를 사용하는건 아님

#### WSGI 서버 예제

    def my_app(environ,start_response):
        status = '200 ok'
        headers = [('Content-type','text/plain')]
        start_response(status,headers)

        return ["this is a sample WSGI Application"]

    if __name__ == "__main__":
        from wsgiref.simple_server import make_server

        print "Started WSGI Server on port 8888.."
        server = make_server('',8888,my_app)
        server.serve_forever()


+ WSGI 서버도 웹 서버이므로 웹 서버를 만드는 방식을 따름(핸들러 클래스가 아닌 애플리케이션 함수를 설정하여 객체를 생성함)

# reference 
[파이썬 웹 프로그래밍: Django(장고)로 배우는 쉽고 빠른 웹 개발](http://www.hanbit.co.kr/store/books/look.php?p_code=B5790464800)
