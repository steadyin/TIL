# 1. String

* String은 Java에서 문자열을 표현하기 위해 사용되는 java.lang 패키지내에 포함된 클래스이다. 

* String은 문자열을 기본적으로 내부에서 byte배열로 다루고 기본 인코딩은 UTF-16 입니다.

* String은 한번 생성 되면 변할 수 없는 immutable객체(불변) 이다. 

* Java의 immutable 객체는 모두 thread-safe 하다.

* String으로 생성된 문자열은 Constant String Pool에 저장된다.

* "+" 연산자를 사용할 수 있습니다.

# 2. String 인스턴스

## 2.1 String Constant Pool ?

  * String Constant Pool(문자열 상수 풀)은 JVM에서 문자열 인스턴스를 저장하는 특수한 메모리 영역이다.

  * 같은 문자열의 복사본을 허용하지 않으므로서 메모리 공간을 최적화한다.
        
  * JAVA 6에는 String Constant Pool은 PermGen영역에 존재합니다. 이 PermGen은 일반적으로 사이즈가 고정되고 확장되지 않습니다. 따라서 너무 많은 String 인스턴스를 사용하면 OOM이 발생했습니다.

  * Java 7에서는 String Constant Pool은 PermGen영역이 아닌 Heap영역으로 위치를 옮겼습니다. 따라서 String Constant Pool도 GC의 대상이 되었고 -xx:StringTableSize 옵션으로 사이즈를 지정할 수 있게 되었습니다.

## 2.2 intern() ?

String의 intern() 메서드는 String Constant Pool에서 주어진 문자열과 동등한 문자열이 있는지 찾아보고 존재하면 해당 문자열의 참조값을 반환, 존재하지 않다면 String Constant Pool에 문자열을 생성하고 반환한다.

String의 intern 메소드를 호출하면 

```JAVA
String str1 = "test";
String str2 = new String("test");
String str3 = new String("test").intern();

System.out.println(str1 == str2);         // false
System.out.println(str1 == str3);         // true
```

## 2.3 String 인스턴스화

String은 두 가지 방법으로 인스턴스화 할 수 있다.

1. 리터럴로 생성하는 경우 -> String str = "ABC";

2. 키워드를 사용하는 경우 -> String str = new String("ABC");

String을 리터럴로 인스턴스화하는 경우 내부적으로 intern() 메소드를 호출해서 Strng Constant Pool에서 같은 문자열을 찾고 존재하면 해당 문자열을 반환하고 존재하지 않으면 String ConstantPool에 새로운 문자열을 생성하고 반환한다. 

String을 new 키워드로 인스턴스화 하는 경우 일반 객체를 인스턴스화하는 것처럼 Heap메모리에 String객체를 생성한다.

```JAVA
    String str1 = "test";
    String str2 = new String("test");
    String str3 = str2.intern();

    System.out.println(str1 == str2);         // false
    System.out.println(str1.equals(str2));    // true
    System.out.println(str1 == str3);         // true
    System.out.println(str1.equals(str3));    // true
```        

# 3. String 동일성과 동등성

String은 "=="으로 동일성(같은 객체)을 비교하고 equals() 메소드로 동등성(같은 문자)을 비교합니다.

str1==str2은 참조값이 같은지 비교합니다. str1.equals(str2)은 가지고 있는 문자열값이 같은지 비교합니다.

참고로 두 String 문자열이 같다면 str1.intern() == str2.intern()도 같습니다.

```JAVA
    String str1 = "test";
    String str2 = new String("test");

    System.out.println(str1 == str2);                    // false
    System.out.println(str1.equals(str2));               // true
    System.out.println(str1.intern()==str2.intern());    // true
```

# 4. Question

## 다음 코드에서 생성되는 String 인스턴스는 ?

```JAVA
    String s1 = new String("abc");
    String s2 = new String("abc");
    System.out.println(s1 == s2);
```

총 3개 String Constant Pool에 리터럴 인스턴스 1개, Heap 메모리에 String 인스턴스 2개

## String v StringBuffer/StringBuilder 

String은 immutable(불변)하다. 그리고 thread-safe하다. <br>
StringBuffer은 mutable(가변)하다, 그리고 thread-safe하지 않다. <br>
StringBuilder은 mutable(가변)하다, 그리고 thread-safe하다. <br>

String 의 + 연산자는 내부적으로는 StringBuilder 객체를 생성 후 append()로 문자열을 추가 후 toString()으로 다시 String객체로 변환한다.

이런 비효율적인 연산 때문에 일반적으로 String의 + 연산은 성능적으로도, 메모리 효율적으로도 지양해야한다.

웹서버 처럼 수많은 요청이 발생하는 경우 + 연산이 문제가 될 수도 있고 StringBuffer나 StringBuilder를 고려해보아야 한다.

정리하면 단순한 + 연산의 경우는 String으로, 반복문같이 반복적으로 + 연산이 발생하는 경우는 StringBuffer를, 멀티스레드 환경이면 StringBuilder를 고려해볼 수 있다. 
