2. 자바의 GC는 어떻게 동작하는지, 메모리구조와 어떤 연관이 있는지

4. static 키워드를 사용함으로 인해 memory leak이 발생하는 케이스는 어떤 게 있을지.

4.5 static block을 사용하지 말라고 하는 이유?

5. hashCode와 equals의 상관관계, hashCode는 언제 쓰게될까?

------------------------------------------------------------------------



# 1. JVM vs JRE vs JDK
<img src="https://user-images.githubusercontent.com/79847020/135798876-a603d2fe-b05d-41d6-96f0-565f5dda35b0.png" width="60%" height="40%">
* 간단하게 말하면 JDK는 소프트웨어 개발 키트, JRE는 자바 프로그램을 실행할 수 있는 소프트웨어 번들, JVM은 바이트코드를 실행하는 환경을 말한다. 

## 1.1 JVM ?
* JVM은 Java Virtual Machine의 줄임말로 Java 프로그램이 플랫폼(OS, 하드웨어)에 종속되지 않고 일관되게 구동될 수 있도록 런타임 환경을 제공하는 엔진이다. 
* Java 바이트코드를 플랫폼(OS,하드웨어)이 이해할 수 있는 기계어로 변역한다. 운영체제와 바이트코드간의 중계자 역할을 한다고 보면 된다.
* JDK, JRE에 포함되어 있다.
* GC(Garbase Collection)을 이용하여 자동으로 메모리 관리를 해준다.
* 표준이 존재하지만 벤더 사마다 차이가 있다.

## 1.2 JRE ?
* JRE는 Java Runtime Environment은 Java 프로그램을 실행되게하고 필요한 구성요소를 제공하는 소프트웨어 번들이다.
* JVM를 포함한다.
* JRE는 클래스 라이브러리, JVM, JDBC(Java Database Connectivity), 원격 메서드 호출(RMI), Java Naming and Directory Interface(JNDI) 등과 같은 라이브러리를 포함한다. 
(컴파일러, 디버거같은 개발 도구는 포함되지 않는다.)
* Math, Swing, Util, Lang, Awt, 런타임 라이브러리가 같은 중요 패키지 클래스가 포함된다.

<img src="https://user-images.githubusercontent.com/79847020/136079130-6e66243a-93aa-4f7f-8279-e6a04d48c8fa.png" width="60%" height="300px">

* JRE의 중요 구성 요소와 동작순서는 다음과 같다.
    * ClassLoader : 클래스로더는 Java 프로그램 실행에 필요한 다양한 클래스를 로드한다.
    * Bytecode Verifier : 바이트코드 검증기는 바이트코드를 검증한다.
    * Interpreter : 클래스가 로드되고 바이트코드가 확인되면 한줄씩 코드를 읽는다.
    * Runtime : 런타임은 특정 프로그램이 실행되는 단계이다.
    * Hardware : Java Native 코드를 컴파일하면 특정 하드웨어 플랫폼에서 실행된다.
  
## 1.3 JDK ?
* JDK는 Java Development Kit, 자바 애플리케이션을 개발하는데 사용되는 개발 도구를 말한다. 
* 컴파일러, 역어셈블러, 디버거, 의존관계 분석 등 개발에 필요한 도구를 제공한다. 
* JVM, JRE를 포함한다.
* 컴파일러는 말그대로 자바로 작성된 코드를 바이트 코드로 변환한다.

---
    
# 2. JVM의 사용목적

<img src="https://user-images.githubusercontent.com/79847020/135797267-f031d44a-98a4-4884-aac8-b7c5f1e8e610.png" width="50%" height="50%">

C/C++는 컴파일 플랫폼과 타겟 플랫폼이 다를 경우, 프로그램이 동작하지 않는다.
이를 크로스 컴파일(Cross Compile, 타겟 플랫폼에 맞춰 컴파일하는 것)로 해결한다. 
때문에 사용자가 자신의 플랫폼에 맞는 배포 버전을 찾아 설치를 진행한다. 

JAVA는 이 문제를 JVM으로 해결한다. 
자바 바이트코드는 타겟 플랫폼과 상관없이 JVM 위에서 동작한다.
물론 JVM은 타겟 플랫폼에 의존하고, 사용자는 JRE를 각 플랫폼에 맞게 설치를 해야한다.

