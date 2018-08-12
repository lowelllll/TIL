# oAuth
> 인증을 위한 오픈 스탠더드 프로토콜로, 사용자가 Facebook이나 트위터 같은 인터넷 서비스의 기능을 다른 애플리케이션(데스크톱, 웹, 모바일 등)에서도 사용할 수 있게 한 것

- 다른 서비스와 나의 서비스가 서로 상호작용 할 수 있음.
- oAuth를 통해 AccessToken를 얻어 서비스에 접근해 데이터를 CRUD 할 수 있음.
- ex) 나의 애플리케이션이 google의 <b>부분적인 서비스(일정,주소록)</b>에 접근해 데이터를 CRUD함.

### Role
#### resource server 
제어하고자 하는 자원을 가지고 있는 서버
#### resource owner 
자원의 소유자
#### client
resource server에 자원을 가져가고자 하는 클라이언트
#### authorization server
인증과 관련된 처리를 전담하는 서버

### register
client는 자원을 사용하기 위해 resource server에 등록을 해야함.

- client ID  
    애플리케이션 식별자(resource server에게 부여받음)
- client Secret  
    애플리케이션 Secret code(resource server에게 부여받으며 절대 노출되면 안됨)
- authorization redirect urls   
    resource server에게 authorize code를 받을 주소