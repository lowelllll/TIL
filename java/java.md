# JAVA
> JAVA는 썬 마이크로시스템즈의 제임스 고슬링(James Gosling)과 다른 연구원들이 개발한 객체 지향적 프로그래밍 언어이다. 처음에는 가전제품 내에 탑재해 동작하는 프로그램을 위해 개발했지만 현재 웹 애플리케이션 개발에 가장 많이 사용하는 언어 가운데 하나이고, 모바일 기기용 소프트웨어 개발에도 널리 사용하고 있다. 

## 객체지향
- 클래스를 정의할 수 있고 객체를 생성할 수 있어야 함.
- 상태와 행동을 소프트웨어 객체의 변수와 메소드로 모델링함.

<b>변수</b> 실세계의 객체가 갖는 특성이나 상태.  
<b>메소드</b> 특성이나 상태를 변경시키는 행동을 표현하기 위해 변수의 값을 변경하거나 다른 객체로부터 온 요청에 대한 서비스를 수행하는 것 (객체의 행동)

### 객체지향의 특징
1. 캡슐화
    > 상태 정보를 저장하고 있는 변수와 상태를 변경하거나 서비스를 수행하는 메소드를 하나의 소프트웨어 묶음으로 캡슐처럼 만드는 것
    - 높은 모듈성과 정보 은닉을 제공.
        - 모듈성 : 하나의 객체를 위한 소스 코드가 다른 객체를 위한 소스 코드와 무관하게 유지될 수 있음.
        - 정보 은닉 : 접근 권한을 이용해 중요한 정보, 불필요한 접근이 필요하지 않은 정보에 대해 다른 객체에게 공개하지 않아도 됨.

2. 메세지
    > 객체는 메시지를 다른 객체에 보냄으로써 다른 객체와 통신을 함.
    - 객체 A가 객체 B에 메세지를 보내 객체 B의 메소드를 수행함.
3. 클래스
    > 실세계에 존재하는 객체들이 가질 수 있는 상태와 행동들에 대해 소프트웨어적으로 추상화 해 놓은 것.
    - 벽돌을 찍기 위한 하나의 틀
    - 재사용성 ↑
4. 인스턴스
    > 클래스를 실제로 사용할 수 있도록 선언하는 것
    - 클래스에 대한 변수를 생성
    - 메모리 공간을 차지함
5. 객체
    - 클래스 : 객체의 상태와 행동을 정의하고 있는 프로토타입, 설계도
    - 인스턴스 : 클래스를 실제로 사용할 수 있도록 변수 선언 한 것
    - <b>인스턴스를 객체라고 할 수 있음.</b>
6. 상속
    > 하위 클래스가 상위 클래스를 상속해 상위 클래스가 가지고 있는 모든 특성들을 상속하여 사용할 수 있음.
7. 다형성
    > 여러 개의 클래스가 같은 메세지에 대해 각자의 방법으로 작용할 수 있는 능력
    - 같은 이름을 갖는 여러가지 형태가 존재함.
    - 다중 정의(overloading),메소드 재정의 (overriding)

## 클래스 정의 및 객체 생성
### 클래스 정의
```java
// ex)
[접근 지정자] class ClassName [상속 클래스] [인터페이스]{
    //변수
    //생성자
    //메소드
    public static void main(String args[]){
        // 메인 함수
    }
}
```
### 객체 생성
```java
Point p1 = new Point() , p2;
p2 = new Point();
```

## 변수 및 메소드의 접근 제어
접근 권한을 지정하기 위한 <b>접근 지정자</b>
### Public 
- 같은 클래스 , 하위 클래스 , 같은 패키지 내에 있는 어떤 클래스에서도 접근 가능
- 클래스 또는 객체의 외부 인터페이스를 제공해줄 때 사용.
### Private 
- 같은 클래스 내에서만 접근 가능.
- 자신의 클래스 내에 있는 메소드에서만 참조하거나 사용 가능. 
- 정보 은닉
### Protected
- 같은 클래스 , 하위 클래스, 같은 패키지 내의 모든 클래스 접근 가능.
### 생략
- 같은 클래스 , 같은 패키지 내에 있는 모든 클래스 내에서 접근 가능
- 하위 클래스 X

## 메소드 다중정의 (Overloading)
> 같은 이름의 여러개의 메소드를 메소드가 갖는 매개변수의 개수와 자료형을 이용하여 구분함.
- 메소드를 호출할 때 호출되는 메소드는 실 매개변수의 자료형과 가장 가까운 형식의 매개변수를 갖는 메소드가 호출됨.