* WORA(Write Once, Run Anywhere)

크로스 컴파일을 도입하지 않고 굳이 JVM을 도입한 이유는 무엇일까? <br>
자바는 네트워크에 연결 된 모든 디바이스에서 동작하는 것이 목적이었다. 웹 서버에서 컴파일 된 바이트코드를 전송해주면 브라우저에 JVM 내장되어 동작하길 원했다. <br>
갈수록 디바이스가 다양해지고 디바이스 마다 운영체제나 하드웨어가 다르기 때문에 플랫폼에 의존하지 않은 언어를 설계했다.

그 결과가 **<U>Bytecode, JVM</U>** 이다.

<img src="https://user-images.githubusercontent.com/79847020/135797687-a44afde7-d9b5-44c4-9a95-c159c3ee80e0.png" width="40%" height="40%">

---

# 3. JAVA 컴파일

컴파일러(javac)가 자바소스코드(.java)를 컴파일(Complile)해서 자바 바이트코드를 생성한다.(.class)

```CONSOLE
> javac Sourcecode.java
```

javac 명령어의 옵션은 다음과 같다.
|옵션|설명|예제|
|---|---|---|
|-classpath, -cp|클래스패스, 즉 실행할 클래스의 위치를 지정한다.|javac -cp "/Users/home/A.java"|
|-d|어디에 클래스파일을 생성할지 지정한다.|javac -d "/User/home/path"|
|-encoding|소스 파일에 사용된 인코딩을 지정한다.|javac -encoding "uft-8" A.java|
|-g|모든 디버깅 정보를 출력|javac -g ~~~|
|-verbose|컴파일러가 진행하는 작업을 모두 출력한다.|javac -verbose ~~~|
|-sourcepath|소스파일 위치 지정|javac -sourcepath "/User/home/path"|
|-source|소스파일 자바 버전 지정|javac -source 1.8 ~~~|
|-target|타겟파일 자바 버전 지정|javac -target 1.8 ~~~|

<img src="https://user-images.githubusercontent.com/79847020/135797749-970eda38-4a4c-49a4-8751-fdba44865916.png" width="50%" height="40%">

---

# 4. JAVA 실행

```CONSOLE
> java Sourcecode.class
```

자바실행기(java)가 바이트코드(.class)를 실행한다. 

내부적으로는 다음과 같이 동작한다.

<img src="https://user-images.githubusercontent.com/79847020/135800301-bb85c404-ac6d-4c58-bf22-22b072c86e8f.png" width="50%" height="40%">

실행엔진으로 클래스 로드하는것을 클래스로더가 담당한다.

컴파일 된 .CLASS 파일을 로드 후  JVM의 일부인 클래스 파일, 예를 들면 내장 클래스(문자열, 컬렉션 클래스 등)과 같은 클래스 파일의 바이트 코드를 클래스 로더가 로드한다. 이 바이트코드는 실행 엔진에 공급되고 실행 엔진은 바이트코드 실행을 담당하므로 실행 엔진은 운영체제와 통신해야 합니다.

마지막으로 기계 명령어 세트에 대한 명령어를 실행 엔진은 네이티브 메서드 호출을 사용하고 마지막으로 수행을 담당하는 기계 코드 값으로 변환됩니다.

---

# 5. Class Loader
        
<img src="https://user-images.githubusercontent.com/79847020/135800488-13697543-451b-490d-a630-23b6ca56aa6a.png" width="50%" height="40%">

컴파일 된 .class클래스파일들을 엮어서 JVM이 운영체제로부터 할당받은 메모리영역인 Runtime Data Area로 적재하는 역할을 Class Loader가 한다. <br>
(자바 애플리케이션이 실행중일 때 이런 작업이 수행된다.)    

## 5.1 로드(Load)

Bootstrap, Extension, Application - 이 컴포넌트들에 의해 클래스들이 로드된다. 클래스 로더들이면서 이 세가지 클래스로더에 의해 클래스가 로드됩니다. 이 세가지 클래스 로더들은 모두 상속관계로 정의되어 있으며 delegate(위임) 방식으로 작업을 진행합니다. 

### 5.1.1 Bootstrap ClassLoader 

