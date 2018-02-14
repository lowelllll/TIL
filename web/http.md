# HTTP
- HTTP는 Hyper Text Transfer Protocol의 약자로 www 상에서 서버와 클라이언트 사이에서 이루어지는 요청(request)/응답(response) 프로토콜이다.
- TCP/IP 프로토콜 위에서 동작하기 때문에 웹을 이용하려면 웹 서버와 웹 클라이언트는 각각 TCP/IP 동작에 필수적인 IP주소를 가져야 한다.
- HTTP는 하이퍼 텍스트 뿐만 아니라 이미지,음성,동영상,자바스크립트 등 컴퓨터에서 다룰  수 있는 데이터라면 무엇이든 전송 가능하다.
- HTTP 프로토콜 요청/응답 과정
    + 사용자가 http://www.naver.com 접속
    + 웹 클라이언트와 웹 서버 사이에 HTTP 연결이 맺어지고 웹 클라이언트는 웹서버에 HTTP 요청(request)메세지를 보냄.
    + 웹 서버는 요청에 따른 처리를 진행함.
    + 결과를 웹 클라이언트에게 HTTP 응답(response)메세지를 보냄.

이런 방식의 요청 메세지와 응답 메세지가 반복적으로 오가며 웹을 사용함.
## HTTP METHOD
- HTTP 프로토콜에서 웹 서버가 요청과 응답 데이터를 전송하는 방식(서버에 요청하는 방법)
- 8가지의 메소드가 있다
	+ GET
	+ POST
	+ HEAD
	+ OPTIONS
	+ PUT
	+ DELETE
	+ TRACE
	+ CONNECT

### GET,POST
- http method 중 가장 많이 사용하는 메소드는 GET과 POST이다. (html 폼에서 지정할 수 있는 메소드가 둘밖에 없음)
#### GET

	GET http://www.naver.com?query=lowell HTTP/1.1

- uri의 ? 뒷부분에 키=값 쌍으로 이어 붙여 보낸다.
- uri는 길이 제한이 있기 때문에 많은 데이터를 보내기 어려움.
- 전달되는 사용자의 데이터가 주소창에 노출이 되어 보안에 불리함.
- 사용되는 곳:검색,게시판 조회 등

#### POST

	POST http://naver.com/login HTTP/1.1
	Content-Type:application ...

	id = lowell
	password = xxxxxxx

- GET에서 uri에 포함시켰던 파라미터를 HTTP request 메시지의 바디 부분에 넣어 보냄.
- 폼이나 추가적인 파라미터를 서버로 보내는 경우는 POST를 사용함.
	+ 장고도 폼에선 POST 밖에 못씀.
- 글쓰기,수정,보안이 필요한 로그인,회원가입 등

## HTTP 메세지 구조
![HTTP 메세지 구조](http://cfile25.uf.tistory.com/image/1546964E4E239D53091EAE)
- header와 body는 생략 가능하다.
- 요청 메세지 
    
        GET /book/1 HTTP/1.1 # 요청라인 - 요청방식, 요청 uri, 프로토콜 버전
        Host:example.com:8000 # 헤더 이름:값 
        # Host는 필수로 넣어줘야 하며 요청라인에 넣을 수도 있음(http://www.example.com:8000/book/1 HTTP/1.1)

- 응답 메세지

        HTTP/1.1 200 ok # 상태라인 - 프로토콜 버전,상태코드,상태 텍스트(서버에서 처리 결과를 상태라인에 표시)
        Content-Type:Application/xhtml+xml; charset=utf-8 # 헤더
                                                          # 빈줄
        <html> # 바디
        ...
        </html>

### HTTP HEADER
- 웹 서버로 보내는 요청과 요청 데이터를 설명하는 메타 정보가 포함되어있다.
- 전송 가능 메타 정보
	+ HTTP METHOD
	+ 요청 URL
	+ HTTP 프로토콜 버전
	+ 추가적으로 지정된 이름과 값 등
	
    
## HTTP 상태코드
- 서버에 요청한 것이 정상적으로 작동했는지에 대한 응답
- 응답은 5개의 클래스로 분류된다.
	+ 정보 응답(1XX)
	+ 성공(2XX)
	+ 리디렉션(3XX)
	+ 클라이언트 오류(4XX)
	+ 서버 오류(5XX)
- [HTTP 상태코드 자세히](https://developer.mozilla.org/ko/docs/Web/HTTP/Status)

