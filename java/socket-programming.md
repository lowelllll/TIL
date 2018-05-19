# Socket
## 소켓
> 네트워크에 연결된 각 호스트 간에 데이터를 주고 받을 때 사용하는 도구

- 소켓을 이용해 상대 측과 접속되면 파일시스템,다른 I/O 처럼 사용이 가능함.
- 각 호스트와 연결을 하고 자료를 주고 받을 수 있는 데이터 송수신의 창구역할을 함.
- 클라이언트에서 소켓 객체를 만들면 소켓은 주어진 IP 주소와 포트번호로 연결을 시도함.
- <b>포트:소켓끼리 연결하는 채널</b>
- 서버 소켓은 다른 소켓을 만들어 클라이언트 소켓과 연결시켜줌.

### 소켓 생성
```java
Socket socket = new Socket(host,port)
// 소켓 객체 생성
```
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
    Socket socket = new Socket(host,port);
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
    Socket http = new Socket(host,port);
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

## 서버 소켓
> 클라이언트로부터 접속 요청을 기다리고 접속 요구가 있으면 클라이언트와 통신할 서버측의 소켓을 생성해 넘겨줌. 그 후 다시 클라이언트의 접속을 기다림.

- 서버 소켓은 원격지의 호스트를 지정하지 않음.
- 일반 소켓과 클라이언트는 서로 스트림 작업을 함.
- 서버 소켓은 서버에서 실행되고 클라이언트 소켓을 생성할 때 socket 생성자의 port가 서버 소켓이 접속 요청을 기다리는 port.
- 자바의 서버 소켓 기능은 <b>ServerSocket</b>클래스가 담당.

### ServerSocket 클래스
> 클라이언트의 Socket 클래스로부터 생성된 소켓 객체가 실제 연결할 대상인 서버(host)쪽의 소켓에 대한 생성 및 운용을 담당하는 클래스

- 서버 소켓 객체 생성
- accept() 메소드 지원
    클라이언트의 접속 요청을 기다리고 승인하며 클라이언트의 소켓과 통신할 서버 측의 소켓을 생성하는 일을 담당함. (접속이 되면 Socket 객체를 반환)

#### ServerSocket 연결 과정
클라이언트에서 접속을 시도하면 하나 하나의 연결 시도는 대기 큐(Queue)에 저장이 됨. 이후 서버 소켓에서 accept() 메소드 호출 시, 가장 처음으로 들어온 연결 시도를 받아들여 소켓을 생성함.

서버가 하나의 클라이언트 접속을 미처 처리하기도 전에 클라이언트가 접속하는 경우 
<b>backlog</b> 크기가 너무 작으면 일부 클라이언트의 접속 시도는 거부될 수 있음.

##### backlog 옵션
> 대기 큐의 크기  
서버 소켓에 대한 연결 요구를 몇 개 까지 받아들이는가?

```java
Socket socket = null;
ServerSocket theServer = new ServerSocket(port); // 해당 port에 바인드된 서버 소켓 객체 생성
socket = theServer.accept(); // 접속이 되면 Socket 객체를 반환함.

// 클라이언트의 통신 수행

theServer.close(); // 서버소켓을 닫음.
```


