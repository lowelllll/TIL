# TCP/IP
> 데이터 송수신에 관한 일련의 작업을 하나로 모은 것
## Network
### network
> 정보나 노동력 등 어떤 자산을 서로 주고 받을 수 있는 상태
### computer network
> 컴퓨터끼리 케이블이나 적외선 전파 등 어떤 수단을 사용하여 연결해 다양한 데이터를 주고 받을 수 있는 상태

#### 컴퓨터 네트워크 규모
- LAN
- WAN
- INTERNET
- INTRANET(LAN)
    - 인터넷 기술을 사용한 지역 한정판 LAN

### protocol 
> 컴퓨터끼리 문제없이 교류할 수 있도록 정해진 절차

## TCP/IP
### TCP/IP 배경
> 1960년 대 미국 국방성이 개발한 ARPANET이라는 네트워크 상에서 사용할 프로토콜로 개발됨. ARPANET은 4개의 LAN을 연결한 것으로 현재 인터넷의 원형이 됨.

### TCP/IP를 사용한 다양한 통신 서비스
- WWW(World Wide Web)
- 전자메일
- 파일공유
- 원격 로그인
- 등

### TCP/IP 절차
> 데이터를 신호로 변환 -> 전달 -> 데이터 재 변환 

#### TCP/IP의 구조
데이터를 신호로 바꾸거나 신호를 데이터로 되돌리는데 5단계 절차를 거친다.  
각 단계를 계층(layer)라고 한다.  
각 층에는 다양한 프로토콜이 준비되어있음.(TCP,IP도 그중 하나)
##### TCP/IP 프로토콜 군
- 애플리케이션층
    - 애플리케이션에 맞춰 통신을  수행할 수 있도록 함. 
    - 애플리케이션마다 다양한 프로토콜 존재
    - HTTP,SMTP,POP3,FTP 등
- 트랜스포트층
    - 송신되는 데이터를 수신측 애플리케이션에 확실히 전달하기 위해서 작동
    - TCP,UDP
- 네트워크층
    - 수신측 컴퓨터까지 데이터를 전달하기 위해 작동.
    - 전달된 데이턱라 손상되었는지 또는 수신측이 잘 받았는지에는 관여하지 않음.
    - IP
- 데이터링크층
    - 네트워크에 직접 연결된 기기 간을 전송할 수 있도록 만듬.
    - 네트워크층과 물리층 간의 차이를 완전히 흡수하기 위한 다양한 프로토콜 존재.
    - Ethernet,FDDI,ATM 등
- 물리층
    - 데이터를 신호로, 신호를 데이터로 변환.
    - 변환 방법은 통신 매체에 의존하기 때문에 특정 프롵콜이 정해지지 않음.

아래로 내려갈 수록 기계와 가깝다.

### TCP/IP 특징
데이터를 일정한 크기로 분할하여 보냄.  
작게 나눠진 데이터를 패킷이라고 한다.
이 통신 방법을 <b>패킷 통신</b> 이라고 함.

#### 패킷통신 장점
- 데이터를 세분화 함으로써 하나의 회선을 사용하여 여러 데이터를 거의 동시에 송수신 가능
- 데이터의 일부가 손상되어도 해당 부분만 다시 보내면 됨.

## refer
[TCP/IP가 보이는 그림책](http://www.cyber.co.kr/shop/goods/goods_view.php?goodsno=5993&category=020040)