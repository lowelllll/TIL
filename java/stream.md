# Stream
> 프로그램 사이에서 데이터의 흐름.

- 데이터를 가진 쪽에서 데이터가 필요한 쪽으로 데이터를 보내는 것.
- 하드웨어 장치로부터 데이터를 읽거나 기록할 때 사용하는 중간 매개체 역할.

자바의 입출력은 스트림을 통해 이루어짐.

## 스트림 분류
### 데이터의 흐름에 따른 분류
- 입력 스트림
    > 키보드로부터의 입력과 같은 데이터의 입력에 관련된 처리를 하는 스트림.
    - `InputStream` , `Reader`
- 출력 스트림
    > 파일로의 출력과 같은 데이터의 출력에 관련된 처리를 하는 스트림.
    - `OutputStream` , `Writer`

### 스트림의 기능에 따른 분류
- 노드 스트림
    > 기본적인 기능을 담당하는 스트림.
    - `FileOutputStream` , `FileInputStream`
- 필터 스트림
    > 노드 스트림이나 다른 필터 스트림에 붙어 더 효울적이도록 기능을 확장해주는 스트림.
    - `BufferedOutputStream` , `BufferedInputStream` , `DataOutputStream` , `DataInputStream`

### 데이터의 종류에 따른 분류
- 바이트 스트림 
    > 데이터가 8비트 단위로 전송되어 입출력을 지원하는 스트림.
    - 대체로 스트림의 이름에 InputStream,OutputStream이 붙음.
    - 파일 입출력, 객체의 입출력, 기본 자료형의 입출력 등에 사용됨.

- 문자 스트림
    > 문자 전송을 위한 스트림.
    - 자바에서 문자는 16비트 유니코드를 사용하므로 문자 전송을 위해 2바이트 단위로 전송됨.
    - 대체로 스트림의 이름에 Reader,Writer가 붙음.

## 바이트 스트림
바이트 단위로 입출력을 제어함.
### 바이트 스트림 클래스
> 처리하고자 하는 데이터의 종류가 파일,그림,동영상 등의 <b>바이트</b> 기반인 경우 사용하는 클래스

- InputStream / OutputStream   
    입출력을 위한 바이트 스트림의 최상위 추상 클래스
- FileInputStream / FileOutputStream   
    파일 입출력을 위한 바이트 스트림 클래스
- DataInputStream / DataOutputStream  
    자바 기본형 데이터를 입출력하기 위한 클래스
- BufferedInputStream / BufferedOutputStream  
    입출력 스트림에 버퍼링 기능을 추가한 클래스
- printStream  
    System.out을 통해 콘솔로 출력하기 위한 클래스

## 문자 스트림
### 문자 스트림 클래스
> 처리하고자 하는 데이터가 문자인 경우 제공하는 클래스

16비트 유니코드 문자을 주고받기 위해 사용됨.

- Reader / Writer  
      입출력을 위한 문자 스트림의 최상위 추상 클래스
- FileReader / FileWriter  
    파일 입출력을 위한 문자 스트림 클래스
- BufferedReader / BufferedWriter  
    입출력 스트림에 버퍼링 기능을 추가해주는 스트림 클래스
- PrintWriter   
    출력을 위한 동작을 지원하는 문자 스트림    
- InputStreamReader / OutputStreamWriter  
    바이트와 문자 변환을 위한 입출력 스트림 클래스

