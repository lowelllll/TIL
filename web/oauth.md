# oAuth
> 인증을 위한 오픈 스탠더드 프로토콜로, 사용자가 Facebook이나 트위터 같은 인터넷 서비스의 기능을 다른 애플리케이션(데스크톱, 웹, 모바일 등)에서도 사용할 수 있게 한 것

- 다른 서비스와 나의 서비스가 서로 상호작용 할 수 있음.
- oAuth를 통해 `AccessToken`를 얻어 서비스에 접근해 데이터를 CRUD 할 수 있음.
- ex) 나의 애플리케이션이 google의 <b>부분적인 서비스(일정,주소록)</b>에 접근해 데이터를 CRUD함.

## 역할
#### resource server 
제어하고자 하는 자원을 가지고 있는 서버
#### resource owner 
자원의 소유자
#### client
resource server에 자원을 가져가고자 하는 클라이언트
#### authorization server
인증과 관련된 처리를 전담하는 서버

## 등록
client는 자원을 사용하기 위해 resource server에 등록을 해야함.

- client ID  
    애플리케이션 식별자(resource server에게 부여받음)
- client Secret  
    애플리케이션 Secret code(resource server에게 부여받으며 절대 노출되면 안됨)
- authorization redirect urls   
    resource server에게 authorize code를 받을 주소

## 과정
1. owner가 resource server 로그인을 하고 로그인 할 때는 owner가 client id와 redirect url을 같이 보냄.
2. resource server는 client id와 redirect url이 저장되어 있고 일치한 지 확인 후 owner에게 client 서비스에 기능(scope)을 제공해도 되는지 물어봄.  
    `http://resource.server/?client_id=1&scope=b,c&redirect_url=redirecturl`
3. owner가 승인 버튼을 눌렀을 경우 user id가 부분적인 기능(scope)에 대해 허용했다고 저장함.
4. resource server는 `authorization code`를 resource owner를 통해(redirect()) client로 전송함.
5. client는 `access code`를 받기 위해 `authorization code`와 client ID,client Secret를 resource server에 전송함.
6. resource server는 client에게 받은 client ID,client Secret, authorization code가 일치한지 확인 후 `access code`를 보냄.

### refresh token
 `access token` 은 수명이 있음.  
`access token`을 발급 받을 때 `refresh token`도 같이 발급 받아 저장해놓고 `access token`의 수명이 끝나면 새로 발급해달라는 요청을 `refresh token`과 함께 보내면 새로운 `access token`을 발급 받을 수 있음.