jre의 lib폴더에 있는 rt.jar 파일을 뒤져 기본 자바 API 라이브러리를 로드합니다. 가장 최우선으로 로드됩니다.

### 5.1.2 Extension ClassLoader 

jre의 lib 폴더에 있는 ext 폴더에 있는 모든 확장 코어 클래스파일들을 로드합니다. 최근에는 Platform ClassLoader라고 부르기도 합니다. Bootstrap ClassLoader의 child 입니다. Extension 클래스 로더는 jdk 확장 디렉토리(JAVA_HOME/lib/ext 디렉토리 혹은 java.ext.dirs 에 저장된 경로)에서 로드됩니다.

### 5.1.3 Application ClassLoader 

Extension ClassLoader의 child이며 시스템 클래스로더(System ClassLoader)라고도 불립니다. 어플리케이션 레벨에 있는 클래스들을 로드합니다. 즉, 사용자가 직접 정의한 클래스파일들을 로드합니다. Classpath 환경변수에 있는 클래스 파일이나 -classpath 또는 -cp 명령어 옵션이 있는 파일들을 로드합니다.

## 5.2 연결(Link)

### 5.2.1 검증(verify) 

바이트코드 검증기는 생성된 자바 바이트코드가 적절한지 아닌지에 대해서 검증하며 검증이 실패할 경우 검증오류를 내보내게 됩니다.

### 5.2.2 준비(prepare) 
 
모든 정적변수의 메모리가 할당되며 기본 default 값으로 할당됩니다.(아직 초기화되지는 않음)

### 5.2.3 해석(resolve) 
 
모든 심볼릭한(명확하게 정의되지 않은) 메모리 참조를 메소드 영역에 있는 타입으로 직접 참조합니다.

## 5.3 초기화(initialize)

클래스 로딩의 마지막 단계로써 모든 정적 변수가 자바 코드에 명시된 값으로 초기화되며 정적블록이 실행됩니다.
    
# 6. Runtime Data Area

<img src="https://user-images.githubusercontent.com/79847020/135800271-089cdb07-10a2-4ba6-b345-95d54d777c61.png" width="50%" height="60%">

Class Loader에 의해 메모리에 적재된 클래스(바이트 코드)들을 기계어로 변경해 명령어 단위로 실행하는 역할을 한다. 

바이트코드를 실행하는 방법은 2가지가 있는데 Interpreter 방식과 JIT컴파일러 방식이 존재한다. 인터프리터 방식은 명령어를 하나씩 해석해서 처리하는 개념이기 때문에 속도가 느린 단점이 있다. 이를 개선하기 위해 나온 것이 JIT 컴파일러이다. JIT 컴파일러는 런타임 시 클래스파일(바이트코드)를 네이티브 기계어로 한번에 컴파일 후 사용하는 개념이다. 

최초 JVM이 나왔을 당시에는 인터프리터 방식이었기때문에 속도가 느리다는 단점이 있었지만 JIT 컴파일러 방식을 통해 이 점을 보완하였습니다. JIT는 바이트코드를 어셈블러 같은 네이티브 코드로 바꿈으로써 실행이 빠르지만 역시 변환하는데 비용이 발생합니다. 이같은 이유로 JVM은 모든 코드를 JIT 컴파일러 방식으로 실행하지 않고, 인터프리터 방식을 사용하다가 일정한 기준이 넘어가면 JIT 컴파일러 방식으로 실행합니다.

실행 데이터 영역은 5가지 영역으로 구분할 수 있다.

## 6.1. 메서드 영역(Method Area)
   
모든 클래스 수준의 데이터가 저장됩니다. 실제 클래스 인스턴스가 존재하는 것이 아닌 참조하기 위한 클래스 정보를 갖고 있는 영역입니다. 
클래스 멤버 변수의 이름, 데이터 타입, 접근 제어자 정보같은 필드 정보와 메소드의 이름, 리턴 타입, 파라미터, 접근 제어자 정보같은 메소드 정보, Type정보(Interface인지 class인지), Constant Pool(상수 풀 : 문자 상수, 타입, 필드, 객체 참조가 저장됨), static 변수, final class 변수등이 생성되는 영역이다. 공유자원이며 JVM당 하나의 영역밖에 존재하지 않습니다.
    
