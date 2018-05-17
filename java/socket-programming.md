# Socket
## 소켓
> 네트워크에 연결된 각 호스트 간에 데이터를 주고 받을 때 사용하는 도구

- 소켓을 이용해 상대 측과 접속되면 파일시스템,다른 I/O 처럼 사용이 가능함.
- 각 호스트와 연결을 하고 자료를 주고 받을 수 있는 데이터 송수신의 창구역할을 함.
- 클라이언트에서 소켓 객체를 만들면 소켓은 주어진 IP 주소와 포트번호로 연결을 시도함.
- <b>포트:소켓끼리 연결하는 채널</b>
- 서버 소켓은 다른 소켓을 만들어 클라이언트 소켓과 연결시켜줌.

## Socket 예외처리
- Socket 생성 시 도메인 네임 서버 (DNS)가 호스트 명을 인식하지 못하거나 작동하지 않으면 <b>UnKnownHostException</b> 예외가 발생함.  
- 어떤 이유로 소켓이 열리지 않을 경우엔 <b>IOException</b> 예외가 발생함.

## Socke 입출력
#### 문제
클라이언트의 Socket을 생성한 다는 것은 <b>통신을 원하는 원격 호스트와 접속을 시도하고 접속이 가능하면 데이터 송수신을 위한 소켓을 만드는 것</b>이다.  
하지만 Socket 클래스로부터 생성된 객체는 접속 여부를 판단해 데이터를 송수신할 준비만 할 뿐, 송수신을 지원해주진 않는다.

#### 해결
실제 응용프로그램이 생성된 소켓을 이용해 데이터 송수신하기 위해선 응용프로그램과 소켓의 연결을 위해 입출력 스트림을 사용한다.  
read() 메소드와 write() 메소드를 사용해 데이터를 읽거나 씀.

### getInputStream
> 소켓에서 데이터를 읽어 들일 수 있는 입력스트림. InputStream 객체를 반환함.
```java
try{
    Socket socket = new Socket(host,ip);
    InputStream is = socket.getInputStream();
    DataInputStream datais = new DataInputStream(is);
    // 더 많은 기능을 위해
}
```

### getOutputStream
> 소켓에 데이터를 써 넣을 수 있는 OutputStream 객체를 반환함.
```java
writer out;
try{
    Socket http = new Socket(host,ip);
    OutputStream os = http.getOutputStream();
    OutputStream buffered = new BufferedOutputStream(os);
    out = new OutputStreamWriter(buffered,"ASCII");
    out.write("..");
    // 서버의 응답을 읽음
}catch(IOException e){
    //예외처리
}finally{
    try{
        out.close()
    }catch(Exception e){}
}
```

## 소켓 정보 다루기
소켓 객체의 정보를 접근할 수 있는 메소드
- `public int getPort()` : Socket이 원격 호스트의 몇번 포트에 연결되어있는지 확인.
- `public int getLocalPort()` : localhost의 포트번호를 알고싶을 때 사용.
- `public inetAddress getLocalAddress()` : 어떤 네트워크 환경에 바인드 되어있는지 알 수 있음.


