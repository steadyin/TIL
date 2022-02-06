# Volatile 변수란 ?

Java의 Volatile 키워드는 Java 변수를 "메인 메모리에 저장되는 것"으로 표시하는 데 사용된다. 좀 더 정확히 말하자면, Volatile 변수의 모든 읽기/쓰기는 컴퓨터의 메인 메모리에서 읽혀지고 쓰여질 것이다. 단지 CPU캐시에서는 메인메모리와 연결만 될뿐이다. 

* Java에서 Volatile 키워드는 멀티 스레드 환경에서 변수를 CPU 캐시가 아닌 메인 메모리에 직접 읽고 쓰겠다는 것을 의미합니다.
* Primitive 타입과 Reference 타입 모두 사용 가능합니다.

# 변수 가시성 문제(Variable Visibility Problems)

Java의 Volatile 키워드는 스레드 전체에서 변수에 대한 변경 사항을 볼 수 있도록 보장합니다. 

Non-Volatile 변수는 멀티 스레드 환경에서 각 스레드는 성능상의 이유로 메인 메모리에서 CPU 캐시로 데이터를 복사합니다. 멀티 코어 환경에서는 각 스레드가 서로 다른 CPU에서 실행 될 수 있습니다. 이런 경우 각 스레드가 변수를 복사하여 서로 다른 CPU의 CPU 캐시에 복사를 하는 케이스가 발생합니다. 

