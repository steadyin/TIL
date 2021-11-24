# 1. 인터페이스(interface)

## 1.1 인터페이스의 역할 

* 인터페이스는 개발 코드와 객체가 서로 통신하는 접점 역할, 중간 역할을 한다.

* 인터페이스는 객체의 사용 방법을 정의하고 설계하는 역할을 한다. 객체 사용 설명서 라고도 한다.

* 인터페이스는 객체의 교환성을 높여주기 때문에 다형성을 구현하는 매우 중요한 역할을 한다.

* 개발코드에서 직접 객체를 호출하지 않고 중간에 인터페이스를 두고 호출 하는 이유는 사용하는 객체를 변경할 수 있도록 하기 위해서이다.

* 추상화를 위해서 사용한다.

## 1.2 변화

* Java 7 까지는 상수와 추상 메서드만 선언 가능했다. → 강제성이 강했음을 유추할 수 있다.

* Java 8 부터는 default method와 static method가 추가되었다. → 두가지 메소드를 통해 강제성 안에 유연함을 추가했다고 이해할 수 있다.

* Java 9 에서 private 메서드가 추가되었다.

# 2. 인터페이스 선언

```JAVA
[ public ] interface 인터페이스명 {
    //상수
    [public] [static] [final] 타입 상수명 = 값;
   
    //추상 메소드
    [public] [abstract] 타입 메소드명(매개변수,...);
    
    //디폴트 메소드
    [public] default 타입 메소드명(매개변수,...) {...}
    
    //정적 메소드
    [public] static 타입 메소드명(매개변수) {...}
}
```

```JAVA
package thisisjava.chap08;

public interface RemoteControl {

    //상수
    public int MAX_VOLUME = 10;
    public int MIN_VOLUME = 0;

    //추상 메소드
    public void turnOn();
    public void turnOff();
    public void setVolume(int volume);

    //디폴트 메소드
    default void setMute(boolean mute) {
        if(mute) {
            System.out.println("무음 처리 합니다.");
        } else {
            System.out.println("무음 해제 합니다.");
        }
    }
    
    //정적 메소드
    static void changeBattery() {
        System.out.println("건전지를 교환합니다.");
    }
}
```

* 상수 필드(Coinstant Field)
 
  인터페이스 내의 상수 필드는 public static final로 선언되며 생략할 수 있다.
  
  인터페이스 상수는 static {} 블록으로 초기화할 수 없기 때문에 반드시 선언과 동시에 초기값을 지정해주어야 한다.
  
* 추상 메소드(Abstract Method)

  인터페이스 구현 클래스에게 구현을 강요하는 메소드을 의미한다. public abstract로 선언되며 생략할 수 있다.

* 디폴트 메소드(Default Method)

  Java8 에서 추가된 기능, 인터페이스에 구현되어 있지만 구현 객체가 가지게 되는 인스턴스 메소드라고 생각해야한다. public으로 선언되며 생략할 수 있다.

* 정적 메소드(Static Method)

  Java8 에서 추가된 기능, 객체가 없어도 인터페이스만으로 호출이 가능한 메소드이다.
  
# 3. 인터페이스 구현
  
## 3.1 구현 클래스

```JAVA
public class 구현클래스 implements 인터페이스 {
    //인터페이스에 선언된 추상 메소드의 실제 메소드 선언
    @Override
    public void method() {
        구현
    }
}
```

```JAVA
package thisisjava.chap08;

public class Television implements RemoteControl {
    //필드
    private int volume;

    @Override
    public void turnOn() {
        System.out.println("TV를 켭니다.");
    }

    @Override
    public void turnOff() {
        System.out.println("TV를 끕니다.");
    }

    @Override
    public void setVolume(int volume) {
        if (volume > RemoteControl.MAX_VOLUME) {
            this.volume = RemoteControl.MAX_VOLUME;
        } else if (volume < RemoteControl.MIN_VOLUME) {
            this.volume = RemoteControl.MIN_VOLUME;
        } else {
            this.volume = volume;
        }
        System.out.println("현재 TV 볼륨 : " + volume);
    }

    @Override
    public void setMute(boolean mute) {
        RemoteControl.super.setMute(mute);
    }
}
```

