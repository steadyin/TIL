# Serializable(직렬화)

## Serializable(직렬화) ?

자바 직렬화란 자바 시스템 내부에서 사용되는 객체 또는 데이터를 외부의 자바 시스템에서도 사용할 수 있도록 바이트(byte) 형태로 데이터 변환하는 기술과 바이트로 변환된 데이터를 다시 객체로 변환하는 기술(역직렬화)을 아울러서 이야기합니다.

시스템적으로 이야기하자면 JVM(Java Virtual Machine 이하 JVM)의 메모리에 상주(힙 또는 스택)되어 있는 객체 데이터를 바이트 형태로 변환하는 기술과 직렬화된 바이트 형태의 데이터를 객체로 변환해서 JVM으로 상주시키는 형태를 같이 이야기합니다.

## serialVersionUID ?

```JAVA
static final long serialVersionUID = 1L;
```

이 값은 해당 클래스의 버전을 명시하는 용도로 사용합니다. A의 인스턴스 객체 B를 내부 Java 시스템에서 외부 Java 시스템으로 전송하기 위해서는 각 시스템에 A 클래스가 존재해야만 합니다. 각 시스템의 클래스가 같은 클래스인지 구분하기 위해 Java에서는 serialVersionUID을 비교합니다.  반드시 static final long으로 선언해야 하며, 변수명도 serialVersionUID로 선언해 주어야만 Java에서 인식할 수 있습니다. 만약 별도로 지정하지 않으면 자바 소스가 컴파일 될 때 자동으로 serialVersionUID를 생성합니다.

## 객체 저장

ObjectOutputStream이라는 클래스를 사용하면 객체를 저장할 수 있습니다. ObjectOutputStream에는 wrtie() 메소드를 사용하여 int 값을 저장하고 writeByte() 메소드로 바이트 값을 저장하면 된다. Serializable 인터페이스가 구현되있지 않으면 writeByte()는 NotSeralizableException 예외가 발생합니다. 실행 후 경로에 생성 된 serial.obj를 확인할 수 있고 해당 파일은 바이너리로 되어 있습니다.

```JAVA
import java.io.Serializable;

public class SerialDTO implements Serializable {
  private String booName;
  private int bookOrder;
  private boolean bestSeller;
  private long soldPerDay;

  public SerialDTO(String booName, int bookOrder, boolean bestSeller, long soldPerDay) {
    this.booName = booName;
    this.bookOrder = bookOrder;
    this.bestSeller = bestSeller;
    this.soldPerDay = soldPerDay;
  }

  @Override
  public String toString() {
    return "SerialDTO{" +
          "booName='" + booName + '\'' +
          ", bookOrder=" + bookOrder +
          ", bestSeller=" + bestSeller +
          ", soldPerDay=" + soldPerDay +
          '}';
  }
}
```
```JAVA
import java.io.FileOutputStream;
import java.io.ObjectOutputStream;

public class ManageObject {
  public static void main(String[] args) {
    ManageObject manage = new ManageObject();
    String fullPath = "\test.txt";

    SerialDTO dto = new SerialDTO("God of Java", 1, true, 100);
    manage.saveObject(fullPath, dto);
  }

  public void saveObject(String fullPath, SerialDTO dto) {
    FileOutputStream fos = null;
    ObjectOutputStream oos = null;
    try {
      fos = new FileOutputStream(fullPath);
      oos = new ObjectOutputStream(fos);
      oos.writeObject(dto);
      System.out.println("Write Success");
    } catch (Exception e) { 
      e.printStackTrace();
    } finally {
      if (oos != null) {
        try {
          oos.close();
        } catch (Exception e) {
          e.printStackTrace();
        }
      }
      if (fos != null) {
        try {
          fos.close();
        } catch (Exception e) {
          e.printStackTrace();
        }
      }
    }
  }
}
```

## 객체 읽기