## 6.2 힙 영역(Heap Area)

모든 인스턴스 오브젝트(new 키워드로 생성 된 클래스, 배열)이 생성되는 영역입니다. 공유자원이며 JVM당 하나의 영역밖에 존재하지 않습니다. 각 스레드에서 접근이 가능하기 때문에 스레드 동기화 작업이 이루어집니다.
    
```JAVA
Scanner sc = new Scanner(System.in)
//참조변수 sc는 Stack 영역에 할당되고, 생성되는 Scanner 객체는 Heap에 할당됩니다.
```

## 6.3 스택 영역(Stack Area)

각각의. 스레드마다 개별의 스택영역이 존재합니다. 메소드 콜스택이 메소드가 호출될 때마다 스택에 스택 프레임이라는 스택메모리에 쌓이게 됩니다. 모든 지역변수가 스택 메모리에 저장될것입니다. 스택 영역은 공유자원이 아니기 때문에 스레드에 안전합니다.

참고 : 프레임 데이터(Frame Data) - 메소드 관련 데이터(메소드 리턴 주소, 메소드 지역변수, 메소드 리턴 정보 등)을 포함하고 있습니다. 메소드 진입이 스택에 프레임데이터가 쌓입니다.

## 6.4 PC Register

현재 실행중인 명령문의 주소를 가지기 위해 각각의 스레드마다 개별 PC 레지스터가 존재한다.
    
## 6.5 Native Method Stacks

native 메소드 정보를 가지고 있는 스택. 개별 스레드마다 생성됨.    
    
# 7. Execution Engine(실행 엔진)

실행 데이터 영역에서 할당된 바이트 코드는 실행엔진에 의해서 실행된다. 실행엔진은 바이트코드를 읽으며 조각단위별로 실행합니다. 명령어를 하나 하나 실행하는 인터프리터(Interpreter)방식이 있고 JIT(Just-In-Time) 컴파일러를 이용하는 방식이 있다.

JIT 컴파일러는 적절한 시간에 전체 바이트 코드를 네이티브 코드로 변경해서 Execution Engine이 네이티브로 컴파일된 코드를 실행하는 것으로 성능을 높이는 방식이다.

## 7.1 인터프리터(Interpreter) 
    
인터프리터는 바이트코드를 빠르게 해석하지만 실행속도는 느립니다.(한줄씩) 인터프리터의 단점은 하나의 메소드가 여러번 호출되었을 때, 매번 새로운 해석(interpretation)이 필요하다는 것입니다. 

## 7.2 JIT 컴파일러(Just-In-Time)

JIT컴파일러는 인터프리터의 단점을 보완해줍니다. 실행엔진은 바이트코드를 변환하는데에 인터프리터의 도움을 받을테지만 반복되는 코드가 발견되었을 시에는 JIT컴파일러를 사용해서 반복되는 부분을 native code(원시코드)로 컴파일합니다. 변환된 원시코드는 인터프리터의 변환과정없이 직접적으로 사용이 가능하며(기존에는 바이트코드에서 원시코드로 변환 후 실행하였다면 JIT 컴파일러를 사용하여 변환된 원시코드는 변환하지않고 바로 실행) 이로 인해 시스템의 성능이 좋아지게 된다. 
    
## 7.3 가비지 컬렉터(Garbage Collector)
 
아무 참조가 없는 인스턴스들을 모아 제거하는 역할. System.gc() 메소드로 가비지 컬렉션을 실행할 수 있지만 보장되지는 않음.(반드시 실행되는건 아니라는 뜻), JVM의 가바지 컬렉션은 생성된 인스턴스들을 모음.
    
# 8. JVM 내부 구조

<img src="https://user-images.githubusercontent.com/79847020/135800259-e9d5bc7c-f7f6-46fd-9d04-558cdc4ce6bb.png" width="50%" height="50%">

* 자바 원시 인터페이스(Java Native Interface, JNI) - JNI는 Native Method Libraries와 관련있으며 실행엔진에 필요한 원시 라이브러리들을 제공합니다.

* 원시 메소드 라이브러리(Native Method Libraries) - Native Libraries의 집합이며 실행엔진에 필수적이다.

