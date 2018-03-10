# Ajax
> Ajax(Asynchronous Javascript and Xml),자바스크립트를 이용하여 비동기적으로 서버와 브라우저가 데이터를 주고 받는 형식.웹 브라우저와 웹 서버가 내부적으로 데이터 통신을 하게 되고 변경된 결과를 웹 페이지에 프로그래밍적으로 반영함으로써 웹 페이지의 로딩 없이 서비스를 사용할 수 있게 됨.
Xml이라고 나와있지만 요즘엔 Json을 주로 이용함.

### 원리
웹의 흐름 : 보통 웹 사이트에 들어갈 때 주소창에 브라우저에서 url을 쓰고 들어가면 브라우저가 url(서버)로 연결해주고 서버에서 받아온 데이터들을 브라우저가 받아 파싱하여 뿌려줌.  
ajax의 흐름 : url을 받고 ajax 내부에서 XMLHttpRequest 통신을 하여 url(서버)로 연결시켜주고 서버에서 데이터를 ajax가 받음. 어떻게 뿌릴지는 개발자가 설정함(ajax자체가 브라우저라고 생각하면 됨)

### 사용법
Ajax는 보통 Jquery를 이용하여 작성함.why? 간단하게 작성이 가능하기 때문.
#### Ajax 기본 항목 설정 값
- type : http 전송 method(GET,POST)
- url : 호출 url, get 방식일 경우 url 뒤 파라미터를 붙여 사용해도 됨.
- dataType : ajax를 통해 호출한 페이지의 return 형식(xml,json,html 등)\
- error : 에러가 났을 때 처리 부분
- success : 성공했을 때 처리 부분, 해당 부분에서 데이터 핸들링을 함.

#### Ajax 사용 예제
```html
<body>
    <button id="btn">버튼</button>
</body>
<script src="http://code.jquery.com/jquery-1.11.2.min.js"></script>
<script>
    $("#btn").on("click",function(){
        $.ajax({
            url:'/url/',
            dataType:'json',
            success:function(data){
               alert('data:'+data); 
            },
            error:function(){
                alert("연결 실패");
            }
        });
    });
</script>
```
## refer
http://fruitdev.tistory.com/172  
https://opentutorials.org/course/1375/6851