![image](https://user-images.githubusercontent.com/79847020/152637805-71a6347c-ff4f-444e-adbf-f2ad2400ef0c.png)

Non-Volatile 변수를 사용하면 JVM(Java Virtual Machine)이 메인 메모리에서 CPU 캐시로 데이터를 읽거나 CPU 캐시에서 메인 메모리로 데이터를 쓰는 시기에 대해 알 수 없습니다. 이로 인해 다음 섹션에서 설명할 몇 가지 문제가 발생할 수 있습니다. 

두 개 이상의 스레드가 다음과 같이 카운터 변수를 포함하는 공유 객체에 액세스할 수 있는 상황을 봅시다.

```JAVA
public class SharedObject {

    public int counter = 0;

}
```

스레드1과 스레드2은 카운터를 읽을 수 있고 스레드1은 추가로 카운터를 증가시키는 상황이 있습니다. 만일 Counter 변수에 Volatile 키워드가 적용되있지 않다면 언제 Counter변수가 CPU 캐시에서 메인 메모리로 Write 될 지 보장할 수 없습니다. CPU 캐시의 Counter 변수와 메인 메모리의 Counter 변수가 다른 값을 가질 수 있는 것입니다. 

![image](https://user-images.githubusercontent.com/79847020/152638046-06ce4413-b2f8-42e3-b88f-7e638fdb6912.png)

스레드에서 변경한 값이 메인 메모리에 저장되지 않아서 다른 스레드에서 이 값을 확인할 수 없는 것을 '가시성' 문제 라고 합니다. 한 스레드에서 변경이 다른 스레드에서 보이지 않습니다.

만약 Volatile 키워드를 적용한다면 이 변수에 대한 Write 작업은 바로 메인 메모리에서 이루어질 것이고 읽기 작업 또한 메인 메모로부터 읽어질 것 입니다.

```JAVA
public class SharedObject {

    public volatile int counter = 0;

}
```

Volatile 키워드는 다른 스레드의 Write 작업에 대해 가시성을 보장합니다.

# 완전 가시성 보장 (Full Visibility Guarantee)

JAVA 5 부터 Volatile 키워드는 변수에 대한 읽기/쓰기 작업의 메인 메모리 사용을 보장하는 것 이상을 보장합니다.

* 스레드 A가 Volatile 변수를 사용하고 스레드 B가 이후에 Volatile 변수를 읽는 경우 Volatile 변수에 쓰기 전에 스레드 A가 볼 수 있었던 모든 변수는 스레드 B가 Volatile 변수를 읽은 후에도 볼 수 있습니다.
* 스레드 A가 Volatile 변수를 읽으면 Volatile 변수를 읽을 때 스레드 A가 볼 수 있는 모든 변수도 메인 메모리에서 다시 읽습니다.

이 말은 스레드가 Volatile 변수를 Write할 때 Volatile 변수만 메인 메모리로 저장되는 것이 아니라 이 스레드에서 수정한 모든 변수들이 함께 메인 메모리에 Write 저장됩니다. 그리고 스레드가 Volatile 변수를 메인 메모리에서 Read할 때 Volatile 변수를 Write하면서 메인 메모리에 함께 저장된 다른 변수들도 메인 메모리로부터 읽어들여집니다.

```JAVA
public class MyClass {
    private int years;
    private int months
    private volatile int days;


    public void update(int years, int months, int days){
        this.years  = years;
        this.months = months;
        this.days   = days;
    }
}
```

update() 메서드는 3개의 변수를 Write하며 그 중 Volatile 변수는 days입니다. 완전한 가시성 보장은 days가 기록될 때 스레드에서 볼 수 있는 다른 모든 변수들도 메인 메모리에 Write 된다는 것을 의미합니다. 즉 days가 Write되면 years, months도 메인 메모리에 Write됩니다. 

년, 월, 일의 값을 읽을 때 다음과 같이 작성할 수 있습니다.

```JAVA
public class MyClass {
    private int years;
    private int months
    private volatile int days;

    public int totalDays() {
        int total = this.days;
        total += months * 30;
        total += years * 365;
        return total;
    }

    public void update(int years, int months, int days){
        this.years  = years;
        this.months = months;
        this.days   = days;
    }
}
```

totalDays() 메서드는 날짜 값을 total 변수로 읽어 오는 것으로 시작합니다. days의 값을 읽을 때 months, years 값도 메인 메모리로 읽어들입니다. 따라서 days, months, years를 메인 메모리에서 최신 값을 볼 수 있습니다.

# 명령어 재정렬 문제

JVM과 CPU는 성능상의 이유로 수행되는 명령어들을 재정렬할 수 있습니다. 다음 명령어 살펴보자.

```JAVA
int a = 1;
int b = 2;

a++;
b++;
```

이 명령어들은 JVM에서 동일한 결과를 가져오는 다음 명령어로 재정렬됩니다.

```JAVA
int a = 1;
a++;

int b = 2;
b++;
```

이 같은 예제에서는 특별한 문제가 발생하지 않지만 변수 중 하나라도 Volatile 변수인 경우 명령어 재정렬은 문제가 될 수 있습니다. MyClass를 다시 살펴보겠습니다.

```JAVA
public class MyClass {
    private int years;
    private int months
    private volatile int days;


    public void update(int years, int months, int days){
        this.years  = years;
        this.months = months;
        this.days   = days;
    }
}
```

update() 메소드가 days값을 write하면 새로 작성 된 years, months도 메인 메모리에 write됩니다. 하지만 JVM 다음과 같이 명령을 재정렬하면 어떻게 될지 생각해보자.

```JAVA
public void update(int years, int months, int days){
    this.days   = days;
    this.months = months;
    this.years  = years;
}
```

전에는 days 변수가 write 될 때 months, years 값은 메인 메모리에 기록되지만 이번에는 months, years 값이 write 되기 전에 발생합니다. 따라서 다른 스레드에서 months, years의 정확한 값이 보장되지 않습니다. 재정렬된 명령이 다른 결과를 가져오는 것입니다. 

# Volatile 키워드의 Happens-Before 보증

명령 재정렬 문제를 해결하기 위해 Volatile 키워드는 가시성 보장과 이전 'Happens-Before'을 보장합니다. 

* 읽기/쓰기가 원래 Volatile 변수에 쓰기 전에 발생한 경우 다른 변수에 대한 읽기 및 쓰기는 Volatile 변수에 대한 쓰기 후에 발생하도록 재정렬할 수 없습니다. Volatile 변수에 쓰기 전에 읽기/쓰기는 Volatile 변수에 쓰기 "전에 발생"하도록 보장됩니다. 예를 들어 여전히 가능합니다. Volatile에 대한 쓰기 이전에 발생하도록 재정렬될 Volatile에 대한 쓰기 후에 위치한 다른 변수의 읽기/쓰기. 그 반대가 아닙니다. 이후에서 이전으로 허용되지만 이전에서 이후로 허용되지 않습니다.

* `읽기/쓰기가 원래 Volatile 변수를 읽은 후에 발생한 경우 다른 변수에서 읽고 쓰는 것은 Volatile 변수를 읽기 전에 발생하도록 재정렬할 수 없습니다. Volatile 변수 읽기 전에 발생하는 다른 변수 읽기는 Volatile 변수 읽기 후에 발생하도록 재정렬될 수 있습니다. 그 반대가 아닙니다. 이전에서 이후로 허용되지만 이후에서 이전으로 허용되지 않습니다.(여기 번역 오류 이상!!!!!)`

위의 발생 전 보장은 volatile 키워드의 가시성 보장이 시행되고 있음을 보장합니다.

# volatile 은 만병통치약이 아니다

![image](https://user-images.githubusercontent.com/79847020/152639679-c4c71977-a9d3-4f9c-a275-418f612ced10.png)

volatile 선언이 변수의 읽기/쓰기 명령을 메인 메모리로부터 수행한다는 것을 보장한다고 할지라도, volatile 선언으로 해결할 수 없는 상황들은 여전히 남아있다.

위의 예제들 중, 공유 변수인 counter 가 있고 Thread1 만이 이 변수를 수정하고, Thread2 만이 이 변수를 읽는 이런 상황에서라면 volatile 선언이 변수의 가시성을 보장해준다.

멀티쓰레드 환경에서, volatile 공유 변수에 세팅된 새로운 값이 이 변수가 가지고 있던 이전의 값에 의존적이지 않는다면, 다수의 쓰레드들이 volatile 공유 변수를 수정하면서도 메인 메모리에 존재하는 정확한 값을 읽을 수 있다. 달리 말하자면, 만일 volatile 공유 변수를 수정하는 한 쓰레드가 이 변수의 다음 값을 알아내기 위해 이전의 값을 필요로 하지 않는다면 말이다. 메인 메모리에 바로 WRITE하므로

쓰레드가 volatile 변수의 초기값을 필요로 할 때, 그리고 volatile 변수의 새 값이 이 초기값을 근거로 할 때, volatile 선언은 더이상 정확한 가시성을 보장하기 못한다. volatile 변수를 읽고, 새 값을 쓰는 사이의 짧은 갭은 경합 조건을 일으킨다 - 다수의 쓰레드가 volatile 변수의 값을 똑같이 읽고, 새 값을 생성하여 이를 메인 메모리로 저장하는 동안, 서로의 값을 덮어쓸 수 있다.

다수의 쓰레드가 같은 counter 값을 증가시키는 상황이 바로 volatile 변수가 불완전해지는 상황이다. 다음 섹션에서 이에 대해 자세히 설명한다.

상황을 가정해보자. Thread1 이 공유 변수인 counter 의 값 0 을 자신의 CPU 캐시에 읽어들이고, 이 값을 1 증가시키고, 아직 메인 메모리로 저장하지 않았다. 그리고 Thread2 는 같은 counter 변수를 메인 메모리에서 자신의 CPU 캐시로 읽어들였고, 이 때의 값은 여전히 0 이다. 그리고 Thread2 도 이 값을 1 증가시키고 메인 메모리로 저장하지는 않았다. 다음은 이 상황을 묘사한 다이어그램이다.

# Volatile 을 사용하기 적절한 때는?

앞서 설명했듯, 두 스레드가 공유 변수에 읽기/쓰기 를 실행할 때, volatile 선언은 충분치 않다. 이런 상황에서는 변수값의 읽기/쓰기 명령의 원자성을 보장하기 위해 Synchronized 를 써야한다. 변수를 읽고 쓸 때 Volatile 선언은 변수에 접근하는 스레드들을 블록시키지 않는다. 이런 임계 영역에는 Synchronized 키워드가 필요하다.

Synchronized 블록을 대체하는 다른 것을 찾는다면, java.util.concurrent 패키지의 많은 원자성 데이터 타입들을 사용할 수도 있다. 에를 들자면 AtomicL Long 이나 Atomic Reference 와 같은 것들이다.

한 변수를 두고 오직 한 쓰레드만 이 변수에 읽기/쓰기 작업을 하고, 다른 쓰레드들은 읽기 작업만 하는 상황에서라면 이 때는 Volatile 선언이 유효하다. 읽기 작업을 수행하는 쓰레드들은 언제나 이 변수의 가장 최근 수정된 값을 봐야하고, Volatile 은 이를 보장해준다.

그리고 volatile 은 32비트와 64비트 변수에서 효과를 볼 수 있다.

# volatile 의 성능에 대한 고찰

volatile 변수의 읽기/쓰기는 메인 메모리를 이용한다. 메인 메모리로부터 데이터를 읽고 쓰는 작업은 CPU 캐시를 이용하는 것 보다 많은 비용이 요구된다. 또한 volatile 선언은 JVM 의 성능 향상을 위한 기술인, 코드 재정리를 막기도 한다. 그러므로 volatile 키워드는 변수의 가시성 보장이 반드시 필요한 경우에만 사용되어야 한다.


출처 : http://tutorials.jenkov.com/java-concurrency/volatile.html, https://parkcheolu.tistory.com/16