```JAVA
package thisisjava.chap08;

public class Audio implements RemoteControl {
    //필드
    private int volume;

    @Override
    public void turnOn() {
        System.out.println("Audio를 켭니다.");
    }

    @Override
    public void turnOff() {
        System.out.println("Audio를 끕니다.");
    }

    @Override
    public void setVolume(int volume) {
        if (volume > RemoteControl.MAX_VOLUME) {
            this.volume = RemoteControl.MAX_VOLUME;
        } else if (volume < RemoteControl.MIN_VOLUME) {
            this.volume = RemoteControl.MIN_VOLUME;
        } else {
            this.volume = volume;
        }
        System.out.println("현재 Audio 볼륨 : " + volume);
    }

    @Override
    public void setMute(boolean mute) {
        RemoteControl.super.setMute(mute);
    }
}
```
* 인터페이스에 선언도니 추상 메소드에 대응하는 실체 메소드를 구현 클래스가 작성하지 않으면 구현 클래스는 추상 클래스가 되므로 ABSTRACT 키워드를 추가해야 한다.

* 구현 객체가 인터페이스의 추상 메소드를 구현 할 때는 @Override 애노테이션을 붙여 컴파일러가 체크하도록 하는 것이 좋습니다.

## 3.2 익명 구현 객체

자바에서는 소스 파일을 만들지 않고 구현 객체를 만들 수 있는 방법을 제공하는데 그것이 익명 구현 객체이다.

주로 임시 작업 스레드를 만들기 위해, Java8에서 람다식을 활용하기 위해 익명 구현 객체를 사용한다.

익명 구현 객체가 포함된 클래스를 컴파일 하면 생성 번호 $1이 순차적으로 붙여지며 익명클래스의 클래스파일이 만들어진다. ex) ExampleClass$1.class

```JAVA
인터페이스 변수 = new 인터페이스() {
    //인터페이스에 선언된 추상 메소드의 실체 메소드 선언
};
```
```JAVA
package thisisjava.chap08;

public class RemoteControlExample {
    public static void main(String[] args) {
        RemoteControl rc = new RemoteControl() {
            @Override
            public void turnOn() {
                System.out.println("익명클래스 turnOn");
            }

            @Override
            public void turnOff() {
                System.out.println("익명클래스 turnOff");
            }

            @Override
            public void setVolume(int volume) {
                System.out.println("익명클래스 setVolume");
            }
        };
    }
}
```

## 3.3 다중 인터페이스 구현 클래스

* 인터페이스는 다중 인터페이스 구현을 지원한다.

```JAVA
public class 구현클래스명 implements 인터페이스A, 인터페이스B {
  //인터페이스 A에 선언된 추상 메소드의 실제 메소드 선언
  //인터페이스 B에 선언된 추상 메소드의 실제 메소드 선언  
}
```
```JAVA
package thisisjava.chap08;

public interface Searchable {
    void search(String url);
}
```
```JAVA
package thisisjava.chap08;

public class SmartTelevision implements RemoteControl, Searchable {
    private int volume;

    @Override
    public void turnOn() {
        System.out.println("TV를 켭니다.");
    }

    @Override
    public void turnOff() {
        System.out.println("TV를 끕니다.");
    }

    @Override
    public void setVolume(int volume) {
        if (volume > RemoteControl.MAX_VOLUME) {
            this.volume = RemoteControl.MAX_VOLUME;
        } else if (volume < RemoteControl.MIN_VOLUME) {
            this.volume = RemoteControl.MIN_VOLUME;
        } else {
            this.volume = volume;
        }
        System.out.println("현재 TV 볼륨 : " + volume);
    }

    @Override
    public void search(String url) {
        System.out.println(url + "을 검색합니다.");
    }
}
```

