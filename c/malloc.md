# malloc 함수
> 데이터를 동적으로 할당해주는 함수.
## 사용법
### 헤더파일 
strlib.h include 
```c
#include <strlib.h>
```
### 메모리 할당
함수 호출시 할당하고자 하는 메모리의 크기를 바이트 단위로 전달하면 그 크기만큼 메모리를 할당하게 된다.  
할당 후 할당 한 메모리의 주소를 반환함.  
보통 sizeof 함수로 할당하고자 하는 메모리의 크기를 알아낸다.
#### 리턴 형
malloc는 단순히 메모리만 할당하는 함수이기 때문에 개발자가 어떠한 데이터 형을 저장하는지 예측할 수 없다.(4바이트를 할당했을 경우 int형 데이터를 저장하기 위해서인지, float형 데이터인지 알 수 없음)  

때문에 malloc에서 리턴되는 void* 을 알맞은 데이터 형으로 변환해야한다.

## 사용 예
```c
char *str;
str = (char*)malloc(sizeof(char)*sizeof("hi"));
```
### sizeof 함수
> sizeof 연산자는 피연산자 형식의 개체를 저장하기 위해 필요한 저장소 공간(바이트)을 제공합니다

쉽게 sizeof 함수 안의 type-name의 바이트 크기를 알아낸다.

```c
char *str = "hello";
printf("%d\n",sizeof(char)); // 4
printf("%d\n",sizeof(str)); // 4 출력 str 포인터 변수의 type , 즉 char type의 바이트 크기를 반환함.
printf("%d\n",sizeof("hello")); // 6  "hello" 문자열의 길이와 null 값을 포함한 길이를 반환 
```

## refer
http://dsnight.tistory.com/51