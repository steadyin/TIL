# 1. 멀티 스레드

## 1.1 프로세스와 스레드

운영체제에서 실행 중인 하나의 애플리케이션을 프로세스(Process)라고 부른다. 사용자가 애플리케이션을 실행하면 운영체제로부터 실행에 필요한 메모리를 할당받아 애플리케이션의 코드를 실행하는데 이것이 프로세스이다. 하나의 애플리케이션은 다중 프로세스를 만들기도 하는데 Chrome 브라우저를 두 개 실행했을 때가 그 예이다.

멀티 태스킹(multi tasking)은 두 가지 이상의 작업을 동시에 처리하는 것을 말하는데, 운영체제는 멀티 태스킹을 할 수 있도록 CPU 및 메모리 자원을 프로세스마다 적절히 할당해주고, 병렬로 실행시킨다. 멀티 태스킹은 꼭 멀티 프로세스를 뜻하지는 않는다. 한 프로세스 내에서 멀티 태스킹을 할 수 있도록 만들어진 애플리케이션들도 있다. 예를 들면 메신저에서 채팅 기능을 하면서 파일 전송 기능을 수행하기도 한다. 하나의 프로세스가 두 가지 이상의 작업을 어떻게 처리할 수 있을까? 그 비밀은 멀티 스레드(multi thread)에 있다.

스레드(thread)는 사전적 의미로 한 가닥의 실이라는 뜻인데 한 가지 작업을 실행하기 위해 순차적으로 실행할 코드를 실처럼 이어 놓았다고 해서 유래된 이름이다. 멀티 프로세스가 애플리케이션 단위의 멀티 태스킹이라면 멀티 스레드는 애플리케이션 내부에서의 멀티 태스킹이라고 볼 수 있다.

멀티 프로세스가 애플리케이션 단위의 멀티 태스킹 이라면 멀티 스레드는 애플리케이션 내부에서의 멀티 태스킹이라고 볼 수 있다.

## 1.2 메인 스레드

모든 자바 애플리케이션은 메인 스레드(main thread)가 main() 메소드를 실행하면서 시작된다. 메인 스레드는 main() 메소드의 첫 코드부터 아래로 순차적으로 실행하고 main() 메소드의 마지막 코드를 실행하거나 return문을 만나면 실행이 종료된다.

메인 스레드는 필요에 따라 작업 스레드들을 만들어서 병렬로 코드를 실행할 수 있다. 싱글 스레드 애플리케이션에서는 메인 스레드가 종료되면 프로세스도 종료된다. 하지만 멀티 스레드 애플리케이션에서는 실행 중인 스레드가 하나라도 있다면 프로세스는 종료되지 않는다.

# 2. 스레드 생성과 실행

어떤 자바 애플리케이션이건 메인 스레드는 반드시 존재하기 때문에 메인 작업 이외에 추가적인 병렬 작업의 수만큼 스레드를 생성하면 된다. 자바에서는 작업 스레드도 객체로 생성되기 때문에 클래스가 필요하다. 

