# Thread 
> 하나의 프로그램을 프로세스라고 볼 때 , 스레드는 하나의 프로그램 내에서 실행 단위, 각 작업(task)를 스레드로 표현함.

이러한 스레드를 여러 개 둘 수 있도록 함으로써 멀티 태스킹(여러 작업)을 가능하게 해줌.

- 자바 가상머신은 하나의 애플리케이션이 동시에 수행되는 여러 개의 스레드를 가질 수 있도록 함.
- 모든 스레드는 우선순위를 가짐.
- 자바 가상머신이 시작할 때는 데몬 스레드가 아닌 단 하나의 스레드가 존재하며 이 스레드는 일반적으로 실행하려는 애플리케이션의 main() 메소드를 호출하도록 되어있음.

### 스레드의 수행
자바 가상머신이 시작할 때 부터 존재하는 스레드는 다음과 같은 경우가 발생할 때 까지 계속적으로 수행됨.
- Runtime 클래스의 exit() 메소드가 호출되고 보안 관리자(security manager)가 exit 동작이 수행되도록 허락할 때.
- 스레드의 run() 메소드의 수행이 끝나고 반환되거나 stop() 메소드가 수행되는 등 데몬 스레드가 아닌 모든 스레드가 종료되었을 때.

자바는 스레드를 구현할 수 있도록
1. Thread 클래스를 상속해 확장하는 방법.
2. Runnable 인터페이스를 구현하는 방법.
등 을 제공함.

## Thread 클래스를 사용한 스레드
> Thread 클래스를 상속하는 하위 클래스 선언, 하위 클래스가 Thread 클래스의 run() 메소드를 재정의하도록 함.

- run() 메소드 : 스레드가 수행할 작업을 나타내는 스레드의 몸체 역할
- run() 메소드의 수행이 끝나고 리턴하게 되면 스레드는 종료함. 
     run() 메소드 내에는 주로 while 문을 사용해 스레드가 반복 작업을 수행하고 
     항상 살아있도록 함.

```java
import java.lang.Thread;
class ThreadNAme extends Thread{ //Thread 상속
    public void run() { // run 메소드 오버라이딩
        // Thread body
    }
}

ThreadName t = new ThreadName();
// Thread t = new ThreadName();
t.start();
```

## Runnable 인터페이스로 구현한 스레드
> Runnable 인터페이스를 구현하는 클래스 선언. Runnable 메소드를 구현하는 클래스를 선언하고 클래스의 인스턴스 생성 후 인스턴스를 Thread를 생성할 때 매개변수로 넘겨줌.

```java
import java.lang.Runnable;
class RunnableThreadName implements Runnable{
    public void run(){
     // Thread body   
    }
}

Thread t = new Thread(new RunnableThreadName);
t.start()
```

- Runnable 인터페이스는 클래스에 의해 구현되어져야 하고 이 클래스의 인스턴스는 스레드에 의해 수행되도록 되어있음.
- Thread 클래스의 하위 클래스를 만들지 않고서도 스레드로 수행 될 클래스를 만들 수 있는 방법을 제공함.
- run() 메소드만 구현하여 사용하고 Thread 클래스의 나머지 메소드는 사용할 필요가 없을 때 사용됨.