## 객체 생성
### 객체 생성자
- new 연산자를 이용하여 객체를 생성할 때 호출됨.
- 객체가 생성될 때 자동으로 호출 되며 객체가 가지는 변수에 대한 초기화 작업, 메모리 할당 등의 작업을 함.
- 오버로딩 가능
- 객체 생성자가 여러개 정의 되어있을 때 하나의 객체 생성자가 다른 객체 생성자를 호출할 수 있는데 이때 this 키워드를 사용함.
- 상속 관계에 있는 하위 클래스에서 상위 클래스의 객체 생성자를 호출할 때 super 키워드를 사용하며 이러한 다른 객체 생성자의 호출은 반드시 첫 번째 줄에 나타나야함.
```java
class Point{
    Point(){
        // 생성자 
        // 객체를 생성할 때 호출됨.
    }
}
```
### 객체 생성 과정
new 연산자를 이용하여 객체를 생성할 때
1. 객체를 위한 메모리 공간을 할당해 이를 default 값으로 초기화
2. 객체 생성자의 첫 번째 문장이 다른 생성자를 호출 할 경우 먼저 처리함.
3. 객체가 갖는 인스턴스 변수의 초기화 수식,초기화 블록, 생성자의 몸체를 순서대로 수행.

### This
- 객체 자신에 대한 참조 값을 가짐.
- 메소드 내에서만 사용됨.
- 매개변수와 객체 자신이 가지고 있는 변수의 이름이 같을 경우 이를 구분하기 위해 자신이 가지고 있는 변수 앞에 this를 사용함.
    ```java
    class Point{
        private x,y;
        Point(int x,int y){
            this.x = x;
            this.y = y;
        }
    }
    ```
- 객체 생성자 내에서 다른 생성자를 호출하기 위해 사용함.
    ```java
    class Point{
        Point(){
            this(0,0);
        }
        Point(int x,int y){
            // ... 
        }
    }
    ```
- this를 사용함으로써 모호하지않고 조금 더 <b>명확한</b> 프로그램 작성할 수 있음

### 클래스 멤버, 인스턴스 멤버
클래스 내에는 변수와 메소드가 있고 변수와 메소드에는 <b>인스턴스 변수,인스턴스 메소드</b>와 <b>클래스 변수,클래스 메소드</b>가 있음.

#### 인스턴스 변수와 메소드
> 인스턴스가 따로 갖는 변수,메소드
일반적으로 클래스 내에 선언된 변수와 메소드는 대부분 인스턴스 변수,인스턴스 메소드임.  
- 클래스에 대해 인스턴스를 생성할 때 각 인스턴스가 독립적으로 이를 위한 메모리 공간을 확보하기 때문.

#### 클래스 변수와 메소드(static 변수,static 메소드)
> 클래스의 모든 객체(인스턴스)가 공유하는 전역 변수와 전역 메소드. 해당 클래스에 대한 모든 인스턴스들이 공유함.
- 객체의 생성과 관련이 없고 클래스가 메모리에 로드 될 때 한번 클래스 변수를 위한 메모리가 할당되고 초기화 됨. 
- 객체를 생성하지 않아도 클래스 변수에 대한 접근이 가능
- 클래스  메소드 내에서는 해당 객체에 대한 참조값(this,super)을 갖지 않으므로 인스턴스 변수 또는 인스턴스 메소드를 사용할 때 해당 객체 참조값을 명시해 줘야함.

```java
// 클래스 변수와 클래스 메소드의 선언
[접근 권한] static 변수 선언;
[접근 권한] static 메소드 선언;

// 클래스 변수와 클래스 메소드의 접근 
클래스 이름.클래스 메소드();
클래스메소드();
객체참조값.클래스 메소드();

// ex)
class Point(){
    private int pointId; // 인스턴스 변수
    public static int getPointID(){ // 클래스 메소드
        return pointId; //오류 발생. Why? 클래스 메소드에서 인스턴스 변수 (pointId)에 접근을 시도했기 때문에 
    }
}
```

