# java 파일과 class 파일 구분하기

## 상황
java 프로그램을 열심히 만들던 중 cmd에서 java파일을 컴파일할 때,  
java 파일과 같은 곳에 class 파일이 생성되어 java 파일과 class파일이 뒤섞인 모습이 보기 싫었다.  
때문에 나는 java파일과 class파일을 각 다른 폴더에 분리하여 관리하고 싶었다.

구글링해보니 .java 파일과 .class파일이 한 폴더에 함께 있는 것은 바람직 하지 못하다고 한다. (관리가 불편함)
## 해결
- 컴파일할 때 옵션을 줘서 어느 폴더에 저장할 지 지정해주면 된다.
- 일반적으로 소스코드는 'src', 클래스파일은 'bin' 이라는 이름의 폴더를 지정해준다.
### 컴파일
```
javac -d bin src/파일명.java
```
- 폴더 지정 옵션으로 컴파일을 한다.
- -d 옵션 : 컴파일한 클래스 파일을 어디에 둘지 폴더를 지정함.
- src 폴더 안에 있는 해당 자바 파일을 컴파일해 bin이라는 폴더에 클래스 파일을 저장하라는 뜻.

### 실행
```
java -cp bin 파일명
```
- -cp 옵션 : 클래스 파일을 읽을 때 해당 파일이 어디에 있는지 위치를 지정함.
- bin안에 있는 클래스파일을 실행하라는 뜻.

## refer
http://wanzargen.tistory.com/12