ObjectInputStream이라는 클래스를 사용하면 저장해 놓은 객체를 읽을 수 있습니다. ObjectInputStream의 read()로 시작하는 메소드를 사용합니다. 일단 Object로 받은 후 SerialDTO로 형 변환 합니다.
```JAVA
import java.io.*;

public class ManageObject {
    public static void main(String[] args) {
        ManageObject manage = new ManageObject();
        String fullPath = "\test.txt";
        manage.loadObject(fullPath);
    }

    public void loadObject(String fullPath) {
        FileInputStream fis = null;
        ObjectInputStream ois = null;
        try {
            fis = new FileInputStream(fullPath);
            ois = new ObjectInputStream(fis);
            Object obj = ois.readObject();
            SerialDTO dto = (SerialDTO)obj;
            System.out.println(dto);
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (ois != null) {
                try {
                    ois.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
        if (fis != null) {
            try {
                fis.close();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}
```
```CONSOLE
SerialDTO{booName='God of Java', bookOrder=1, bestSeller=true, soldPerDay=100}
```

이번에는 Serializable 객체가 변경되었을 때 어떤 상황이 연출되는지 확인해보겠습니다. SerialDTO 클래스에 변수를 추가후 다시 loadObject를 실행하겠습니다.

```JAVA
public class SerialDTO implements Serializable {
    private String bookType = "IT";
    //생략
```    

```CONSOLE
java.io.InvalidClassException: FileIO.SerialDTO; local class incompatible: stream classdesc serialVersionUID = -3241876345619234545, local class serialVersionUID = -1745434743762347454
    at java.base/java.io.ObjectStreamClass.initNonProxy(ObjectStreamClass.java:689)
    at java.base/java.io.ObjectInputStream.readNonProxyDesc(ObjectInputStream.java:1982)
    at java.base/java.io.ObjectInputStream.readClassDesc(ObjectInputStream.java:1851)
    at java.base/java.io.ObjectInputStream.readOrdinaryObject(ObjectInputStream.java:2139)
    at java.base/java.io.ObjectInputStream.readObject0(ObjectInputStream.java:1668)
    at java.base/java.io.ObjectInputStream.readObject(ObjectInputStream.java:482)
    at java.base/java.io.ObjectInputStream.readObject(ObjectInputStream.java:440)
    at FileIO.ManageObject.loadObject(ManageObject.java:49)
    at FileIO.ManageObject.main(ManageObject.java:12)
```    

serialVersionUID가 다르다는 InvalidClassException 예외가 발생합니다. 컴파일시 자동으로 생성되는 serialVersionUID가 SerialDTO가 수정되면서 변경되었기 때문입니다. 

주의 해야 할 것은 Serializable을 구현해 놓은 상황에서 serialVersionUID를 명시적으로 지정하면 변경되더라도 예외가 발생하지 않는다. 하지만 만약 Serializable한 객체의 내용이 바뀌었는데도 아무런 예외가 발생하지 않으면 운영 상황에서 데이터가 꼬일 수 있기 때문에 절대 권장하는 코딩 습관이 아닙니다. 데이터가 바뀌면 serialVersionUID의 값을 변경하는 습관을 가져야 합니다. 

## transient

다음과 같이 SerialDTO 클래스에 transient 라는 예약어를 추가 후에 다시 객체를 파일에 저장하고 읽어오는 코드를 실행하겠습니다.

```JAVA
transient private int bookOrder;
```

```CONSOLE
Write Success
SerialDTO{booName='God of Java', bookOrder=0, bestSeller=true, soldPerDay=100}
```

bookOrder 값을 1로 지정했음에도 읽어낸 값을 살펴보면 0이 출력된 것을 확인할 수 있습니다. 객체를 저장하거나 다른 JVM으로 보낼 때 transient라는 예약어를 사용하여 선언한 변수는 Serializable의 대상에서 제외됩니다. 예를 들어 전송된다면 보안상 큰 문제가 발생할 수 있는 패스워드 변수에 적용할 수 있습니다.

