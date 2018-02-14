# URL 설계
- url은 웹 클라이언트에서 호출한다는 시점에서 보면 웹 서버에 존재하는 애플리케이션에 대한 api라고 할수 있음.
- 이 api의 명명 규칙을 정하는 법은 두가지로 분류 할 수 있다.
## RPC(Remote Procedure Call)
-  클라이언트가 네트워크상에서 원격에 있는 서버가 제공하는 api 함수를 호출하는 방식
- url 설계와 api 설계를 동일하게 고려하여 url의 경로를 api 함수 명으로, 쿼리 파라미터를 함수의 인자로 간주함.

    /blog/search?q=test&debug=true

- 웹 클라이언트에서 url을 전송하는 것이 웹 서버의 api 함수를 호출한다고 인식.
- 경로의 대부분이 동사가 됨. 왜? 함수나 메소드 명이 일반적으로 동사이기 때문.

## REST(Representational State Transfer)
- 웹 서버에 존재하는 요소를 모두 리소스(자원)라 정의하고 url을 통해 웹 서버의 특정 리소스를 표현하는 개념.
- 리소스에 대한 조작을 GET,POST,PUT,DELETE http 메소드로 맵핑
- 웹 클라이언트에서 url을 전송하는 것이 웹 서버에 있는 리소스 상태에 대한 데이터를 주고 받는 것으로 간주 
- url를 자원으로 표현함. (동사 없이 명사,자원이름으로 구성함)

        /user/sunny/profile/address
        
- 위 url은 http 메소드마다 다른 행동을 할 수 있다.
    + GET일 경우: 사용자 주소를 조회
    + PUT일 경우: 사용자 주소를 수정
    + DELETE일 경우: 사용자 주소를 삭제


## 간편 url
- 쿼리스트링 없이 경로만 가진 간단한 구조의 url
- 검색엔진의 처리를 최적화 하기위해 생겨남.

    http://example.com/index.php?page=foo // 기존 url 

    http://exmpale.com/foo // 간편 url


