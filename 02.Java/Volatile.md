# Volatile 변수란 ?

Java의 Volatile 키워드는 Java 변수를 "메인 메모리에 저장되는 것"으로 표시하는 데 사용된다. 좀 더 정확히 말하자면, Volatile 변수의 모든 읽기/쓰기는 컴퓨터의 메인 메모리에서 읽혀지고 쓰여질 것이다. 단지 CPU캐시에서는 메인메모리와 연결만 될뿐이다. 

* Java에서 Volatile 키워드는 멀티 스레드 환경에서 변수를 CPU 캐시가 아닌 메인 메모리에 직접 읽고 쓰겠다는 것을 의미합니다.
* Primitive 타입과 Reference 타입 모두 사용 가능합니다.

# 변수 가시성 문제(Variable Visibility Problems)

Java의 Volatile 키워드는 스레드 전체에서 변수에 대한 변경 사항을 볼 수 있도록 보장합니다. 

Non-Volatile 변수는 멀티 스레드 환경에서 각 스레드는 성능상의 이유로 메인 메모리에서 CPU 캐시로 데이터를 복사합니다. 멀티 코어 환경에서는 각 스레드가 서로 다른 CPU에서 실행 될 수 있씁니다. 이런 경우 각 스레드가 변수를 복사하여 서로 다른 CPU의 CPU 캐시에 복사를 하는 케이스가 발생합니다. 

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

# Volatile 키워드의 Happens-Before 보장

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









## Java5 이후 변화

* Volatile 변수를 사용할 경우, 변수 접근 코드는 메모리 장벽 코드를 생성합니다. 즉, Volatile 변수를 사용할 경우, 해당 변수에 접근하는 코드에 대해서는 최적화를 수행하지 않습니다.

## Volatile 동작

![image](https://user-images.githubusercontent.com/79847020/141278302-aef9b05f-c28d-4cd6-ae7a-61e4bc8f3f0f.png)

* 멀티스레드 환경에서는 성능 향상을 위해서 Main Memory에 있는 정보들을 CPU Catch에 복사하고 

volatile 변수를 사용하고 있지 않는 MultiThread 어플리케이션에서는 Task를 수행하는 동안 성능 향상을 위해 Main Memory에서 읽은 변수 값을 CPU Cache에 저장하게 됩니다.
만약에 Multi Thread환경에서 Thread가 변수 값을 읽어올 때 각각의 CPU Cache에 저장된 값이 다르기 때문에 변수 값 불일치 문제가 발생하게 됩니다.



Voltei


Vo

출처 : http://tutorials.jenkov.com/java-concurrency/volatile.html 

기존에 살펴보았던 스레드의 구조 이다. 

![image](https://user-images.githubusercontent.com/79847020/141274978-ca54f28f-ca38-41ab-8a3d-c8ded0db5f27.png)

volatile 변수를 읽어 들일 때 CPU 캐시가 아니라 컴퓨터의 메인 메모리로 부터 읽어들입니다. 그리고 volatile 변수를 쓸 때에도(write) CPU 캐시가 아닌 메인 메모리에 기록합니다.


Java volatile은 변수의 가시성(Visibility)을 보장한다.

Java volatile 키워드는 여러개의 쓰래드들 에서 사용되는 변수의 변화(값의 변화) 에 대한 가시성의 보장합니다.

non-volatile 변수들로 운영되는 멀티 쓰래드 어플리케이션에서 각 쓰래드들은 성능적이 이유로 메인 메모리로 부터 변수를 읽어 CPU 캐시에 복사하고 작업하게 됩니다.

만약 여러분의 컴퓨터가 하나 이상의 CPU로 구성되어 있고 각 쓰래드들이 서로 다른 CPU에서 실행 될수 있습니다.

이 말은 각 쓰래드들이 서로 다른 CPU들의 CPU 캐시에 값을 복사할 수 있다는 것으로 아래의 다이어그램이 이를 설명해주고 있습니다.

non-volatile 변수들은 어느 시점에 Java Virtual Machine(JVM)이 메인 메모리로 부터 데이터를 읽어 CPU 캐시로 읽어 들이거나 혹은 CPU 캐시들에서 메인 메모리로 데이터를 쓰는지(write) 보장해 줄 수 없습니다.

이럴 경우 어떤 문제가 발생 할 수 있는지 예를 들어 보겠습니다.