![image](https://user-images.githubusercontent.com/79847020/140723394-2613a587-bbc9-4cab-bd7d-4115f367d061.png)

## 2.1 Thread 클래스로부터 직접 생성

java.lang.Thread 클래스로 부터 작업 스레드 객체를 직접 생성하려면 다음과 같이 Runnable을 매개값으로 갖는 생성자를 호출해야 한다. 

```JAVA
Thread thread = new Thread(runnable target);
```
Runnable은 작업 스레드가 실행할 수 있는 코드를 가지고 있는 인터페이스라고 해서 붙여진 이름이다. 구현 클래스는 run()을 재정의해서 작업 스레드가 실행할 코드를 작성해야 한다.

```JAVA
class Task implements Runnable {
    public void run() {
        스레드가 실행할 코드;
    }
}
```

다음과 같이 익명객체를 많이 사용한다.
```JAVA
Thread thread = new Thread(new Runnable() {
    public void run() {
        스레드가 실행할 코드;
    }
}
```

Java8 부터는 람다식을 매개값으로 사용할 수도 있다.
```JAVA
Thread thread = new Threade(() -> {
    //스레드가 실행할 코드;
});
```
작업 스레드는 생성되는 즉시 실행되는 것이 아니라 start() 메소드를 다음과 같이 호출해야만 비로소 실행된다.
```JAVA
thread.start()
```
```JAVA
import java.awt.*;

public class BeepTask implements Runnable {
    @Override
    public void run() {
        Toolkit toolkit = Toolkit.getDefaultToolkit();
        for(int i=0; i<5; i++) {
            toolkit.beep();
            try { Thread.sleep(500); } catch (Exception e) {}
        }
    }
}
```
```JAVA
public class BeepPrintExample2 {
    public static void main(String[] args) {
        Runnable beepTask = new BeepTask();
        Thread thread = new Thread(beepTask);
        thread.start();

        for(int i =0; i<5; i++) {
            System.out.println("땡");
            try { Thread.sleep(500); } catch(Exception e) {}
        }
    }
}
```

## 2.2 Thread 하위 클래스로부터 생성

작업 스레드가 실행할 작업을 Runnable로 만들지 않고 Thread의 하위 클래스로 작업 스레드를 정의하면서 작업 내용을 포함시킬수도 있다.

```JAVA
public class 클래스명 extends Thread {
    @Override
    public void run() {
        //스레드가 실행할 코드
    }
}
```

마찬 가지로 Thread 익명 객체로 작업 스레드 객체를 생성할 수도 있다.

```JAVA
Thread thread = new Thread() {
    public void run() {
    //스레드가 실행할 코드
    }
};
```
start() 메소드로 동작시킬 수 있다.
```JAVA
thread.start()
```

## 2.3 Thread의 이름

메인 스레드는 "main" 이라는 이름을 가지고 있고 우리가 직접 생성한 스레드는 자동적으로 "main"이라는 이름을 가지고 있고 직접 생성한 스레드는 자동적으로 "Thread-n"이라는 이름으로 설정된다.

아니면 다음과 같은 스레드의 이름을 지정할 수도 있다.
```JAVA
thread.setName("스레드 이름");
thread.getName();
```

Thread의 정적 메소드인 currentThread()로 실행하는 현재 스레드의 참조를 얻을 수 있다.

```JAVA
Thread thread = Thread.currentThread();
```

## 3. Thread 우선순위

멀티 스레드는 동시성(Concurrency) 또는 병렬성(Parallelism)으로 실행되기 때문에 이 용어들에 대해 정확히 이해하는 것이 좋다. 동시성은 멀티 작업을 위해 하나의 코어에서 멀티 스레드가 번갈아가며 실행하는 성질을 말하고 병렬성은 멀티 작업을 위해 멀티 코어에서 개별 스레드를 동시에 실행하는 성질을 말한다. 

싱글 코어 CPU를 이용한 멀티 스레드 작업은 병렬적으로 실행되는 것처럼 보이지만 사실은 번갈아가며 실행하는 동시성 작업이다. 번갈아 실행하는 것이 워낙 빠르다보니 병렬성으로 보일 뿐이다.

![image](https://user-images.githubusercontent.com/79847020/140731045-b3ddcda5-3173-4328-9542-f18b524f98c4.png)

스레드의 개수가 코어의 수보다 많은 경우, 스레드를 어떤 순서에 의해 동시성으로 실행할 것인가를 결정해야 하는데 이것을 스레드 스케줄링이라고 한다. 스레드 스케줄링에 의해 스레드들은 아주 짧은 시간에 번갈아 가면서 그들의 run() 메소드를 조금씩 실행한다. 쿼드 코어일 경우에는 4개의 스레드가 병렬성으로 실행될 수 있기 때문에 4개 이하의 스레드를 실행할 경우 우선순위 방식이 크게 영향을 미치지 못한다.

자바의 스레드 스케줄링은 우선순위(Priority) 방식과 순환 할당(Round-Robin) 방식을 사용한다. 우선순위 방식은 우선순위가 높은 스레드가 실행 상태를 더 많이 가지도록 스케줄링 하는 것을 말한다. 순환 할당 방식은 시간 할당량(Time Slice)을 정해서 하나의 스레드를 정해진 시간만큼 실행하고 다시 다른 스레드를 실행하는 방식을 말한다. 스레드 우선순위 방식은 스레드 객체에 우선 순위 번호를 부여할 수 있기 때문에 코드로 제어할 수 있다. 하지만 순환 할당 방식은 자바 가상 기계에 의해서 정해지기 때문에 코드로 제어할 수 없다.

```JAVA
thread.setPriority(우선순위);
thread.setPriority(Thread.MAX_PRIORITY);
thread.setPriority(Thread.NORM_PRIORITY);
thread.setPriority(Thread.MIN_PRIORITY);
```

```JAVA
public class PriorityExample {
    public static void main(String[] args) {
        for(int i=1; i<=10; i++) {
            Thread thread = new CalcThread("thread" + i);
            if(i!=10) {
                thread.setPriority(Thread.MIN_PRIORITY);
            } else {
                thread.setPriority(Thread.MAX_PRIORITY);
            }
            thread.start();
        }
    }
}
```
```JAVA
public class CalcThread extends Thread {
    public CalcThread(String name) {
        setName(name);
    }

    public void run() {
        for(int i=0; i<2000000000; i++) {}
        System.out.println(getName());
    }
}
```

# 4 동기화 메소드와 동기화 블록
## 4.1 공유 객체를 사용할 때의 주의할 점

싱글 스레드 프로그램에서는 한 개의 스레드가 객체를 독차지해서 사용하면 되지만 멀티 스레드 프로그램에서는 스레드들이 객체를공유해서 작업해야 하는 경우가 있다. 이 경우 스레드 A를 사용하던 객체가 스레드 B에 의해 상태가 변경 될 수 있기 때문에 스레드 A가 의도했던 것과는 다른 결과를 산출할 수도 있다. 

```JAVA
public class User1 extends Thread {
    private Calculator calculator;

    public void setCalculator(Calculator calculator) {
        this.setName("CalculatorUser1");
        this.calculator = calculator;
    }

    public void run() {
        calculator.setMemory(100);
    }
}
```
```JAVA
public class User2 extends Thread {
    private Calculator calculator;

    public void setCalculator(Calculator calculator) {
        this.setName("CalculatorUser2");
        this.calculator = calculator;
    }

    public void run() {
        calculator.setMemory(50);
    }
}
```
```JAVA
public class Calculator {
    private int memory;

    public int getMemory() {
        return memory;
    }

    public void setMemory(int memory) {
        this.memory = memory;
        try {
            Thread.sleep(2000);
        } catch(InterruptedException e) {}
        System.out.println(Thread.currentThread().getName() + ": " + this.memory);
    }
}
```
```JAVA
public class MainThreadExample {
    public static void main(String[] args) {
        Calculator calculator = new Calculator();

        User1 user1 = new User1();
        user1.setCalculator(calculator);
        user1.start();

        User2 user2 = new User2();
        user2.setCalculator(calculator);
        user2.start();
    }
}
```
![image](https://user-images.githubusercontent.com/79847020/140734534-3dd47f58-7b3a-47f1-94df-62ff285692b3.png)

## 4.2 동기화 메소드 및 동기화 블록

스레드가 사용 중인 객체를 다른 스데르가 변경할 수 없도록 하려면 스레드 작업이 끝날 때까지 객체에 잠금을 걸어서 다른 스레드가 사용할 수 없도록 해야 한다. 멀티 스레드 프로그램에서 단 하나의 스레드만 실행할 수 있는 코드 영역을 임계 영역(Critical Section)이라고 한다. 자바는 임계 영역을 지정하기 위해 동기화(synchronized) 메소드와 동기화 블록을 제공한다. 

스레드가 객체 내부의 동기화 메소드 또는 블록에 들어가서 즉시 객체에 잠금을 걸어 다른 스레드가 임계 영역 코드를 실행하지 못하도록 한다. 동기화 메소드를 만드는 방법은 다음과 같이 메소드 선언에 synchronized 키워드를 붙이면 된다. synchronized 키워드는 인스턴스와 정적 메소드 어디든 붙일 수 있다.

Synchronized 메소드
```JAVA
public synchronized void method() {
    임계 영역 ; // 단 하나의 스레드만 실행
}
```

Synchronized 블록
```JAVA
public void method() {
    //여러 스레드가 실행 가능 영역
    ...
    //공유 객체가 객체 자신이면 this를 넣을 수 있다.
    synchronized(공유 객체) { //동기화 블록
        // 단 하나의 스레드만 실행
    }
}
```

만약 동기화 메소드와 동기화 블록이 여러 개 있을 경우, 스레드가 이들 중 하나를 실행할 때 다른 스레드는 해당 메소드는 물론이고 다른 동기화 메소드 및 블록도 실행할 수 없다. 하지만 일반 메소드는 실행이 가능하다.

```JAVA
public class Calculator {
    private int memory;

    public int getMemory() {
        return memory;
    }

    public synchronized void setMemory(int memory) {
        this.memory = memory;
        try {
            Thread.sleep(2000);
        } catch(InterruptedException e) {}
        System.out.println(Thread.currentThread().getName() + ": " + this.memory);
    }
}
```
![image](https://user-images.githubusercontent.com/79847020/140740620-78f5290a-6cf8-433c-8a05-a12daf2e1a51.png)

User1 스레드는 Calculator 객체의 동기화 메소드인 setMemory()를 실행하는 순간 Calculator 객체를 잠근다. 메인 스레드가 User2 스레드를 실행시키지만 동기화 메소드인 setMemory()를 실행시키지는 못하고 User1이 setMemory()를 모두 실행할 동안 대기해야 한다. 

동기화 블록으로도 해당 문제를 해결할 수 있다.
```JAVA
public void setMemory(int memory) {
    synchronized(this) {
        this.memory = memory;
        try {
            Thread.sleep(2000);
        } catch(InterruptedException e) {}
        System.out.println(Thread.currentThread().getName() + ": " + this.memory);
    }
}
```

스레드가 동기화 블록으로 들어가면 this(Calculator 객체)를 잠그고 동기화 블록을 실행한다. 동기화 블록을 모두 실행할 때까지 다른 스레드들은 this(Calculator 객체)의 모든 동기화 메소드 또는 동기화 블록을 실행할 수 없게 된다.

# 5. 스레드 상태

![image](https://user-images.githubusercontent.com/79847020/140945004-aba2bf83-1889-421f-873d-07aa4ac678ec.png)

## 5.1 스레드 상태 종류

스레드 객체를 생성하고 start() 메소드를 호출하면 실행 대기 상태가 된다. 실행 대기 상태란 아직 스케줄링이 되지 않아 실행을 기다리는 상태를 말한다. 실행 대기 상태에 있는 스레드 중에서 스레드 스케줄링으로 선택된 스레드가 비로서 run() 메소드를 실행한다. 이때를 실행(Running) 상태라고 한다. 실행 상태의 스레드는 스레드 스케줄링에 의해 다시 실행 대기 상태로 돌아갈 수 있다. 이렇게 스레드는 실행 대기 상태와 실행 상태를 번갈아가면서 자신의 run() 메소드를 조금씩 실행한다. 스레드의 run() 메소드가 종료되면 종료 상태가 된다.

실행 상태에서 일시 정지 상태로 가기도 하는데 일시 정지 사태는 스레드가 실행할 수 없는 상태이다. 스레드가 다시 실행 상태로 가기 위해서는 일시 정지 상태에서 실행 대기 상태로 가야 한다.

|상태|열거 상수|설명|
|---|---|---|
|객체 생성|NEW|스레드 객체가 생성, 아직 start() 메소드가 호출되지 않은 상태|
|실행 대기|RUNNABLE|실행 상태로 언제든지 갈 수 있는 상태|
|일시 정지|WAITING|다른 스레드가 통지할 때까지 기다리는 상태|
|일시 정지|TIMED_WAITING|주어진 시간 동안 기다리는 상태|
|일시 정지|BLOCKED|사용하고자 하는 객체의 락이 풀릴 때까지 기다리는 상태|
|종료|TERMINATED|실행을 마친 상태|

```JAVA
public class StatePrintThread extends Thread {
    private Thread targetThread;

    public StatePrintThread(Thread targetThread) {
        this.targetThread = targetThread;
    }

    public void run() {
        while(true) {
            Thread.State state = targetThread.getState();
            System.out.println("타겟 스레드 상태 : " + state);

            if(state == Thread.State.NEW) targetThread.start();

            if(state == State.TERMINATED) break;

            try {
                Thread.sleep(500);
            } catch(Exception e) {}
        }
    }

}
```
```JAVA
public class TargetThread extends Thread {
    public void run() {
        for(long i=0; i<1000000000; i++) {}

        try {
            //1.5초간 일시 정지
            Thread.sleep(500);
        } catch(Exception e) {}

        for(long i=0; i<1000000000; i++) {}
    }
}
```
```JAVA
public class ThreadStateExample {
    public static void main(String[] args) {
        StatePrintThread statePrintThread = new StatePrintThread(new TargetThread());
        statePrintThread.start();
    }
}
```
```CONSOLE
NEW -> RUNNABLE -> TIMED_WAITING -> RUNNABLE -> TERMINATED
```

## 5.2 스레드 상태 제어

실행 중인 스레드의 상태를 변경하는 것을 스레드 상태 제어라고 한다. 멀티 스레드 프로그램을 만들기 위해서는 정교한 스레드 상태 제어가 필요한데 상태 제어가 잘못되면 프로그램은 불안정해져서 먹통이 되거나 다운된다. 스레드 제어를 제대로 하기 위해서는 스레드의 상태 변화를 가져오는 메소드들을 파악하고 있어야 한다.

|메소드|설명|
|---|---|
|interrupt()|일시 정지 상태의 스레드에서 InterruptedException 예외를 발생시켜, 예외 처리 코드(catch)에서 실행 대기 상태로 가거나 종료 상태로 갈 수 있도록 한다.|
|notify()<br>notifyAll()|동기화 블록 내에서 wait() 메소드에 의해 일시 정지 상태에 있는 스레드를 실행 대기 상태로 만든다.|
|resume()|suspend() 메소드에 의해 일시 정지 상태에 있는 스레드를 실행 대기 상태로 만든다.<br> -Deprecated (대신 notify(), notifyAll() 사용)|
|sleep(long millis)<br>sleep(long millis, int nanos)|주어진 시간 동안 스레드를 일시 정지 상태로 만든다. 주어진 시간이 지나면 자동적으로 실행 대기 상태가 된다.|
|join()<br>join(long millis)<br>join(long millis, int nanos)|join() 메소드를 호출한 스레드는 일시 정지 상태가 된다. 실행 대기 상태로 가려면, join() 메소드를 멤버로 가지는 스레드가 종료되거나, 매개값으로 주어진 시간이 지나야 한다.|
|wait()<br>wait(long millis)<br>join(long millis, int nanos)|동기화(synchronized) 블록 내에서 스레드를 일시 정지 상태로 만든다. 매개값으로 주어진 시간이 지나면 자동적으로 실행 대기 상태가 된다. 시간이 주어지지 않으면 notify(), notifyAll() 메소드에 의해 실행 대기 상태로 갈 수 있다.|
|suspend()|스레드를 일시 정지 상태로 만든다. resume() 메소드를 호출하면 다시 실행 대기 상태가 된다. <br> -Deprecated (대신 wait() 사용)|
|yield()|실행 중에 우선순위가 동일한 다른 스레드에게 실행을 양보하고 실행 대기 상태가 된다.|
|stop()|스레드를 즉시 종료시킨다. - Deprecated|

위 표에서 wait(), notify(), notifyAll()은 Object 클래스의 메소드이고 그 이외의 메소드는 모두 Thread 클래스의 메소드들이다. wait(), notify(), notifyAll() 메소드의 사용방법은 스레드의 동기화에서 다시 설명하겠다.

### 5.2.1 sleep() - 주어진 시간 동안 일시 정지

실행 중인 스레드를 일정 시간 멈추게 하고 싶다면 Thread 클래스의 정적 메소드인 sleep()을 사용하면 된다. 밀리세컨드(1/1000) 단위로 시간을 주면 된다. 일시 정지 상태에서 주어진 시간이 되기 전에 interrupt() 메소드가 호출되면 InterruptedException이 발생한다.

### 5.2.2 yield() - 다른 스레드에게 실행 양보

yield() 메소드를 호출한 스레드는 실행 대기 상태로 돌아가고 동일한 우선순위 또는 높은 우선순위를 갖는 다른 스레드가 실행 기회를 가질 수 있도록 해준다.

```JAVA
package thisisjava.chap12;

public class YieldExample {
    public static void main(String[] args) {
        TestThread thread1 = new TestThread("thread1");
        TestThread thread2 = new TestThread("thread2");
        thread1.start();
        thread2.start();

        try { Thread.sleep(1000); } catch (InterruptedException e) {}
        thread1.work = false;

        try { Thread.sleep(1000); } catch (InterruptedException e) {}

        thread1.work = true;

        try { Thread.sleep(1000); } catch (InterruptedException e) {}

        thread1.stop = true;
        thread2.stop = true;
    }
}

class TestThread extends Thread {
    public boolean stop = false; //종료 플래스
    public boolean work = true; //작업 진행 여부 플래그

    public TestThread(String name) {
        super(name);
    }

    @Override
    public void run() {
        while(!stop) {
            //stop이 true가 되면 while종료
            if(work) System.out.println(this.getName() + " 작업 내용");
            // work가 false면 다른 스레드에게 실행 양보
            else Thread.yield();
        }
        System.out.println(this.getName() + " 작업 종료");
    }
}
```

### 5.2.3 join() - 다른 스레드의 종료를 기다림

스레드는 다른 스레드와 독립적으로 실행하는 것이 기본이지만 다른 스레드가 종료될 때까지 기다렸다가 실행해야 하는 경우가 발생할 수도 있다. 

ThreadA가 ThreadB의 join 메소드를 호출하면 ThreadA는 ThreadB가 종료될 때까지 일시 정지 상태가 된다. ThreadB() run() 메소드가 종료되면 비로소 ThreadA는 일시 정지에서 풀려 다음 코드를 실행하게 된다. 

```JAVA
package thisisjava.chap12;

public class SumThreadExample {
    public static void main(String[] args) {
        SumThread thread = new SumThread();
        thread.start();

        System.out.println(thread.sum);

        try {
            thread.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println(thread.sum);
    }
}

class SumThread extends Thread {
    public long sum = 0;

    @Override
    public void run() {
        for(int i=1; i<=100; i++) {
            sum += i;
        }
    }
}
```

### 5.2.4 스레드 간 협업(wait(), notify(), notifyAll())

경우에 따라서는 두 개의 스레드를 교대로 번갈아가며 실행해야 할 경우가 있다. 자신의 작업이 끝나면 상대방 스레드를 일시 정지 상태에서 풀어주고 자신은 일시 정지 상태로 만드는 것이다. 이 방법의 핵심은 공유 객체에 있다. 공유 객체는 두 스레드가 작업할 내용을 각각 동기화 메소드로 구분해 놓는다. 한 스레드가 작업을 완료하면 notify() 메소드를 호출해서 일시 정지 상태에 있는 다른 스레드를 실행 대기 상태로 만들고 자신은 두 번 작업을 하지 않도록 wait() 메소드를 호출하여 일시 정지 상태로 만든다.

notify()는 wait()에 의해 일시 정지된 스레드 중 한 개를 실행 대기 상태로 만들고 notifyAll() 메소드는 wait()에 의해 일시 정지된 모든 스레드들을 실행 대기 상태로 만든다. 이 메소드들은 Thread 클래스가 아닌 Object 클래스에 선언된 메소드이므로 모든 공유 객체에서 호출이 가능하다. 주의할 점은 이 메소드들은 동기화 메소드 또는 동기화 블록 내에서만 사용할 수 있다.

```JAVA
public class ConsumerThread extends Thread {
    private DataBox dataBox;

    public ConsumerThread(DataBox dataBox) {
        this.dataBox = dataBox;
    }

    @Override
    public void run() {
        for(int i=1; i<=3; i++) {
            String data = dataBox.getData();
        }
    }
}

public class ProducerThread extends Thread {
    private DataBox dataBox;

    public ProducerThread(DataBox dataBox) {
        this.dataBox = dataBox;
    }

    @Override
    public void run() {
        for(int i=1; i<=3; i++) {
            String data = "Data-" + i;
            dataBox.setData(data);
        }
    }
}

public class DataBox {
    private String data;

    public synchronized String getData() {
        if (this.data == null) {
            try {
                wait();
            } catch (InterruptedException e) {}
        }

        String returnValue = data;
        System.out.println("ConsumerThread가 읽은 데이터 : " + returnValue);
        data = null;
        notify();
        return returnValue;
    }

    public synchronized void setData(String data) {
        if (this.data != null) {
            try {
                wait();
            } catch (InterruptedException e) {}
        }
        this.data = data;
        System.out.println("ProducerThread가 생성한 데이터 : " + data);
        notify();
    }
}

public class WaitNotifyExample {
    public static void main(String[] args) {
        DataBox dataBox = new DataBox();

        ProducerThread producerThread = new ProducerThread(dataBox);
        ConsumerThread consumerThread = new ConsumerThread(dataBox);

        producerThread.start();
        consumerThread.start();
    }
}
```
```CONSOLE
ProducerThread가 생성한 데이터 : Data-1
ConsumerThread가 읽은 데이터 : Data-1
ProducerThread가 생성한 데이터 : Data-2
ConsumerThread가 읽은 데이터 : Data-2
ProducerThread가 생성한 데이터 : Data-3
ConsumerThread가 읽은 데이터 : Data-3
...
```

### 5.2.5 stop 플래그, interrupt() - 스레드의 안전한 종료|

스레드는 자신의 run() 메소드가 모두 실행되면 자동적으로 종료된다. 경우에 따라서는 실행 중인 스레드를 즉시 종료할 필요가 있다. 스레드를 종료 시키는 메소드 stop()을 제공했었는데 deprecated(사용중단) 되었다. 그 이유는 stop() 메소드로 스레드를 갑자기 종료하게 되면 스레드가 사용 중이던 자원들인 불안전한 상태로 남겨지기 때문이다.

#### stop 플래그를 이용하는 방법

스레드 run() 메소드가 끝나면 자동적으로 종료되므로 run() 메소드가 정상적으로 종료되도록 유도하는 것이 최선의 방법이다. stop 플래그를 이용해서 run() 메소드의 종료를 유도한다.

```JAVA
public class XXThread extends Thread {
    public void run() {
        private boolean stop; // stop 플래그

        while(!stop) {
            스레드 반복코드;
        }
        스레드가 사용한 자원 정리    
    }
}
```


#### interrupt() 메소드를 이용하는 방법

interrupt() 메소드는 스레드가 일시 정지 상태에 있을 때 InterruptedException 예외를 발생시키는 역할을 한다. 이것을 사용하면 run() 메소드를 정상 종료시킬 수 있다.

```JAVA
public class XXThread extends Thread {
    public void run() {
        try { 
            while(true) {
              스레드 반복 코드
              Thread.sleep(1) // InterruptedException 예외발생
            }
        } catch(InterruptedException e) {}
        스레드가 사용한 자원 정리    
    }
}

...생략
Thread thread = new XXThread();
thread.start();
thread.interrupt();
```

주목할 점은 스레드가 실행 대기 또는 실행 상태에 있을 때 interrupt() 메소드가 실행되면 즉시 InterruptedException 예외가 발생하지 않고 스레드가 미래에 일시 정지 상태가 되면 InterruptedException 예외가 발생한다는 것이다. 따라서 스레드가 일시 정지 상태가 되지 않으면 interrupt() 메소드 호출은 아무런 의미가 없다. 따라서 Thread.sleep(1) 이 필요한것이다.

Thread.sleep(1) 을 이용하지 않고자 한다면 thread.interrupted()와 thread.isInterrupted() 메소드를 활용해 앞에서 봤던 Stop플래그로 이용해도 된다.
스레드의 interrupted()와 isInterrupted() 메소드는 스레드의 interrupted상태를 확인할 수 있는 Boolean 메소드이다. 

# 6. 데몬 스레드

데몬 스레드는 메인 스레드의 작업을 돕는 보조적인 역할을 수행하는 스레드로 주 스레드가 종료되면 데몬 스레드 더는 존재 의미가 없기에 강제로 종료됩니다. 워드의 자동 저장 기능을 예로 들을 수 있습니다. 데몬 스레드를 만드는 방법은 스레드를 만들고 해당 스레드에 setDaemon(true); 메소드를 세팅하는 것입니다 

주의할 점은 start() 메소드가 호출되고 나서 setDaemon(true)를 호출하면 IllegalThreadStateException이 발생하기 때문에 start() 메소드 호출 전에 setDaemon(true)를 호출해야 한다. 현재 실행 중인 스레드가 데몬 스레드인지 아닌지를 구별하는 방법은 isDaemon() 메소드를 활용하면 된다. 

다음 예제는 1초 주기로 save() 메소드를 자동 호출하도록 AutoSaveThread를 작성하고 메인 스레드가 3초 후 종료되면 AutoSaveThread도 같이 종료되도록 AutoSaveThread를 데몬스레드로 만들었다.
```JAVA
public class DaemonExample {
    public static void main(String[] args) {
        Thread autoSaveThread = new Thread(() ->{
            while(true) {
                try {
                    Thread.sleep(1000);
                } catch(InterruptedException e) {
                    break;
                }
                System.out.println("작업 내용을 저장함. ");
            }
        });

        autoSaveThread.setDaemon(true);
        autoSaveThread.start();

        try {
            Thread.sleep(3050);
        } catch (InterruptedException e) {}

        System.out.println("메인 스레드 종료");
    }
}
```
```CONSOLE
작업 내용을 저장함. 
작업 내용을 저장함. 
작업 내용을 저장함. 
메인 스레드 종료
```
# 7. 스레드 그룹

스레드 그룹이란 관련된 스레드를 묶어서 관리할 목적으로 이용된다. 스레드는 반드시 하나의 스레드 그룹에 포함되는데 명시적으로 스레드 그룹에 포함시키지 않으면 기본적으로 자신을 생성한 스레드와 같은 스레드 그룹에 속하게 된다. 우리가 생성하는 작업 스레드는 main스레드가 생성하므로 기본적으로 main 스레드 그룹에 속하게 된다.

## 7.1 스레드 그룹 이름 얻기

현재 스레드가 속한 스레드 그룹의 이름을 얻고 싶다면 다음과 같은 코드를 사용할 수 있다.
```JAVA
ThreadGroup group = Thread.currentThread.getThreadGroup();
String groupName = group.getName();
```
Thread의 정적 메소드인 getAllStackTraces()를 이용하면 프로세스 내에서 실행하는 모든 스레드에 대한 정보를 얻을 수 있다. getAllStackTraces() 메소드는 Map타입의 객체를 리턴하는데 키는 스레드 객체이고 값은 스레드의 상태 기록들을 갖고 있는 StackTraceElement[] 배열이다.
```JAVA
Map<Thread, StackTraceElement[]> map = Thread.getAllStackTraces();
```

```JAVA
public class ThreadInfoExample {
    public static void main(String[] args) {
        Thread autoSaveThread = new Thread(() ->{
            while(true) {
                try {
                    Thread.sleep(1000);
                } catch(InterruptedException e) {
                    break;
                }
                System.out.println("작업 내용을 저장함. ");
            }
        });

        autoSaveThread.setDaemon(true);
        autoSaveThread.start();

        Map<Thread, StackTraceElement[]> map = Thread.getAllStackTraces();
        Set<Thread> threads = map.keySet();

        for(Thread thread : threads) {
            System.out.println("Name: " + thread.getName() + ((thread.isDaemon())?"(데몬)":"(주)"));
            System.out.println("\n" + "소속그룹: " + thread.getThreadGroup().getName());
            System.out.println();
        }
     }
}
```
```CONSOLE
Name: Signal Dispatcher(데몬)
소속그룹: system

Name: AutoSaveThread(데몬)
소속그룹: main

Name: main(주)
소속그룹: main

Name: Attach Listener(데몬)
소속그룹: system

Name: Reference Handler(데몬)
소속그룹: system

..생략
```

실행 결과를 보면 가비지 컬렉션을 담당하나느 Finalizer 스레드를 비롯한 일부 스레드들이 system 그룹에 속하고 main() 메소드를 실행하는 main 스레드는 system그룹의 하위그룹이 main에 속하는 것을 볼 수 있다. 그리고 main 스레드가 실행시킨 AutoSaveThread는 main 스레드가 소속된 main 그룹에 포함되어 있는 것을 볼 수 있다.

## 7.2 스레드 그룹 생성

```JAVA
ThreadGroup tg = new ThreadGroups(String name);
```

스레드 그룹 생성시 부모 스레드 그룹을 지정하지 않으면 현재 스레드가 속한 그룹의 하위 그룹으로 생성된다.


## 7.3 스레드 그룹의 일괄 interrupt()

스레드 그룹은 그룹 내에 포함된 모든 스레드들을 일괄 interrupt() 할 수 있다. 스레드 그룹의 interrupt() 메소드는 소속된 스레드의 interrupt() 메소드를 호출만 할 뿐 개별 스레드에서 발생하는 InterruptedException에 대한 예외 처리를 하지 않는다. 따라서 안전한 종료를 위해서는 개별 스레드가 예외 처리를 해야 한다.

스레드 그룹이 제공하는 다양한 메소드를 통해서 그룹을 제어합니다. 아래 예제코드는 myGroup이라는 스레드 그룹을 만들고 interrupt()를 통해 해당 그룹에 속하는 모든 thread를 중지시키는 예제입니다.

```JAVA
public class ThreadGroupExample {
    public static void main(String[] args) {
        ThreadGroup myGroup = new ThreadGroup("myGroup");
        WorkThread workThreadA = new WorkThread(myGroup, "workThreadA");
        WorkThread workThreadB = new WorkThread(myGroup, "workThreadB");

        workThreadA.start();
        workThreadB.start();

        System.out.println("[main 스레드 그룹의 list() 메소드 출력 내용");
        ThreadGroup mainGroup = Thread.currentThread().getThreadGroup();
        mainGroup.list();
        System.out.println();

        try { Thread.sleep(3000);} catch(InterruptedException e) {}

        System.out.println("[myGroup 스레드 그룹의 interrupt() 메소드 호출 ]");
        myGroup.interrupt();
    }

}

public class WorkThread extends Thread {
    public WorkThread(ThreadGroup threadGroup, String threadName) {
        //스레드 그룹과 스레드 이름을 설정
        super(threadGroup, threadName);
    }
    
    @Override
    public void run() {
        while(true) {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                //InterruptedException이 발생될 때, while문을 빠져나와 스레드를 종료시킴
                System.out.println(getName() + " interrupted");
                break;
            }
        }
        System.out.println(getName() + " 종료됨");
    }
}
```
```CONSOLE
[main 스레드 그룹의 list() 메소드 출력 내용
java.lang.ThreadGroup[name=main,maxpri=10]
    Thread[main,5,main]
    Thread[Monitor Ctrl-Break,5,main]
    java.lang.ThreadGroup[name=myGroup,maxpri=10]
        Thread[workThreadA,5,myGroup]
        Thread[workThreadB,5,myGroup]

[myGroup 스레드 그룹의 interrupt() 메소드 호출 ]
workThreadA interrupted
workThreadB interrupted
workThreadB 종료됨
workThreadA 종료됨
```

# 8. 스레드 풀

병렬 작업 처리가 많아지면 스레드 개수가 증가되고 그에 따른 스레드 생성과 스케줄링으로 인해 CPU가 바빠져 메모리 사용량이 늘어난다. 따라서 애플리케이션의 성능이 저하된다. 병렬 작업의 폭증으로 인한 스레드의 폭증을 막으려면 스레드풀(ThreadPool)을 사용해야 한다. 스레드풀은 작업 처리에 사용되는 스레드를 제한된 개수만큼 정해 놓고 작업 큐(Queue)에 들어오는 작업들을 하나씩 스레드가 맡아 처리한다. 작업 처리 요청이 폭증해도 스레드의 전체 개수가 늘어나지 않으므로 애플리케이션의 성능이 급격히 저하되지 않는다.

자바는 스레드풀을 생성하고 사용할 수 있도록 java.util.concurrent 패키지에서 ExecutorService 인터페이스와 Executors 클래스를 제공하고 있다. Executors의 다양한 정적 메소드를 이용해서 ExecutorService 구현 객체를 만들 수 있는데 이것이 바로 스레드풀이다.

![image](https://user-images.githubusercontent.com/79847020/141249788-a09d60fe-f824-43fc-bf91-60ad78169125.png)

## 8.1 스레드풀 생성 및 종료

### 스레드 풀 생성

ExecutorService 구현 객체는 Executors 클래스의 다음 두 가지 메소드 중 하나를 이용해서 간편하게 생성할 수 있다.

|메소드명(매개 변수)|초기 스레드 수|코어 스레드 수|최대 스레드 수|
|---|---|---|---|
|newCachedThreadPool()|0|0|Integer.MAX_VALUE|
|newFixedThreadPool(int nThreads)|0|nThreads|nThreads|

코어 스레드 수는 스레드 수가 증가된 후 사용되지 않은 스레드를 스레드풀에서 제거할 때 최소한으로 유지해야 할 스레드 수를 말한다.

newCachedThreadPool()을 호출해서 ExcutorService 구현 객체를 얻는 코드이다.

```JAVA
ExecutorSerice executorService = Executors.newCachedThreadPool();
```

newFixedThreadPool은 스레드가 작업을 처리하지 않고 놀고 있더라도 스레드 개수가 줄지 않는다. 다음은 CPU 코어의 수만큼 최대 스레드를 사용하는 스레드풀을 생성한다.

```JAVA
ExecutorService executorService = Executors.newFixedThreadPool(
    Runtime.getRuntime().availableProcessors()
);
```

ThreadPoolExecutor 객체를 직접 생성하는 방법도 있다. 사실 위 두가지 메소드도 내부적으로 ThreadPoolExecutor객체를 생성해서 리턴한다.


```JAVA
ExecutorService threadPool = new ThreadPoolExecutor(
    3,     //코어 스레드 개수
    100,   //최대 스레드 개수
    120L,  //놀고 있는 시간
    TimeUnit.SECONDS, //놀고 있는 시간 단위
    new SynchronousQueue<Runnable>() //작업 큐
);
```

### 스레드풀 종료

스레드는 main 스레드가 종료되더라도 작업을 처리하기 위해 실행 상태로 남아있다. 애플리케이션을 종료하려면 스레드풀을 종료시켜 스레드들이 종료 상태가 되도록 처리해주어야 한다. ExecutorService는 종료와 관련해서 다음 세 개의 메소드를 제공하고 있다.

|리턴 타입|메소드명(매개변수)|설명|
|---|---|---|
|void|shutdown()|현재 처리 중인 작업뿐만 아니라 작업 큐에 대기하고 있는 모든 작업을 처리한뒤에 스레드풀을 종료시킨다.|
|List<Runnable>|shutdownNow()|현재 작업 처리 중인 스레드를 interrupt해서 작업 중지를 시도하고 스레드풀을 종료시킨다. 리턴값은 작업 큐에 있는 미처리된 작업(Runnable)의 목록이다.|
|boolean|awaitTermination(long timeout, TimeUnit unit)|shutdown() 메소드 호출 이후 모든 작업 처리를 timeout 시간 내에 완료하면 true를 리턴하고 완료하지 못하면 작업 처리 중인 스레드를 itnerrupt하고 false를 리턴한다.|

## 8.2 작업 생성과 처리 요청
    
### 작업 생성
    
하나의 작업은 Runnable 또는 Callable 구현 클래스로 표현한다 Runnable 과 Callable의 차이점은 작업 처리 완료 후 리턴값이 있느냐 없느냐 이다. 
Runnable의 run()은 리턴값이 없고 Callable의 call() 메소드는 Callable<T>에서 지정한 T 타입이다.
    
* Runnable 구현 클래스

```JAVA
Runnable task = new Runnable() {
    @Override
    public void run() {
        //작업
    }
}
```

* Callable 구현 클래스

```JAVA
Callable<T> task = new Callable<T> {
    @Override
    public T call() throws Exception {
        //작업
        return T;
    }
}
```
    
### 작업 처리 요청
    
작업 처리 요청이란 ExecutorService의 작업 큐에 Runnable 또는 Callable 객체를 넣는 행위를 말한다. 
    
|리턴타입|메소드명(매개변수)|설명|
|---|---|---|
|void|execute(Runnable command)|-Runnable을 작업 큐에 저장<br>-작업 처리 결과를 받지 못함<br>-예외 발생시 해당 스레드 종료, 스레드풀에서 제거|
|Future<?><br>Future<V><br><Future<V>|submit(Runnable task)<br>submit(Runnable task, V result)<br>submit(Callable<V> task)|-Runnable,Callable을 작업 큐에 저장<br>-리턴된 Future를 통해 작업 처리 결과를 얻을 수 있음<br>-예외 발생 시 해당 스레드 종료되지 않고 다음 작업을 위해 재사용|

```JAVA
public class ExecuteExample {
    public static void main(String[] args) throws InterruptedException {
        //최대 스레드 개수가 2인 스레드 풀 생성
        ExecutorService executorService = Executors.newFixedThreadPool(2);

        for(int i=0; i<10; i++) {
            Runnable runnable = new Runnable() {
                @Override
                public void run() {
                    //스레드 총 개수 및 작업 스레드 이름 출력
                    ThreadPoolExecutor threadPoolExecutor = (ThreadPoolExecutor) executorService;
                    int poolSize = threadPoolExecutor.getPoolSize();
                    String threadName = Thread.currentThread().getName();
                    System.out.println("[총 스레드 개수: " + poolSize + "] 작업 스레드 이름: " + threadName);
                    //예외 발생 시킴
                    int value = Integer.parseInt("삼");
                }
            };

            executorService.execute(runnable);
            //executorService.submit(runnable);

            Thread.sleep(10); //콘솔에 출력 시간을 주기 위해 0.01초 일시 정지됨
        }

        executorService.shutdown();
    }
}
```
```CONSOLE
[총 스레드 개수: 1] 작업 스레드 이름: pool-1-thread-1
[총 스레드 개수: 2] 작업 스레드 이름: pool-1-thread-2
[총 스레드 개수: 2] 작업 스레드 이름: pool-1-thread-3
Exception in thread "pool-1-thread-2" java.lang.NumberFormatException: For input string: "삼"
	at java.base/java.lang.NumberFormatException.forInputString(NumberFormatException.java:65)
	at java.base/java.lang.Integer.parseInt(Integer.java:652)
	at java.base/java.lang.Integer.parseInt(Integer.java:770)
	at thisisjava.chap12.ExecuteExample$1.run(ExecuteExample.java:22)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.lang.Thread.run(Thread.java:834)
......
......
[총 스레드 개수: 2] 작업 스레드 이름: pool-1-thread-10
Exception in thread "pool-1-thread-10" java.lang.NumberFormatException: For input string: "삼"
	at java.base/java.lang.NumberFormatException.forInputString(NumberFormatException.java:65)
	at java.base/java.lang.Integer.parseInt(Integer.java:652)
	at java.base/java.lang.Integer.parseInt(Integer.java:770)
	at thisisjava.chap12.ExecuteExample$1.run(ExecuteExample.java:22)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.lang.Thread.run(Thread.java:834)

```
이번에는 submit() 메소드로 작업 처리를 요청한 경우를 살펴보자
```JAVA
            //executorService.execute(runnable);
            executorService.submit(runnable);
```
```CONSOLE
[총 스레드 개수: 2] 작업 스레드 이름: pool-1-thread-2
[총 스레드 개수: 1] 작업 스레드 이름: pool-1-thread-1
[총 스레드 개수: 2] 작업 스레드 이름: pool-1-thread-1
[총 스레드 개수: 2] 작업 스레드 이름: pool-1-thread-2
[총 스레드 개수: 2] 작업 스레드 이름: pool-1-thread-1
[총 스레드 개수: 2] 작업 스레드 이름: pool-1-thread-2
[총 스레드 개수: 2] 작업 스레드 이름: pool-1-thread-1
[총 스레드 개수: 2] 작업 스레드 이름: pool-1-thread-2
[총 스레드 개수: 2] 작업 스레드 이름: pool-1-thread-1
[총 스레드 개수: 2] 작업 스레드 이름: pool-1-thread-2
```
실행 결과를 보면 확실히 execute()와의 차이점을 발견할 수 있다. 예외가 발생하더라도 스레드가 종료되지 않고 계속 재사용되어 다른 작업을 처리하고 있는 것을 볼 수 있다.

## 8.3 블로킹 방식의 작업 완료 통보

ExecutorService의 submit() 메소드는 매개값으로 준 Runnable, Callable 작업을 스레드풀의 작업 큐에 저장하고 즉시 Future 객체를 리턴한다. Future 객체는 작업 결과가 아니라 작업이 완료 될 때까지 기다렸다가(지연했다가=블로킹되었다가) 최종 결과를 얻는데 사용된다. 그래서 Future를 지연완료(pending completion) 객체라고 한다.

|리턴타입|메소드명(매개변수|
|---|---|
|Future<?>|submit(Runnable task)|
|Future<V>|submit(Runnable task, V result)|
|Future<V>|submit(Callable<V> task)|

Future의 get() 메소드를 호출하면 스레드가 작업을 완료할 때까지 블로킹되었다가 작업을 완료하면 처리 결과를 리턴한다. 이것이 블로킹을 사용하는 작업 완료 통보 방식이다. 

|리터타입|메소드명(매개변수)|
|---|---|
|V|get()|

다음은 3가지 타입의 submit의 리턴 타입을 보여준다.    

|메소드|작업 처리 완료 후 리턴 타입|작업 처리 중 예외발생|
|---|---|---|
|submit(Runnable task)|future.get()->null|future.get()->예외발생|
|submit(Runnable task, Integer result)|future.get()->int 타입 값|future.get()->예외발생|
|submit(Callable<String> task)|future.get()->String 타입 값|future.get()->예외발생|

Future를 이용한 블로킹 방식의 작업 완료 통보에서 주의할 점은 작업을 처리하는 스레드가 작업을 완료하기 전에는 get() 메소드가 블로킹되므로 다른 코드를 실행할 수 없다. 그렇기 때문에 get() 메소드를 호출한는 스레드는 새로운 스레드이거나 스레드풀의 또 다른 스레드가 되어야 한다.
    
### 리턴값이 없는 작업 완료 통보
    
리턴값이 없는 작업일 경우는 Runnable 객체로 생성하면 된다. submit(Runnable task) 메소드를 이용하면 된다. Future 객체를 리턴하는데 이것은 스레드가 작업 처리를 정상적으로 완료했는지 아니면 예외가 발생했는지 확인하기 위해서이다.
    
### 리턴값이 있는 작업 완료 통보
    
```JAVA
package thisisjava.chap12;

import java.util.concurrent.Callable;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

public class ResultByCallableExample {
    public static void main(String[] args) {
        ExecutorService executorService = Executors.newFixedThreadPool(Runtime.getRuntime().availableProcessors());

        System.out.println("[작업 처리 요청]");

        Callable<Integer> task = new Callable<Integer>() {
            @Override
            public Integer call() throws Exception {
                int sum = 0;
                for (int i = 1; i <= 10; i++) {
                    sum += i;
                }
                return sum;
            }
        };
        Future<Integer> future = executorService.submit(task);

        try {
            int sum = future.get();
            System.out.println("[처리 결과] " + sum);
            System.out.println("[작업 처리 완료]");
        } catch (Exception e) {
            System.out.println("[실행 예외 발생함] " + e.getMessage());
        }

        executorService.shutdown();
    }
}
```
```CONSOLE
[작업 처리 요청]
[처리 결과] 55
[작업 처리 완료]
```
   

### 작업 처리 결과를 외부 객체에 저장

상황에 따라 스레드가 작업한 결과를 외부 객체에 저장해야 할 경우도 있다. 예를 들어 스레드가 작업 처리를 완료하고 외부 Result 객체에 작업 결과를 저장하면 애플리케이션이 Result 객체를 사용해서 어떤 작업을 진행할 수 잇을 것이다. 대개 Result 객체는 공유 객체가 되어 두 개 이상의 스레드 작업을 취합할 목적으로 이용된다. 
	
주의할 점은 스레드에서 결과를 저장하기 위해 외부 Result 객체를 사용해야 하므로 생성자를 통해 Result 객체를 주입받도록 해야 한다.
	
```JAVA
public class ResultByRunnableExample {
    public static void main(String[] args) {
        ExecutorService executorService = Executors.newFixedThreadPool(Runtime.getRuntime().availableProcessors());

        System.out.println("[작업 처리 요청]");

        class Task implements Runnable {
            Result result;

            Task(Result result) {
                this.result = result;
            }

            @Override
            public void run() {
                int sum = 0;
                for(int i=1; i<=10; i++) {
                    sum+= i;
                }
                result.addValue(sum);
            }
        };

        Result result = new Result();
        Runnable task1 = new Task(result);
        Runnable task2 = new Task(result);
        Future<Result> future1 = executorService.submit(task1, result);
        Future<Result> future2 = executorService.submit(task2, result);

        try {
            result = future1.get();
            result = future2.get();

            System.out.println("[처리 결과] " + result.accumValue);
            System.out.println("[작업 처리 완료]");
        } catch(Exception e) {
            e.printStackTrace();
            System.out.println("[실행 예외 발생함] " + e.getMessage());
        }

        executorService.shutdown();
    }

}

class Result {
    int accumValue;

    synchronized void addValue(int value) {
        accumValue += value;
    }

}
```
	
### 작업 완료 순으로 통보

작업 요청 순서대로 작업 처리가 완료되는 것은 아니다. 작업의 양과 스레드 스케줄링에 따라서 먼저 요청한 작업이 나중에 완료되는 경우도 발생한다. 여러 개의 작업들이 순차적으로 처리될 필요성이 없고 처리 결과도 순차적으로 이용할 필요가 없다면 작업 처리가 완료된 것부터 결과를 얻어 이용하면 된다. 스레드 풀에서 작업 처리가 완료된 것만 통보 받는 방법이 있는데 CompletionService를 이용하는 것이다. CompletionService는 처리 완료된 작업을 가져오는 poll(), take()를 제공한다.
  
|리턴타입|메소드명(매개변수)|설명|
|---|---|---|
|Future<V>|poll()|완료된 작업의 Future를 가져옴. 완료된 작업이 없다면 즉시 null을 리턴함|
|Future<V>|take()()|완료된 작업의 Future를 가져옴. 완료된 작업이 있을 때까지 블로킹됨|

CompletionService 구현 클래스는 ExecutorCompletionService<V>이다. 
  
```JAVA
public class CompletionServiceExample extends Thread {
    public static void main(String[] args) {
        ExecutorService executorService = Executors.newFixedThreadPool(
                Runtime.getRuntime().availableProcessors()
        );

        CompletionService<Integer> completionService =
                new ExecutorCompletionService<Integer>(executorService);

        System.out.println("[작업 처리 요청]");

        for (int i = 0; i < 3; i++) {
            completionService.submit(new Callable<Integer>() {
                @Override
                public Integer call() throws Exception {
                    int sum = 0;
                    for (int i = 1; i <= 10; i++) {
                        sum += i;
                    }
                    return sum;
                }
            });
        }

        System.out.println("[작업 완료된 작업 확인]");
        executorService.submit(new Runnable() {
            @Override
            public void run() {
                while (true) {
                    try {
                        Future<Integer> future = completionService.take();
                        int value = future.get();
                        System.out.println("[처리 결과] " + value);
                    } catch (Exception e) {
                        break;
                    }
                }
            }
        });

        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
        }
        executorService.shutdownNow();
    }
}
```

## 8.4 콜백 방식의 작업 완료 통보
  
이번에는 콜백(callback) 방식을 이용해서 작업 완료 통보를 받는 방법에 대해서 알아보자. 콜백이란 애플리케이션이 스레드에게 작업 처리를 요청한 후 스레드가 작업을 완료하면 특정 메소드를 자동실행하는 기법을 말한다. 이때 자동 실행되는 메소드를 콜백 메소드라고 한다. 블로킹 방식과 콜백 방식의 차이를 살펴보자.
  
![image](https://user-images.githubusercontent.com/79847020/141426827-ef0d17b2-b0ed-43d7-9dc9-9e6a3d43885a.png)
  
블로킹 방식은 작업 처리를 요청한 후 작업이 완료될 때까지 블로킹 하지만 콜백 방식은 작업 처리를 요청한 후 결과를 기다릴 필요 없이 다른 기능을 수행할 수 있다. 그 이유는 작업 처리가 완료되면 자동적으로 콜백 메소드가 실행되어 결과를 알 수 있기 때문이다.
  
ExecutorSErvice는 콜백을 지원하지 않지만 Runnable 구현 클래스에서 콜백 기능을 구현한거나 java.nio.channels.CompletionHandler 인터페이스를 이용해도 좋다. CompletionHandler는 비동기 통신에서 콜백 객체를 만들 때 사용된다.

```JAVA
CompletionHandler<V, A> callback = new CompletionHandler<V,A>() {
      @Override
      public void completed(V result, A attachment) {}
  
      @Override
      public void failed(Throwable exc, A attachment) {}
  };
```
  
```JAVA
package thisisjava.chap12;

import java.nio.channels.CompletionHandler;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class CallbackExample {
    private ExecutorService executorService;

    public CallbackExample() {
        executorService = Executors.newFixedThreadPool(
                Runtime.getRuntime().availableProcessors()
        );
    }

    // 결과타입 : Integer, 첨부 타입 : Void
    private CompletionHandler<Integer, Void> callback = new CompletionHandler<Integer, Void>() {
        @Override
        public void completed(Integer result, Void attachment) {
            System.out.println("completed() 실행: " + result);
        }

        @Override
        public void failed(Throwable exc, Void attachment) {
            System.out.println("failed() 실행: " + exc.toString());

        }
    };

    public void doWork(final String x, final String y) {
        Runnable task = new Runnable() {
            @Override
            public void run() {
                try {
                    int intX = Integer.parseInt(x);
                    int intY = Integer.parseInt(y);

                    int result = intX + intY;
                    callback.completed(result, null);
                } catch (NumberFormatException e) {
                    callback.failed(e, null);
                }
            }
        };
        executorService.submit(task);
    }

    public void finish() {
        executorService.shutdown();
    }

    public static void main(String[] args) {
        CallbackExample example = new CallbackExample();
        example.doWork("3","3");
        example.doWork("3","삼");
        example.finish();
    }
}
```
```CONSOLE
completed() 실행: 6
failed() 실행: java.lang.NumberFormatException: For input string: "삼"
```
  
  
  





































# 프로세스 구조


![image](https://user-images.githubusercontent.com/79847020/140694043-a59deedd-0a62-4697-b28c-49ede5068291.png)

## 용어 정리

* 프로그램(Program) - 어떤 작업을 위해 운영체제 위에서 실행할 수 있는 파일, 예를 들어 웹 브라우저, 워드 프로세서, 카카오톡 등

* 프로세스(Process) - 운영 체제 위에서 실행중인 프로그램, 프로그램 명령어와 데이터들이 메모리에 올라오고 실행 중 또는 실행 대기중인 상태

* 프로세서(Processor) - 프로세스가 동작될 수 있도록 하는 하드웨어, CPU
    * 동작 : 프로그램의 자원들이 메모리에 올라오고, 실행되어야 할 코드의 메모리 주소를 CPU의 레지스터로 올리는 것

* CPU(프로세서)는 한순간에 하나의 프로세스만 실행할 수 있습니다. 운영체제가 짧은 시간에 실행할 프로세스를 교체하고 있기 때문에 우리는 동시에 여러 개의 작업이 실행되고 있다고 느끼게 됩니다.

* 




애플리케이션 - End User가 사용할 수 있는 프로그램

## Process vs Thread

* 프로세스는 별도로 할당 된 메모리 공간에서 실행된다.

* 스레드는 프로세스의 공유 메모리안에서 실행되는데, 스레드는 프로세스 내에 존재하고 모든 프로세스에는 최소 하나의 스레드(Main Thread)가 존재한다.

* 실행중인 프로그램을 프로세스라고 하고 스레드는 프로세스의 하위 집합으로 스레드는 메모리, 파일등 프로세스의 리소스를 공유한다.

* 프로세스는 프로그램 or 응용 프로그램과 동의어로 인식되지만 사용자가 하나의 응용프로그램으로 보는 것은 실제로 협력하는 여러 프로세스 일 수 있다.

* 프로세스는 작업(Task)라고 하고 스레드는 경량의 프로세스라고 한다.

* 스레드는 wait(), notify(), notifyAll()과 같은 메소드를 사용하여 동일한 프로세스의 다른 스레드와 직접 통신 할 수 있다. 
* 
* 프로세스는 프로세스 간 통신을 사용하여 다른 프로세스와 통신 할 수 있다.

* 새로운 스레드는 자바에서 쉽게 생성된다.(스레드 start), 그러나 새 프로세스를 생성하려면 상위 프로세스를 복제해야 한다.

 

스레드는 동일한 프로세스의 다른 스레드를 제어하지만 프로세스는 형제 프로세스를 제어하지 않고 하위 프로세스 만 제어한다.

자바에서 메모리 관리 및 신호 처리(Signal Handling)와 같은 작업을 수행하는 System Thread까지 포함하면 여러 스레드가 있지만 자바프로그램의 관점에서 보면 단일 프로세스, main Thread로 시작되며 ProcessBuilder를 이용하여 추가 프로세스를 생성 할 수 있다.

프로세스에는 여러 스레드가 포함될 수 있다.

애플리케이션을 실행하면 OS가 새 프로세스를 시작하고 다른 스레드를 시작할 수 있는 해당 앱의 기본 스레드를 실행한다.(자바프로그램의 경우 main Thread)

멀티 스레딩의 장점은 응용 프로그램이 동시에 다른 작업을 실행할 수 있다는 것이다.

프로세스에는 고유 한 주소 공간이 있는 반면 동일한 프로세스의 스레드는 프로세스안의 공유 메모리 공간에서 실행되므로 데이터의 공유는 신중하게 접근해야 한다. (동일한 변수에 대한 읽기/쓰기)

 

2. 스레드

실행중인 프로그램 내의 순차적 제어 흐름

자바 프로그램이 시작되면 JVM의 main 스레드가 main() 메소드를 실행한다. 또한 프로그램 내부에서 main 스레드와 병렬로 프로그램 코드의 일부를 실행할 수 있는 더 많은 스레드를 만들 수 있다.

Java 스레드는 다른 자바 객체와 같은 객체로, java.lang.Thread 클래스의 인스턴스 또는이 클래스의 서브 클래스 인스턴스 이다. 

자바에서 스레드를 만드는 방법은  Thread 클래스를 상속받는 클래스를 만들고 run() 메소드를 구현하여 스레드가 할 일을 작성한다음 인스턴스를 생성 후 start() 메소드를 호출하는 방법이 있고 runnable() 인터페이스 구현 객체를  Thread 인스턴스의 인자로 주고 스레드를 만들수도 있다.

다중 스레드 프로그램

동시에(concurrently) 여러가지 작업을 할 수 있음

프로그램 내부에 2개 이상의 스레드들이 서로 침범하지 않으면서도 동시에 실행의 흐름이 진행되는 프로그램

 

a와 b라는 스레드가 있을때 a스레드가 진행되면서 CPU의 상태가 바뀌게 되는데 이를 저장하고 b의 스레드에 맞게 CPU를 맞추고 b스레드를 처리하는 것이다. 물론 b처리후 얼마후 다시 a가 처리된다. 이때 a와 b가 같은 프로그램이면 멀티 스레드이며 서로 다른 프로그램 일 경우에는 멀티태스킹이 된다.

CPU가 하나인 컴퓨터의 입장에서는 여러 개의 흐름이 동시에 돌아간다는 것은 불가능 하다. 단지 OS가 여러 개의 흐름이 돌아 가는 것처럼 보일 수 있도록 CPU등의 환경 상태를 각각의 흐름에 맞게  설정 하는 것이다.

 

자바 언어가 기존 언어와 구분되는 큰 차이는 Thread를 언어 차원에는 지원 하는 것이다.

쉽게 스레드를 관리 할 수 있게 해 준다. CPU가 여러 개인 경우 각각의 CPU에서 개별적인 프로그램 흐름을 만들 수 있게 해 준다. 즉 CPU가 2개 이면 2개의 스레드가 상태 변환 없이 각각의 CPU에서 돌아가게 되는 것 이다.

프로세스는 실행 환경과 제어환경으로 이루어져 있는데 스레드는 제어부에 해당 된다. 기본적으로 하나의 프로그램이 실행 되는 것을 프로세스가 발생 했다고 표현하며 이 프로세스는 또 다시 여러 개의 스레드를 거느릴 수 있는데 프로세스 간의 직접 상태 변환보다는 이들 프로세스들이 거느리고 있는 스레드에게 제어권을 맡김 으로서 상태 변환에 따르는 부담을 줄인다. (한 프로세스 내의 스레드들이 같은 메모리 공간을 사용)

스레드의 최대 장점은 프로세스 내의 정보들이 공유가 쉽다는 것이다. 물론 여러 개의 프로세스로 같은 작업을 하더라도 프로세스간의 통신을 통해 정보를 공유 할 수 있지만 스레드로서 공유하는 것과는 차이가 있다.

장점

실행 속도

빠른 반응 시간

 

동시성을 가지는 프로그램을 쉽게 작성

자바 툴을 이용하여 자바 프로그램을 실행했다면 이것은 프로세스를 실행 시킨 것 이다. 하지만 자바 프로그램의 main() 메소드가 호출되서 프로그램이 돌아가기 시작하는 것은 스레드가 돌아간다는 뜻이다.

모든 실행의 단위는 프로세스 안의 스레드 이다.

자바 프로그램을 실행하게 되면 메인 스레드가 main 메소드를 호출해 주는 것으로 프로그램이 시작된다.

 

 

3. 실습

 

//결과[스레드이름,우선순위,그룹명]:Thread[main, 5, main]

//이 스레드는 main이라는 이름을 가졌으며 5라는 우선순위를 가진다.

class MainThread {

    public static void main(String[] args) {

        Thread currThread = Thread.currentThread();

        System.out.println(currThread);

    }

}