### 클래스 초기화 및 객체의 생성 과정
자바는 자바 가상머신에 의해 해당 클래스가 메모리에 적재될 때 <b>클래스 초기화</b>를 먼저 수행함.
#### 클래스 초기화 과정
1. 모든 클래스 변수는 자바에서 제공해 주는 세 가지 메모리 영역 중 메소드 영역에 위치함.
2. 각 클래스 변수는 0,'\u0000',false,null 등 디폴트 초기값으로 초기화됨.
3. 클래스 변수 초기화 수식과 클래스 초기화 블록을 정의된 순서대로 실행함.

클래스 변수의 초기화 수식만으로 클래스 변수 초기화 할 수 없을 때, 클래스 초기화 블록을 사용함.{}
- 배열 형의 클래스 변수 초기화
- 여러 개의 클래스 변수를 한꺼번에 초기화
- 예외를 발생하는 메소드를 호출할 때

#### new 연산자에 의해 객체가 생성될 때의 수행 과정
1. new 연산자는 객체를 위한 메모리 공간을 할당.
2. 객체가 가지고 있는 모든 인스턴스변수들을 디폴트 초기값으로 초기화.
3. 객체가 갖고 있는 인스턴스 변수의 초기화 수식 및 초기화 블록 실행.
4. 객체 생성자의 첫 번째 문장에서 다른 생성자를 호출할 경우 이를 먼저 처리함.
5. 객체 생성자의 몸체 실행.

[클래스 초기화 객체 생성 과정 코드](https://github.com/lowelllll/study-java/blob/master/Java/src/ClassInitTest.java)

## 상속
> 부모 클래스에 정의된 멤버를 자식 클래스가 물려 받는 것.

 A 클래스를 확장(상속)하여 B 클래스를 만들 때 A 클래스는 상위 클래스, B 클래스는 하위 클래스라고 함.  
 
<i>하위 클래스는 상위 클래스를 상속한다.</i>  
- 자바 클래스 계층 구조의 최상위 클래스 : java.lang 의 Object 클래스
- private 접근 지정자를 제외한 접근 지정자로 선언된 변수와 메소드는 상속 할 수 있음.
- 상위 클래스와 하위 클래스에 같은 이름의 변수나 메소드가 있는 경우 하위 클래스에서 상속할 수 없음.(메소드 이름이 같은 경우 재 정의 한다고 함.override)
- 단 하나의 클래스만 상속 가능.(다중상속X)
- 상위 자료형 변수는 하위 자료형 값을 가리킬 수 있지만 하위 자료형 변수는 상위 자료형을 가리킬 수 없음.
### 상속과 생성자
상위 클래스가 가지고 있는 객체 생성자는 하위 클래스가 상속할 수 없고 하위 클래스의 객체 생성자에서 상위 클래스의 객체 생성자를 호출할 수 있음.  
<b>super()</b>키워드 사용.  
```java
class B extends A{
    B(){
        super();// A 생성자 호출
        System.out.println("This is B");
    }
}
```

하위 클래스의 객체 생성자에서 상위 클래스의 객체 생성자를 호출하는 문장을 명시적으로 나타내주지 않음녀 자바 컴파일러가 자동으로 super() 문장을 삽입해줌.


### 상속과 다른 객체 생성자를 호출하는 객체 생성 순서
1. new 생성자로 객체를 위한 메모리 공간 할당
2. 모든 인스턴스 변수 디폴트 초기값으로 초기화
3. 상위 클래스의 생성자 호출
4. 하위 클래스의 선언된 인스턴스 변수 초기자 수식 및 인스턴스 초기화 블록 실행
5. 하위 클래스의 생성자 몸체 실행.


### Overriding (메소드 재정의)
> <i>상속 관계에서 나타나는 다형성의 특징. 하위 클래스는 상위 클래스에서 정의된 메소드와 같은 이름,같은 인자들을 갖는 새로운 메소드를 정의하여 상위 클래스에서 상속되는 메소드를 재정의함</i>]

- 소스 코드 및 외부 인터페이스 재사용할 수 있도록 해줌.
- 응용 프로그램의 개발을 돕기 위한 뼈대 구조를 제공하는 강력한 클래스 라이브러리를 제공할 수 있음.

#### Overriding 규약
- 인스턴스 메소드이여야함.
- 메소드 이름이 같아야함.
- 매개 변수의 개수와 자료형,리턴형이 일치해야함.

### 상속과 변수 및 메소드의 접근 제어
상위 클래스에 있는 변수를 참조하거나 또는 메소드를 호출하기 위해선 <b>super</b> 키워드를 사용함.

