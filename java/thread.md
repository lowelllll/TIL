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


## 스레드의 종료와 대기
스레드 객체의 start() 메소드를 호출해 스레드가 시작되면 run() 메소드가 호출되고 run() 메소드가 실행되는 동안 start() 메소드는 계속 살아있음.

### 종료/대기 메소드
- stop()
    > 스레드의 수행을 강제로 정지하도록 함.  
    - 지금은 deparecated됨.(사용하지 않음)  
        [스레드가 사용 중인 자원들이 불안전한 상태로 남겨지기 때문.](http://ict-nroo.tistory.com/22)
    - `t.stop();`
- join() 
    > 스레드가 종료되기를 기다림. 0이면 영원히 기다리라는 말.
    - `t.join();`

## 스레드의 상태
스레드가 가질 수 있는 상태
![스레드의 상태](http://cfile1.uf.tistory.com/image/2036FB454FDAADEE1DCCB7)
- new Thread 
    > new 문장을 이용해 스레드를 생성하고 실행하기 바로 직전의 상태를 나타냄.
- Runnable 
    > 새로운 스레드가 생성되어 그 스레드의 start 메소드를 호출하면 스레드는 실행 가능한 Runnable 상태가 됨.
    - CPU를 실제로 할당 받아 실행되는 상태.
    - 실행 큐에서 CPU를 할당 받아 실행되기 위해 대기하는 상태

<i>상태전환은 자바 런타임 스케줄러에 의해 좌우되고 CPU를 사용하던 스레드가 다른 스레드가 실행될 수 있도록 CPU를 양보하기 위해서 yield 메소드를 사용할 수 있음</i>

- Not Runnable
    > 현재 실행 중인 스레드의 `suspend,wait,sleep` 메소드를 호출하거나 스레드가 i/o 작업을 수행할 시 Runnable 상태에서 Not Runnable 상태가 됨.
    - `suspend`: 잠시 쉬게함 -> `resume` : 쉬고 있는 스레드 다시 시작 메소드 호출 했을 때 Runnable 상태로 변함.
    - `wait` : nodify 메소드 호출 했을 때 Runnable 상태로 변함.
    - `sleep` : 주어진 시간 경과했을 시 Runnable 상태로 변함.
    - i/o : 작업을 수행했을 경우 Runnable 상태로 변함.
- Dead 
    > 스레드가 할 일을 모두 마치면 dead 상태가 됨.

    
