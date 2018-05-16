# 인터넷 주소 처리 관련 클래스
## InetAddress 클래스
> IP 주소를 표현한 클래스, 자바에선 모든 IP 주소를 InetAddress 클래스를 사용함.

- InetAddress 클래스는 객체를 생성해줄 수 있는 5개의 static 메소드를 제공함.

```java
import java.net.*;
class  InetAddressTest1
{
	public static void main(String[] args) throws UnknownHostException // InetAddress class의 static 메소드는 UnknownHostException 예외를 발생시키기 때문에 예외처리를 해줘야함.
	{
		InetAddress address = InetAddress.getLocalHost(); // 로컬호스트의 InetAddress 객체 반환
		System.out.println("로컬 컴퓨터의 이름:"+address.getHostName()); // 호스트 이름을 문자열로 반환
		System.out.println("로컬 컴퓨터의 IP 주소:"+address.getHostAddress()); // 호스트 IP 주소
		address = InetAddress.getByName("www.naver.com"); // host에 대응하는 InetAddress 객체 반환
		// 호스트를 찾을 수 없을 시 UnknownHostException 발생	
		System.out.println("www.naver.com 컴퓨터 이름과 IP 주소:"+address);
		// toString() 호스트이름/IP 주소 반환

		InetAddress sw[] = InetAddress.getAllByName("papago.naver.com"); // host에 대응하는 InetAddress 배열 반환
		for(int i=0; i<sw.length; i++){
			System.out.print(sw[i]);
		}
		
	}
}
```
## URL 클래스
### URL
- 다양한 서비스를 제공하는 수 많은 서버들로부터 필요한 정보를 획득하기 위해 이들의 위치를 표시하는 체계
- 웹상에서 서비스를 제공하는 각 서버들에 있는 파일들의 위치를 명시한 것
- 접속해야 될 서비스의 종류,서버의 위치,파일의 위치를 포함함.

### URL 클래스
> 웹 상의 주소 그 자체를 나타내는 클래스.

<b>이 클래스는 자바로 인터넷에 연결된 특정 호스트로부터 가장 간단하게 데이터를 읽을 수 있음.</b>

- 웹 상에 존재하는 자원에 접근하거나 유일한 주소를 나타내기 위해 사용됨.
- 네트워크 연결까지 가능함.
- URL 객체 생성 시 잘못된 URL 형태를 주면 MalformedURLException 예외가 발생하기 때문에 예외처리를 해줘야함.

### URL 클래스로 페이지를 다운 받아 출력하기 위한 절차
1. URL 객체 생성  
`URL url = new URL("http://www.google.co.kr");`
2. URL 객체로의 스트림 열기  
`InputStream in = url.openStream();`
3. 받은 스트림을 버퍼에 저장  
`BufferedReader br = new BufferedReader(new InputStreamReader(in));`
4. 버퍼를 화면에 출력하고 스트림 닫기.

## URLEncoder 클래스
> 운영체제마다 일부 문자를 인식하는 방식이 다르기 때문에 이 문제를 해결하기 위해 사용하는 클래스

웹 브라우저는 encoding된 문자열을 이해하기 힘들어함.  
URLEncoder 클래스는 데이터를 웹 서버에서 요구하는 데이터 형식으로 바꾸어줌.  
인간언어 -> 기계언어

<b>웹 서버가 요구하는 형식 : `x-www-form-urlencoded` 형식의 MIME 형식</b>

문자열을 검사하여 각 문자를 차례대로 변환함.
### 변환 규칙
1. 아스키 문자, '.' , '-',''','_' 등은 그대로 전달됨.
2. 공백문자는 '+' 기호로 바뀌어 전달됨.
3. 기타 문자는 %XY와 같이 세개의 문자로 변환되며 이때 XY는 해당 문자의 아스키코드를 16진수화한 결과를 두 자리의 대문자로 나타냄.

<b>유니코드 문자의 경우 16진수 인코딩이 일어나기 전에 해당 플랫폼에서 사용하는 문자가 바이트 인코딩을 사용하여 문자 스트림의 도움을 받아 바이트로 바뀜.</b>

## URLDecoder 클래스
> `x-www-form-urlencoded` 형식의 MIME 형식을 일반 문자열로 변환하는 기능.

기계언어 -> 인간언어

- URLEncoder로 암호화된 것을 다시 일반 텍스트 형태로 변환함.
- 변환 규칙은 URLEncoder의 역순으로 텍스트화함.

<b>바이트스트림을 통해 오는 데이터는 플랫폼에서 기본적으로 사용하는 바이트가 문자 인코딩을 통해 String으로 바뀜.</b>

### URLEncoder,URLDecoder 예제
```java
import java.net.*;
import java.io.*; // UnsupportedEncodingException 예외클래스를 사용해야하기 때문에.

public class URLEnDecoder
{
	public static void main(String[] args) throws UnsupportedEncodingException // encode(), decode() 메소드를 사용하기 위해선 이 예외처리를 해줘야함.
	{
		String strData = URLEncoder.encode("인터넷 소프트웨어학과 = www.naver.com","utf-8");
		String strDeData = URLDecoder.decode(strData,"utf-8");
		System.out.println("Encoding된 문자열 :"+strData);
		System.out.println("Decoding된 문자열 :"+strDeData);
	}
}

```

## URLConnection 클래스
> 추상클래스로서 URl을 이용하여 참조된 자원에 대해 읽고 쓰는 작업을 할 수 있도록 해줌.

- URL 클래스와 유사한 기능을 가짐. URL클래스 -> 단순 접속
- application과 url간의 통신 링크를 위한 작업을 수행.
- URL에 데이터 쓰기가 가능하게 해줌. ex) 클라이언트가 서버에게 보내는 query,http 폼 데이터를 보내는 것

### URLConnection 클래스 객체 생성과 URL 연결 과정
1. url에 대한 openConnection  메소드를 호출해 URLConnection 클래스 객체 생성  
`URL url = new URL("http://www.google.com");`  
`URLConnection conn = url.openConnection();`
2. connect 메소드에 의해 실제 연결 이루어짐.
3. 셋업 파라미터와 일반 요청 속성
4. 연결이 되면 객체는 이용 가능, 객체의 헤더 항목과 내용을 읽을 수 있음.