# 4. 인터페이스 사용

* 인터페이스 변수는 참조 타입이기 때문에 구현 객체가 대입될 경우 구현 객체의 번지를 저장한다.

```JAVA
인터페이스 변수 = new 구현 객체();
```

## 4.1 추상 메소드 사용

* 구현 객체가 인터페이스 타입에 대입되면 인터페이스에 선언된 추상 메소드를 개발 코드에서 호출 할 수 있다.

```JAVA
package thisisjava.chap08;

public class RemoteControlExample {
    public static void main(String[] args) {
        RemoteControl rc = null;

        rc = new Television();
        rc.turnOn();
        rc.turnOff();

        rc = new Audio();
        rc.turnOn();
        rc.turnOff();
    }
}
```

## 4.2 디폴트 메소드의 사용

* 디폴트 메소드는 인터페이스에 선언되지만 인터페이스에서 바로 사용할 수 없다. 

* 디폴트 메소드는 추상 메소드가 아닌 인스턴스 메소드이므로 구현 객체가 있어야 사용할 수 있다.

* 디폴트 메소드는 인터페이스이 모든 구현 객체가 가지고 있는 기본 메소드라고 생각하면 된다.

* 구현 클래스에서 디폴트 메소드를 오버라이딩, 재정의 할 수 있다.

```JAVA
    인터페이스.디폴트메소드() -> 사용 불가하다.
    
    인터페이스구현객체.디폴트메소드() -> 사용 가능하며, 구현 객체에서 재정의할 수 있다.
```    
```JAVA
package thisisjava.chap08;

public class RemoteControlExample {
    public static void main(String[] args) {
        RemoteControl rc = null;

        rc = new Television();
        rc.setMute(true);
    }
}
```


## 4.3 정적 메소드 사용

* 정적 메소드는 인터페이스로 바로 호출이 가능하다.

```JAVA
package thisisjava.chap08;

public class RemoteControlExample {
    public static void main(String[] args) {
        RemoteControl.changeBattery();
    }
}
```

# 5. 타입 변환과 다형성

* 다형성 ?

      하나의 타입에 대입되는 객체에 따라서 실행 결과가 다양한 형태로 나오는 성질을 말한다.
      
      상속 관계에서 다형성은 부모 타입에 어떤 자식 객체를 대입하느냐에 따라 실행 결과가 달라지는 것을 말한다.
      
      인터페이스에서 다형성은 인터페이스 타입에 어떤 국현 객체를 대입하느냐에 따라 실행 결과가 달라지는 것을 말한다.
      
* 인터페이스 타입으로 매개 변수를 선언하면 메소드 호출 시 매개값을 여러 가지 종류의 구현 객체를 줄 수 있다.

## 5.1 자동 타입 변환(Promotion)

* 구현 객체가 인터페이스 타입으로 변환되는 것은 자동 타입 변환에 해당한다

* 인터페이스 구현 클래스를 상속해서 자식 클래스를 만들었다면 자식 객체 역시 인터페이스 타입으로 자동 타입 변환할 수 있다.

```JAVA
package thisisjava.chap08;

interface Z {
    default public void print() {
        System.out.println(this.getClass());
    };
}
class A implements Z {}
class B extends A {}
class C implements Z {}
class D extends C {}

public class RemoteControlExample {
    public static void main(String[] args) {
        // 인터페이스 변수
        Z example;

        // A -> 인터페이스 구현체
        example = new A();
        example.print(); //class thisisjava.chap08.A
        // B -> 인터페이스 구현체의 서브 클래스
        example = new B();
        example.print(); //class thisisjava.chap08.B
        // C -> 인터페이스 구현체
        example = new C();
        example.print(); //class thisisjava.chap08.C
        // D -> 인터페이스 구현체의 서브 클래스
        example = new D();
        example.print(); //class thisisjava.chap08.D
    }
}
```

## 5.2 필드의 다형성

