 
JAVA7 과 JAVA 8의 메모리 구조 차이를 알아보기 전에 JAVA 7 메모리 구조를 살펴보자.

## JAVA 7 메모리 구조

<img src="https://user-images.githubusercontent.com/79847020/136273887-2f2ab0dd-0c71-4081-8270-de04d518a7be.png" width="70%" height="70%">

* Permgen 영역

    Perm 영역은 보통 Class의 Meta 정보나 Method의 Meta 정보, Static 변수와 상수 Pool 정보들이 저장되는 공간
    
    흔히 메타데이터 저장 영역이라고도 한다. 

    -XX:PermSize -XX:MaxPermSize 옵션으로 크기를 지정할 수 있습니다.

* Java Heap 영역

    Perm 영역에 존재하는 클래스의 인스턴스 객체를 저장하는 공간입니다.
    
    소스코드에서 new 연산자나 배열 등이 Heap 영역에 저장됩니다.
    
    JVM 내에서 공유되는 공유 자원이며, 각 스레드에서 접근이 가능하기 때문에 스레드 동기화 작업이 이루어집니다.

    -XX:MetaspaceSize -XX:MaxMetaspaceSize 옵션으로 크기를 지정할 수 있습니다.

* Native 영역

    Native 영역은 JVM 메모리에 포함되지 않는 OS에서 동적으로 할당하는 메모리 영역이다. 

---

## JAVA 8 메모리 구조

<img src="https://user-images.githubusercontent.com/79847020/136274187-88c80661-ce94-4007-82b1-7c89453fafd6.png" width="70%" height="70%">

JAVA 8 부터는 Permgen 영역이 사라지고 Metaspace 영역이 새로 할당되었다.

Metaspace 영역은 Permgen 영역의 역할을 수행하지만 OS에서 관리하는 Native 영역으로 이동했고 

Static Object은 더이상 Metaspace에서 관리하지 않고 Heap 영역으로 옮겨져 GC에 대상이 되도록 했다는 차이가 있다.

* JAVA 7, 8 비교표

    |구분|상세구분|Perm|Metaspace|
    |---|---|---|---|
    |저장정보|Class 메타정보|저장|저장|
    |-|Method 메타정보|저장|저장|
    |-|Static 변수,상수|저장|Heap영역|
    |메모리 튜닝|-|Heap, Perm 영역 튜닝|Heap 영역 튜닝<br>Native 영역은 OS 동적조정<br>(더 큰 메모리 사용 가능)|
    |메모리 옵션|-|-XX:PermSize<br>-XX:MaxPermSize|-XX:MetaspaceSize<br>-XX:MaxMetaspaceSize|

Metaspace 영역은 Native 메모리로 다루기 때문에 기본적으로 JVM에 의해 크기가 강제되지 않는다.

만약 Classloader에 메모리 누수가 의심되는 경우 -XX:MaxMetaspaceSize를 지정할 필요성이 있다. 

왜냐하면 최대 값을 정해놓지 않는다면 Metaspace 영역의 메모리가 계속해서 커질 것이고, 결국 문제가 발생할 것이다.

