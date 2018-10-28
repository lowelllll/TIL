# Interface

## interface를 왜 사용할까?

### Problem

```java
public class InterfaceTest {

	public static void main(String[] args) {
		A a = new A();
		a.methodA(new B());
	}

}

class A{ // 메소드를 사용하는 입장
	public void methodA(B b) {
		b.methodB();
	}
}

class B{
	public void methodB() {
		System.out.println("methodB()");
	}
}

```

이 상황에서 A는 User(메소드를 사용하는 입장) 은 B가 구현이 되어있어야만 사용할 수 있는 구조이다.

만약 이 구조에서 B의 메소드의 선언부가 변경이 된다면 이를 사용하는 클래스 A도 변경되어야 한다.

이런 직접적인 관계의 구조는 좋지 않다! 

### Slove

클래스 A가 클래스 B를 직접 호출하지 않고 인터페이스를 매개체로 하여 클래스 A가 인터페이스를 통해 클래스 B의 메서드에 접근하도록 하면 클래스 B가 변경되도 A는 전혀 영향을 받지 않음!

```java
public class InterfaceTest {

	public static void main(String[] args) {
		A a = new A();
		a.methodA(new B());
	}

}

interface I {
	public abstract void methodB();
}

class A{ // 인터페이스 I를 매개체로 B클래스에 접근 가능함
	public void methodA(I i) {
		i.methodB();
	}
}

class B implements I{
	public void methodB() {
		System.out.println("methodB()");
	}
}

```

A는 오직 직접적인 관계에 있는 인터페이스 I의 영향만 받는다.

- 인터페이스는 실제 구현내용 (B)를 감싸고 있는 껍데기이며 클래스 A는 껍데기 안에 어떤 알맹이가 들어있는지 몰라도 된다.

## refer

자바의 정석