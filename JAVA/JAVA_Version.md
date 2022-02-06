# Java Version 정리

## Java 종류

### Java SE 

  - Java Standard Edition / J2SE
  - 표준 에디션, Java의 핵심 API와 기능들을 제공한다.

### Jakarta EE (Java EE)

  - Java Enterprise Edition / J2EE
  - 기업에서 운영하는 서버 페이지에 특화된 에디션, JSP와 서블릿을 비롯한 웹 애플리케이션 서버에 관련된 기술들이 포함되어 있다.

### Java ME

  - Java Micro Edition / J2ME
  - PDA나 셋톱박스, 센서 등의 임베디드 시스템 환경에 특화된 에디션

### Java FX
  
  - 데스트탑 어플리케이션에 특화된 에디션

# Java 버전

* JDK 1.0a ~ JDK1.1
  * JDK 1.0a
  * JDK 1.0a2
  * JDK 1.0 
    * 발표 이전에 불렸던 이름은 Oak였으며, 안정화 작업을 거친 1.0.2 버전에서 Java로 이름이 바뀌었다.
  * JDK 1.1
    * 이너 클래스, JavaBeans, RMI, 리플렉션, 유니코드, 국제화 
* J2SE 1.2 ~ J2SE 5(1.5)
  * J2SE 1.2
    * 버전 2 부터 약칭을 J2SE(Java 2 Standard Edition)으로 표기
    * strictfp, Swing GUI, JIT, Java Applet을 구동하는 웹 브라우저 플러그인, CORBA, Collections 등이 추가되었다. 
    * 1999년에 업데이트를 통해 HotSpot JVM이 첫 선을 보인다.
  * J2SE 1.3
    * HotSpot JVM, JNDI, JPDA, JavaSound 등이 추가되었다.
  * J2SE 1.4
    * assert, 정규표현식, IPv6, Non-Blocking IO, XML API, JCE, JSSE, JAAS, Java Web Start 등이 추가되었다.
  * J2SE 5 (1.5)
    * 이 때부터 버젼 중 앞의 1을 빼버리고 표기하기 시작했다. 그러나 내부적으로는 여전히 1.5, 1.6, 1.7 등으로 데이터가 들어있다.
    * Generics, Annotation, Auto Boxing/Unboxing, Enumeration, 가변 길이 파라미터, Static Import, 새로운 Concurrency API 등이 추가되었다.
    * Java는 표준 입력(stdin) 지원이 시원찮았는데, J2SE 5에 들어서 java.util.Scanner가 추가되면서 이전보다 편하게 표준 입력을 사용할 수 있게 되었다.
* Java SE 6 ~ 17
  * Java SE 6
  * Java SE 7
  * Java SE 8
    * 오라클 인수 후 첫번째 버전
    * 2개 버전으로 나뉨(Oracle JDK, OpenJDK)
    * Lambda, new Date and Time API(LocalDateTime, …)
    * interface default method
    * Optional
    * PermGen Area 제거
  * Java SE 9
    * 모듈시스템 등장(jigsaw)
  * Java SE 10
    * var 키워드
    * 병렬 처리 가비지 컬렉션 도입으로 인한 성능 향상
    * JVM 힙 영역을 시스템 메모리가 아닌 다른 종류의 메모리에도 할당 가능
  * Java SE 11
    * Oracle JDK와 OpenJDK 통합
    * Oracle JDK가 구독형 유료 모델로 전환
    * 서드파티 JDK 로의 이전 필요
    * lambda 지역변수 사용법 변경 ((var x, var y) -> x.process(y) => (x, y) -> x.process(y))
  * Java SE 12
  * Java SE 13
  * Java SE 14
  * Java SE 15
  * Java SE 16
  * Java SE 17

## Java 1.8 vs Java 8 ?

Java의 공식적인 이름은 Java SE 6이고, 1.6.0은 개발자들을 위한 버전명이라고 생각하면 된다. 

## Backward Compatible 

JDK는 소스코드를 class file로 전환(컴파일) 시켜주는 javac 와, class file을 실행하는 JRE로 구성되어 있습니다.  또한 JDK는 javac 와 jre를 포함하기에, 싸이즈가 큽니다.   그렇기때문에 개발머쉰에는 JDK를 설치하고,  개발된 프로그램을 실행하는 머쉰에는 javac가 필요없으므로, JRE만 설치해서 씁니다.

JDK 8으로 생성된 class file은 Java 8으로 물론 실행되며, 상위버전 Java 9, 10, 11으로 호환됩니다.

이걸 backward compatible 이라합니다.  하지만 하위 버전으로는 실행이 안됩니다.  예를 들면 JDK 8으로 생성(컴파일)된 class file은 하위버전 Java 7,6 등등으로 실행이 안됩니다.  또한 가끔가다, Java 8 에서 실행되는것이 상위 버전으로 안되는 것이 있습니다.  이걸 Deprecated 됬다고 합니다.  Deprecated리스트는  각버젼 Release Note에 나와있습니다.

- 이미 컴파일 된 1.6 자바 클래스 파일은 1.8 에서도 돌아간다.
- 하지만 1.8 에서 1.6 버전으로 컴파일은 불가능하다.

## Oracle JDK, Open JDK

Oracle 자바의 3자 라이브러리 특허 문제로 OpenJDK에서 미구현했거나, 구현 방식의 차이가 생겨서 호환이 일부 안될 수도 있습니다. OpenJDK 전환에서 걸림돌이 바로 업데이트인데, JDK 8까지는 OpenJDK와 Oracle JDK 업데이트가 둘이 따로 노는 수준이었으나, 이후 버전은 OpenJDK를 오라클이 따라가는 식으로 바뀌었습니다. 물론 그 중 특허 라이브러리 수정 사항에 따라 차이가 미묘하게 발생할 수도 있어 걱정을 덜기 위해 위해 호환성이 보장된다고 홍보하는 Zulu 등의 타사 JDK를 쓰기도 합니다.

## 