- super 키워드를 통해 재정의된 메소드를 호출할 경우 현재의 클래스에 재정의된 메소드를 호출하지 않고 상위 클래스의 메소드를 호출함.
```java
Class A{
    int x,y,z;
    ...
}

class B extends A{
    int x; //재정의
    float y; //재정의 
}
```
- 하위 클래스가 상속할 수 없는 클래스 멤버 : private 접근 지정자로 선언된 변수,메소드 

## 추상클래스
> 구현이 덜 되었거나 미완성 클래스 이므로 실제 인스턴스를 생성할 수 없도록 한 클래스

- 추상 메소드가 하나라도 있는 클래스.
- 객체 생성 불가.
- 추상 클래스를 상위 클래스로 하여 상속하는 하위 클래스는 상위 클래스에서 완전히 구현하지 못한 부분을 완전하게 구현해줘야 하위 클래스에 대한 객체를 생성할 수 있음.(모든 추상 메소드를 구현해야함)

### 추상 메소드
> 아직 구현이 이루어지지 않고 단지 프로토타입만을 가지고 있는 메소드.

#### 특징
- 미완성 메소드, 메소드의 몸체를 가질 수 없음.
- 클래스가 가져야 할 인터페이스에 대한 프로토타입(메소드의 형태)을 정의하고 있음.
- 하위 클래스가 가져야 할 인터페이스 정의.
- 추상 메소드를 포함하는 클래스는 반드시 추상 클래스로 정의해야함.

```java
abstract class AbstractClass { // 추상 클래스
    abstract void abstractMethod(); // 추상 메소드. body가 없음 
}
```

## 상수 (final 변수)
> final 변수 : 값을 변경할 수 없는 상수.  
변수를 final 지정자로 선언하면 그 변수의 내용은 변경할 수 없음.(상수)

### final 변수의 장점
- 가독성을 높이고 수정이 용이함.

### 공백 상수
> final 변수에 아무런 초기값도 대입하지 않은 경우.  
초기화 수식만으로 초기값을 계산할 수 없거나 초기값을 계산할 때 예외가 발생할 수 있는 경우 사용.

### 상수 객체
> final 변수와 같이 내용을 변경할 수 없는 객체  
객체의 내용을 외부에서 전혀 바꿀수 없는 객체.

final 변수가 참조형일 경우 final 변수가 직접 저장하고 있는 참조값은 바꿀 수 없지만 참조하고 있는 객체 또는 배열의 내용은 바꿀 수 있음.

```java
final 변수이름 = 초기값;
```

### final 클래스, final 메소드
final 클래스와 final 메소드로 상속과 오버라이딩이 불가능하게 함.

#### final 클래스
> 더 이상 상속될 수 없는 클래스
#### final 메소드
> 상속 관계, 상위 클래스와 하위 클래스에 대해 상위 클래스에 있는 메소드가 더이상 하위 클래스에 의해 재정의 되지 않도록 구현하는 것. (Override 불가능)

```java
final class ClassName{ // 상속 불가능한 클래스
    final method(){}; // override 불가능한 클래스
}
```

## 인터페이스
> 추상 메소드와 final 변수로만 이루어진 클래스.

메소드 정의(prototype,추상메소드)만 가지고 있고 이와 관련된 상수 값 만을 모아놓은 클래스.

- 인터페이스 내에 정의된 메소드는 자바에 의해 자동으로 <b>public abstract</b>로 선언됨.
- 인터페이스 내에 정의된 변수는 자동으로 <b>public static final</b>로 선언됨.
    -> abstract 지정자를 사용하지 않음.
- 하나의 인터페이스는 여러개의 상위 인터페이스를 가질 수 있음.(다중 인터페이스 상속)
- 하나의 클래스는 여러개의 인터페이스를 구현할 수 있음.
- 인터페이스는 추상 클래스와 같이 구현이 이루어지지 않았으므로 클래스에 의해 상속되어 구현이 완성되야 객체를 생성할 수 있음.
- 클래스와 클래스 사이에는 <b>상속</b>이 이루어지고 클래스와 인터페이스 간에는 <b>구현</b>이 이루어짐.

```java
[public] interface InterfaceName [extends 상위 인터페이스,상위 인터페이스2,...]{
    상수 선언;// final 상수
    메소드 선언; // 추상 메소드
}
```

