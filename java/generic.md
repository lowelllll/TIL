# Generic
> 클래스 내부에서 사용할 데이터 타입을 나중에 인스턴스를 생성할 때 확정하는 것

- 다양한 타입의 객체를 다루는 메서드나 컬렉션 클래스에 컴파일 시 타입체크를 해주는 기능
- 객체의 타입을 컴파일 시 체크하기 때문에 객체의 타입 안정성을 높이고 형변환의 번거러움이 줄어듬.

### 예제 
```java
class Person<T>{
    public T info; 
    
    // p1 일시 데이터 타입 String
    // p2 일시 데이터 타입 StringBuilder
}

public class GenericDemo {
    public static void main(String[] args){
        Person<String> p1 = new Person<Stirng>();
        Person<StringBuilder> p2 = new Person<StringBuilder>();
    }
}
```

### 제네릭의 장점
1. 타입의 안정성을 제공함.
     의도하지 않은 타입이 객체를 저장하는 것을 막고, 저장된 객체를 꺼내올 때 원래의 타입과 다른 타입으로 형변환되어 발생할 수 있는 오류를 줄여준다.
2. 타입체크와 형변환을 생략할 수 있으므로 코드가 간결해짐.


## refer
http://devbox.tistory.com/entry/Java-%EC%A0%9C%EB%84%A4%EB%A6%AD