## Java 직렬화는 언제(when) 어디서(where) 사용되나요?

JVM의 메모리에서만 상주되어있는 객체 데이터를 그대로 영속화(Persistence)가 필요할 때 사용됩니다.
시스템이 종료되더라도 없어지지 않는 장점을 가지며 영속화된 데이터이기 때문에 네트워크로 전송도 가능합니다.
그리고 필요할 때 직렬화된 객체 데이터를 가져와서 역직렬 화하여 객체를 바로 사용할 수 있게 됩니다.
그런 특성을 살린 자바 직렬화는 많은 곳에서 이용됩니다. 많이 사용하는 부분 몇 개만 이야기해보겠습니다.

* 서블릿 세션 (Servlet Session)

  서블릿 기반의 WAS(톰캣, 웹로직 등)들은 대부분 세션의 자바 직렬화를 지원하고 있습니다.
물론 단순히 세션을 서블릿 메모리 위에서 운용한다면 직렬화를 필요로 하지 않지만,
파일로 저장하거나 세션 클러스터링, DB를 저장하는 옵션 등을 선택하게 되면 세션 자체가 직렬화가 되어 저장되어 전달됩니다.
(그래서 세션에 필요한 객체는 java.io.Serializable 인터페이스를 구현(implements) 해두는 것을 추천합니다.)
참고로 위 내용은 서블릿 스펙에서는 직접 기술한 내용이 아니기 때문에 구현한 WAS 마다 동작은 달라질 수 있습니다.

* 캐시 (Cache)

  자바 시스템에서 퍼포먼스를 위해 캐시(Ehcache, Redis, Memcached, …)
라이브러리를 시스템을 많이 이용하게 됩니다.
자바 시스템을 개발하다 보면 상당수의 클래스가 만들어지게 됩니다.
예를 들면 DB를 조회한 후 가져온 데이터 객체 같은 경우 실시간 형태로 요구하는 데이터가 아니라면
메모리, 외부 저장소, 파일 등을 저장소를 이용해서 데이터 객체를 저장한 후 동일한 요청이 오면 DB를 다시 요청하는 것이 아니라 저장된 객체를 찾아서 응답하게 하는 형태를 보통 캐시를 사용한다고 합니다.
캐시를 이용하면 DB에 대한 리소스를 절약할 수 있기 때문에 많은 시스템에서 자주 활용됩니다. (사실 이렇게 간단하진 않습니다만 간단하게 설명했습니다.)
이렇게 캐시 할 부분을 자바 직렬화된 데이터를 저장해서 사용됩니다. 물론 자바 직렬 화만 이용해서만 캐시를 저장하지 않지만 가장 간편하기 때문에 많이 사용됩니다.

* 자바 RMI(Remote Method Invocation)

  최근에는 많이 사용되지 않지만 자바 직렬화를 설명할 때는 빠지지 않고 이야기되는 기술이기 때문에 언급만 하고 넘어가려고 합니다.
자바 RMI를 간단하게 이야기하자면 원격 시스템 간의 메시지 교환을 위해서 사용하는 자바에서 지원하는 기술입니다.
보통은 원격의 시스템과의 통신을 위해서 IP와 포트를 이용해서 소켓통신을 해야 하지만 RMI는 그 부분을 추상화하여 원격에 있는 시스템의 메서드를 로컬 시스템의 메서드인 것처럼 호출할 수 있습니다.
원격의 시스템의 메서드를 호출 시에 전달하는 메시지(보통 객체)를 자동으로 직렬화 시켜 사용됩니다.
그리고 전달받은 원격 시스템에서는 메시지를 역직렬화를 통해 변환하여 사용됩니다.
자세한 내용은 작은 책 한 권 정도의 양이 되기 때문에 따로 한번 찾아보시는 것을 추천드립니다.

### 출처

[자바의신 2권 27장](#)  <br>
https://techblog.woowahan.com/2550/