```JAVA
package thisisjava.chap08;

public interface Tire {
    public void roll();
}
```

```JAVA
package thisisjava.chap08;

public class HankookTire implements Tire {
    @Override
    public void roll() {
        System.out.println("한국 타이어가 굴러갑니다.");
    }
}
```

```JAVA
package thisisjava.chap08;

public class KumhoTire implements Tire {
    @Override
    public void roll() {
        System.out.println("금호 타이어가 굴러갑니다.");
    }
}
```
```JAVA
package thisisjava.chap08;

public class Car {
    Tire frontLeftTire = new HankookTire();
    Tire frontRightTire = new HankookTire();
    Tire backLeftTire = new HankookTire();
    Tire bakcRightTire = new HankookTire();

    void run() {
        frontLeftTire.roll();
        frontRightTire.roll();
        backLeftTire.roll();
        bakcRightTire.roll();
    }
}
```
```JAVA
package thisisjava.chap08;

public class CarExample {
    public static void main(String[] args) {
        Car myCar = new Car();

        myCar.run();

        myCar.backLeftTire = new KumhoTire();
        myCar.bakcRightTire = new KumhoTire();

    myCar.run();
    }
}
```
![image](https://user-images.githubusercontent.com/79847020/140635213-9ebd0ce2-a85d-466a-8efd-2f7e949fa4a1.png)

## 5.3 인터페이스 배열로 구현 객체 관리

* 인터페이스 배열을 인터페이스의 구현 객체를 담을 수 있습니다.

```JAVA
package thisisjava.chap08;

public class CarExample {
    public static void main(String[] args) {
        Tire[] tires = {
                new KumhoTire(),
                new HankookTire(),
                new KumhoTire(),
                new HankookTire()
        };
        
        for(Tire tire : tires) {
            tire.roll();
        }
    }
}
```

## 5.7 매개 변수의 다형성

* 자동 타입 변환은 필드의 값을 대입할 때에도 발생하지만 매개변수를 인터페이스 타입으로 선언하고 호출 할 때에도 발생합니다.

## 5.8 강제 타입 변환(Casting)

* 구현 객체가 인터페이스 타입으로 자동 변환하면 인터페이스에 선언된 메소드만 사용 가능하다는 제약 사항이 따릅니다.

* 강제 타입 변환을 해서 구현 클래스 타입으로 변환한 다음, 구현 클래스의 필드와 메소드를 사용할 수 있다.

```JAVA
구현클래스 변수 = (구현클래스) 인터페이스변수;
```

## 5.9 객체 타입 확인(instanceof)

* 강제 타입 변환은 구현 객체가 인터페이스 타입으로 변환되어 있는 상태에서 가능하다.

* 어떤 구현 객체가 변환되어 있는지 알 수 없는 상태에서 무작정 변환을 할 경우 ClassCastException이 발생할 수도 있다.

* instanceof 를 통해서 객체 타입을 확인할 수 있다.

```JAVA
if ( vehicle instanceof Bus) {
    Bus bus = (Bus) vehicle;
}
```
 
# 6. 인터페이스 상속

인터페이스도 다른 인터페이스를 상속할 수 있다. 다중 상속을 허용한다.

```JAVA
public interface 하위인터페이스 extends 상위인터페이스1, 상위인터페이스2 {}
```

하위 인터페이스를 구현하는 클래스의 하위 인터페이스의 메소드 뿐만 아니라 상위 인터페이스의 모든 추상 메소드에 대한 실체 메소드를 가지고 있어야 한다.

그렇기 때문에 구현 클래스로부터 객체를 생성하고 나서 하위 및 상위 인터페이스 타입으로 변환이 가능하다.

```JAVA
하위인터페이스 변수 = new 구현클래스A(...);
상위인터페이스1 변수 = new 구현클래스A(...);
상위인터페이스2 변수 = new 구현클래스A(...);
```

# 7. 인터페이스 Default 메서드 - JAVA 8

* 자바 8 이전에 인터페이스가 가질 수 있는 메서드는 추상메서드 뿐이었다. 

* default 메서드는 인터페이스 내부에 존재할 수 있는 구현메서드 이다.

* 이 때 인터페이스의 구현 클래스들이 메서드 구현 할 때 코드가 일치하더라도 클래스마다 구현부를 작성해야 하는 불편함이 있었다. 이 불편함을 개선하기 위해 자바8부터 default 메서드를 도입하게 되었다. 

* 하위 호환성을 위해서 사용한다. 기존에 존재하던 인터페이스를 이용해서 구현된 클래스를 만들고 사용하고 있는데 추가적으로 구현해야 할 메소드가 있다면 이미 인터페이스를 구현한 클래스와의 호환성이 떨어지게 된다. 이 때 default 메소드를 추가해서 하위 호환성은 유지하면서 인터페이스 기능을 보완할 수 있다.

* 인터페이스를 통해 default 메서드를 바로 사용하는 것이 아니라 인터페이스의 구현 객체를 통해 default 메서드를 사용할 수 있다.

```JAVA
interface Drawable{  
    void draw();  
    default void msg(){System.out.println("default method");}  
}  

class Rectangle implements Drawable{  
    public void draw(){System.out.println("drawing rectangle");}  
}  

class TestInterfaceDefault{  
    public static void main(String args[]){  
        Drawable d=new Rectangle();  
        d.draw();  
        d.msg();  
    }
}  
```

# 8. 인터페이스 Static 메서드 - 자바 8

* Static Method를 사용해 인터페이스의 필요한 유틸리티, 헬퍼 메소드를 제공할 수 있다. 

* 인터페이스의 static 메서드는 인터페이스 내에서 이미 구현한 메서드이다. 

* 구현 클래스에서 오버라이딩하여 사용할 수 없다.

* 인터페이스를 통해 static 메서드를 바로 사용할 수 있다.

```JAVA
interface Drawable{  
    void draw();  
    static int cube(int x){return x*x*x;}  
}  

class Rectangle implements Drawable{  
    public void draw(){System.out.println("drawing rectangle");}  
}  
  
class TestInterfaceStatic{  
    public static void main(String args[]){  
        Drawable d=new Rectangle();  
        d.draw();  
        System.out.println(Drawable.cube(3));  
    }  
}    
```    

# 9. 인터페이스 Private 메서드 - JAVA 9

* 자바9 부터 인터페이스 내에 private 메서드를 가질 수 있게 되었다.

* 인터페이스에서 사용 가능한 멤버는 다음과 같다.

    * 상수
    * Abstract 메서드
    * Default 메서드
    * Static 메서드
    * Private 메서드
    * Private static 메서드

* Private 메서드는 오직 해당 인터페이스 내에서만 접근 가능하며, 인터페이스를 상속받은 클래스나 서브 인터페이스에서는 접근할 수 없다.

```JAVA
public interface TestInterface {
    void mul(int a, int b);

    default void add(int a, int b) {
        System.out.print("default 메서드 => ");
        System.out.println(a + b);
    }

    static void mod(int a, int b) {
        System.out.print("static 메서드 => ");
        System.out.println(a % b);
    }

    private void sub(int a, int b) {
        System.out.print("private 메서드 => ");
        System.out.println(a - b);
    }

    private static void div(int a, int b) {
        System.out.print("private static 메서드 => ");
        System.out.println(a / b);
    }
}

class Test implements TestInterface {
    @Override
    public void mul(int a, int b) {
        System.out.print("abstract 메서드 => ");
        System.out.println(a * b);
    }

    public static void main(String[] args) {
        Test t = new t();
        t.mul(2, 3);
        t.add(6, 2);
        Test.mod(5, 3);
    }
}
```
```CONSOLE
abstract 메서드 => 5
default 메서드 => 8
static 메서드 => 2      
```
      
# 10. Question

## 인터페이스(Interface) vs 추상 클래스(Abstract Class)

실무에서 Class를 디자인 할 때 상위 Class를 토대로 하위 Class를 디자인 하는 경우 Bottom-up으로 디자인 하는 것을 Generalization이라 하며 Top-Down으로 디자인 하는 것을 Specialization이라고 합니다. 그리고 Super Class와 Sub Class의 종속 관계를 만들 수 있습니다. 이런 관계를 객체 지향에서는 상속(Inheritance)라고 합니다.

Super Class를 어느 정도 구현한 상태에서 Sub Class를 디자인 하는 경우에 Sub Class는 Super Class에는 구현이 안되있는 필요한 부분을 더 구현할 수 있습니다. 이렇게 Super Class가 어느 정도 구현이 되있지만 완성된 형태를 Sub Class에 요구하는 경우 Super Class를 Java에서는 Abstrat Class로 디자인 합니다.

Abstrat Class에는 Sub Class에 상속될 수 있는 멤버 변수나 메소드들이 있을 수도 있고 없을 수도 있습니다. 또한 구현 부분이 비어있는 Abstract Method를 선언할 수도 있습니다. Abstrat Class 역시 Java Class이기 때문에 다중 상속이 불가능합니다.

참고로 Abstract Class를 상속받은 Sub Class는 Abstract Method가 있으면 Abstract Method의 Body를 구현해주거나 구현하지 않으면 Sub Class 역시 Abstract Class로 선언되어야 합니다.

Interface는 Abstract와 비슷하지만 실질적인 구현이 전혀 없는 Abstract로 구성된 빈 Class입니다. 즉 구현은 Sub Class에게 맡기고 단지 Sub Class들이 구현해야 되는 메소드들을 정의해주는 일종의 규약 같은 것입니다.

이 개념은 Class 디자인에서 상당히 중요하게 생각되는데 그 이유는 Interface를 보면 Sub Class들이 어떤 기능을 가지고 있을지 파악 되기 때문입니다. 물론 실질적인 기능은 Sub Class 마다 다르게 구현될 수 있습니다.

그래서 Interface는 일반적으로 내부가 비어진 빈 껍데기와 같은 Empty Shell로 구성되어있습니다. 물론 Interface 내에 메소드 뿐만 아니라 변수를 선언할 수도 있지만 Interface내 변수는 자동적으로 public static final이 붙게되서 변수가 아닌 상수가 됩니다.

인터페이스는 구현의 개념이기 때문에 다중 구현 또한 가능합니다.

정리하면

* Abstract Class는 단일 상속(Sigle Inheritance)만 가능하며, Interface는 다중 구현(Multiple Implementations)가 가능합니다.
* Abstract Class는 모든 접근 제어자(private, public, protected, default)가 선언 가능하며, Interface는 public만 가능합니다.
* Abstract Class는 일반적인 변수와, 상수가 사용 가능하며, Interface는 상수만 사용이 가능합니다.
* Abstract Class는 추상 메소드, 일반 메소드 둘다 사용이 가능한 반면에 Interface는 오직 추상 메소드만 사용이 가능합니다.
* Abstract Class A extends B -> A가 B를 확장한 클래스 관계를 디자인할 때, Interface는 A has B -> A는 B의 기능을 포함한다 같은 클래스를 디자인 할 때 사용됩니다.
* Interface에 Java8에서 Default, Static Java9에서 Private 메소드가 추가되면서 기능이 보완되었습니다.

## 다형성

하나의 타입에 대입되는 객체에 따라서 실행 결과가 다양한 형태로 나오는 성질을 말한다.

상속 관계에서 다형성은 부모 타입에 어떤 자식 객체를 대입하느냐에 따라 실행 결과가 달라지는 것을 말한다.

인터페이스에서 다형성은 인터페이스 타입에 어떤 국현 객체를 대입하느냐에 따라 실행 결과가 달라지는 것을 말한다.

## 강한 연결고리 약한 연결고리