### 인터페이스를 사용하는 경우
- 강제적인 클래스 관계를 만들지 않으면서 서로 관련없는 클래스들  사이의 유사성을 타나내야하는 경우.
- 하나 이상의 클래스들이 구현하기를 원하는 메소드를 선언할 경우.
- 클래스를 보여주지 않고서 객체의 프로그래밍 인터페이스를 보여주어야 하는 경우.

> <i>ex) 인터페이스를 사용하면 다른 구성원들이 각각의 부분을 완성할때 까지 기다리지 않고 서로간의 규약만 정해두어 내 부분만 따로 나눠서 작성된 코드를 컴파일 할 수 있습니다.</i>

### 인터페이스의 접근 지정자
<b>인터페이스의 멤버는 무조건 public으로 선언해야한다.</b>

- public interface : 모든 패키지 내에 있는 클래스에서 접근 가능.
- 생략 interface : 같은 패키지 내에 있는 클래스에서 접근 가능.  
나머지 접근 지정자는 X

## 선언 지역에 따른 클래스 구분

### 중첩 클래스
> 클래스 내부에 static으로 선언된 클래스

클래스를 따로 선언하고 정의하기에는 로직이 간단하고 특정 클래스와 밀접한 관계를 갖는 경우 , 클래스로 정의해야하지만 다른 클래스에서는 사용되지 않고 특정 클래스하나에서만 사용하는 경우 사용됨.

- 중첩 클래스의 객체는 중첩 크래스를 포함하고 있는 클래스의 이름을 패키지 이름처럼 사용할 수 있음. ex) OuterClass.NestedClass 
- 중첩 클래스와 중첩 클래스를 포함하고 있는 클래슨느 컴파일 했을 때 각각에 대한 클래스 파일이 생성됨. 중첩클래스 `A&B.class` 중첩클래스를 포함하는 클래스 `A.class`
```java
Class A{
    static class B{ // 중첩 클래스

    }
}
```

### 내부 클래스
> 중첩 클래스와 달리 static 없이 내부에 선언된 클래스

이벤트 처리,데이터 구조 클래스 등을 위해 자주 사용됨.

- 내부 클래스 객체는 생성 당시에 외부 클래스 객체에 대한 보이지 않는 참조값을 갖도록 초기화됨.
- 외부 클래스 객체의 메소드와 필드가 내부 클래스 객체의 메소드와 필드인 것처럼 접근 가능.
<b>내부 클래스 메소드에서 외부 클래스 객체의 참조값을 얻는 방법</b>`외부 클래스 이름.this`
- 내부 클래스는 자신을 포함하는 클래스의 클래스 변수, 클래스 메소드와 인스턴스 변수, 인스턴스 메소드에 접근 가능. But, 내부 클래스는 <b>클래스 변수나 클래스 메소드를 가질 수 없음.</b>

```java

```

### 지역 클래스
> 지역 변수와 같이 메소드 내에 정의된 클래스

지역 변수와 같이 메소드 내에서만 사용하고자 할 경우 사용됨.  
지역 변수는 메소드가 끝난 후에는 사라지므로 지역 변수를 final로 선언함으로써 항상 메모리에 존재하도록 해줌.

- 지역 클래스를 포함하고 있는 메소드가 반환되고 난 후에도 지역 클래스 객체는 계속 존재하고 계속 자신을 포함하고 있는 메소드의 지역변수에 접근함.
- 지역 클래스를 포함하는 메소드의 지역 변수 중 지역 클래스에서 참조하는 지역변수는 항상 final로 선언함.

```java
class ClassName{
    method(){
        final 지역 변수;
        class LocalClass{
            // 지역 클래스
        }
    }
}
```

### 익명 클래스
> 클래스 또는 인터페이스에 대한 객체를 생성하면서 바로 클래스 또는 인터페이스를 정의하도록 한 클래스.

클래스 또는 인터페이스를 구현하여 바로 사용하고자 할 때 사용됨.

- new 수식이 있는 곳에서 클래스 or 인터페이스를 정의함.
- 단순한 클래스 or 인터페이스
- 단 한번만 정의하여 사용하는 경우
- 익명 클래스를 포함하고 있는 메소드의 지역 변수 중 익명 클래스에서 참조할 수 있는 지역 변수는 final로 선언된 지역 변수만 가능함.

```java
new ClassName{
    //class 정의
}; // 세미콜론 꼭 해줘야함

new InterfaceName{
 // 인터페이스 정의
}
```

## refer
http://blog.eairship.kr/122

