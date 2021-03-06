# 배열

> [컴퓨터 과학](https://ko.wikipedia.org/wiki/%EC%BB%B4%ED%93%A8%ED%84%B0_%EA%B3%BC%ED%95%99)에서 **배열**([영어](https://ko.wikipedia.org/wiki/%EC%98%81%EC%96%B4): array)은 번호(인덱스)와 번호에 대응하는 데이터들로 이루어진 [자료 구조](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%A3%8C_%EA%B5%AC%EC%A1%B0)를 나타낸다  

연관되어있는 값의 묶음.

## 배열 사용하기

java 기준.

### 배열 생성

``` java
int[] arr1 = new int[4]; 
String[] arr2 = new String[4];

int[] arr = {1,2,3,4}; // 배열을 생성할 때 값 초기화하기
int[] arr = new int[]{1,2,3,4};
```

첫번 째, 두번 째 줄처럼 값을 정의하지 않고 생성할 때 int형은 default 값으로 <b>0</b>이 들어가고, 그 외 자료형은 <b>null</b>이 들어감.

### 생성한 배열 사용

```java
arr[0] = 10; // 값 정의
arr[1] = 20;
arr[2] = 30; 

System.our.println(arr.length); // 배열의 길이
```



## 특징

### 배열의 단점

- 크기가 정해져있다.

  크기가 정해져있어 동적으로 배열을 사용할 수 없음.(사용하면서 크기를 더 늘리고 싶어도 안됨.)

- 기능이 없다.

  요소의 위치를 옮기거나 요소를 추가하거나 할 수 없음.

### 배열의 장점

- 크기가 정해져있다.

  작고 가벼움. 

  배열의 각 요소들은 연속적인 메모리 공간에 할당되어있기 때문에 각 요소를 찾기 쉽고 빠름.)

- 기능이 없다.

  단순함.



파이썬의 리스트와 배열은 다름. 파이썬의 리스트는 배열이 아닌 array list와 가까움.





