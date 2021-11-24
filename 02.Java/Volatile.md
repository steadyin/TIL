# Volatile 변수란 ?

Java의 Volatile 키워드는 Java 변수를 "메인 메모리에 저장 중"으로 표시하는 데 사용된다. 좀 더 정확히 말하자면, Volatile 변수의 모든 읽기/쓰기는 컴퓨터의 메인 메모리에서 읽혀지고 쓰여질 것이다. 
단지 CPU캐시에서는 메인메모리와 연결만 될뿐이다. 

* Java에서 Volatile 키워드는 멀티스레드 환경에서 변수를 CPU 캐시를 거치지 않고 메인 메모리에 직접 읽고 쓰겠다는 것을 의미합니다.
* Primitive 타입과 Reference 타입 모두 사용 가능합니다.

# 변수 가시성 문제(Variable Visibility Problems)

Java의 Volatile 키워드는 스레드 전체에 걸쳐 해당 변수의 변경을 볼 수 있도록 보장한다.
Volatile 변수에 대해 스레드가 동작하는 멀티스레드 환경에서 각 스레드는 변수를 성능상의 이유로 메인메모리에서 CPU 캐시로, 


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
