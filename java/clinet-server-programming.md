# Client-Server 통신 프로그램
### 순서
1. 서버가 소켓을 생성하고 연결을 기다림.
2. 클라이언트에서 서버의 소켓을 연결.
3. 클라이언트에서 데이터를 전송함. (클라이언트와 서버와의 통신 내용)
4. 서버에서 데이터를 받아 화면에 출력.(클라이언트와 서버와의 통신 내용)
5. 서버와 클라이언트의 소켓 연결을 끊음. 

## Server 측 
1. 통신을 하기 위해선 서버에서 소켓을 준비한다.
```java
ServerSocket server = new ServerSocket(port);
```
2. 서버 쪽에는 반드시 두 개의 소켓이 필요함.  
한개는 서버용 소켓으로 통신을 할 수 없고 클라이언트의 연결을 기다림.(serversocket)    
실제 통신을 하기 위해선 소켓을 하나 만들어 연결을 해줘야함.  
```java
Socket socket = server.accpet();
```

3. 입력을 위한 스트림을 선언함. 만들어 놓은 소켓에서 스트림을 얻어옴.
```java
InputStream is = socket.getInputStream();
```

4. 이 스트림에서 입력을 받기 위해선 이 스트림을 읽을 수 있는 reader를 정의해야 읽을 수 있음.
```java
InputStreamReader isr = new InputStreamReader(is);
```

5. 데이터를 읽기 위해선 InputStreamReader에 있는 read() 메소드를 사용함.
```java
int a = isr.read()
// a에 한글자가 입력됨.
```

## Client 측
1. 앞에서 생성한 서버의 IP와 port 번호로 소켓을 연결.
```java
Socket socket = new Socket(ip,port);
```

2. 출력을 위한 스트림 생성
```java
OutputStream os = socket.getOutputStream();
OutputStreamwriter writer = new OutputStreamWriter(os);
```

3. 데이터 전송
```java
writer.write(message);
```
