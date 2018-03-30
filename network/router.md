# 공유기 (router)
> 인터넷 공유기는 정확히 말하면 ‘인터넷 IP 주소 공유기’이다. 즉, 하나의 IP 주소를 여러 대의 컴퓨터가 공유해서 인터넷에 접속할 수 있도록 하는 역할을 하는 것이다  

ipv4 버전의 ip가 동이 나기 시작하면서 공유기는 더욱 필요해졌다.    
부족한 ipv4 주소를 아껴서 사용할 수 있기 때문이다.

### WAN(world area network)
전 세계를 연결하는 네트워크

### LAN(local area network)
공유기를 중심으로 있는 지역 네트워크

### ip 부여
각 기기가 공유기에 연결한 순간 그 기기는 ip를 부여받게 된다.  
공유기에도 ip가 부여되는데 이것을 gateway address,router address라고 함.

#### public ip address 
전 세계에서 사용할 수 있는 ip
#### private ip address 
지역 네트워크 안에서 쓰는 ip(공유기로 연결된,사내전화)

#### 로컬 ip에서 서버 요청 방법
- 서버에 요청 request
- 공유기에서 요청받은 private ip를 기록
- 아이피를 private ip를 public ip로 변경하고요청을 보냄
- public ip로 요청을 받음
- 공유기가 응답을 보고 요청했었던 정보를 파악 후 response
## refer 
https://www.youtube.com/watch?v=3HEhifFPdIs&t=14s 