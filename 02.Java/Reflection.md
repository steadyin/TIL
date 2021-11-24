# 1. Reflextion?

자바의 Reflextion은 Runtime시 클래스, 메소드, 인터페이스을 추상화해서 조작하는데 사용되는 API입니다.

* Reflextion에 필요한 클래스는 Java.lang.reflect 패키지에 제공됩니다.
* Reflextion은 객체가 속한 클래스 정보와 호출할 수 있는 메소드 정보를 제공합니다.
* Reflextion을 통해 접근 제어자에 상관없이 Runtime시에 메서드를 호출 할 수 있습니다.

![image](https://user-images.githubusercontent.com/79847020/141441937-1b423b29-0db5-4aba-85dd-c0a26581f16b.png)

# 2. Reflextion 사용

Reflextion의 사용법은 다음과 같습니다.

* Class : getClass() 메소드는 객체가 속한 클래스의 이름을 가져오는데 사용합니다.
* Constructors : getConstructors() 메소드는 객체가 속한 클래스의 public 생성자를 가져오는데 사용합니다.
* Method : getMethods() 메소드는 객체가 속한 클래스의 public 메소드를 가져오는데 사용합니다.

```JAVA
class Test {
    // Private 멤버변수 
    private String s;
  
    // Public 생성자
    public Test()  {  s = "Example"; }
  
    // Public 메소드
    public void method1()  {
        System.out.println("The string is " + s);
    }
  
    // Public 메소드 + 매개변수
    public void method2(int n)  {
        System.out.println("The number is " + n);
    }
  
    // Private 메소드
    private void method3() {
        System.out.println("Private method invoked");
    }
}
```
```JAVA
import java.lang.reflect.Method;
import java.lang.reflect.Field;
import java.lang.reflect.Constructor;

public class Demo {
    public static void main(String args[]) throws Exception    {
        Test obj = new Test();
  
        //getclass method
        Class cls = obj.getClass();
        System.out.println("The name of class is " + cls.getName());
  
        //get constructor 
        Constructor constructor = cls.getConstructor();
        System.out.println("The name of constructor is " + constructor.getName());
  
        System.out.println("The public methods of class are : ");
  
        //get methods 
        Method[] methods = cls.getMethods();
  
        //method names
        for (Method method:methods) System.out.println(method.getName());
  
        //mehtod object
        Method methodcall1 = cls.getDeclaredMethod("method2",int.class);
 
        methodcall1.invoke(obj, 19);
        Field field = cls.getDeclaredField("s");
        field.setAccessible(true);
        field.set(obj, "JAVA");
  
        Method methodcall2 = cls.getDeclaredMethod("method1");
        methodcall2.invoke(obj);
        Method methodcall3 = cls.getDeclaredMethod("method3");
        methodcall3.setAccessible(true);
        methodcall3.invoke(obj);
    }
}
```
```CONSOLE
The name of class is Test
The name of constructor is Test
The public methods of class are : 
method2
method1
wait
wait
wait
equals
toString
hashCode
getClass
notify
notifyAll
The number is 19
The string is JAVA
Private method invoked
```

# 3. 주요 활용

### 3.1 메소드명과 매개변수를 알면 Reflextion을 통해 호출할 수 있다. 

두 가지 메소드를 사용합니다.

* getDeclaredMethod() : 호출할 메소드를 객체화 하는 것

    ```JAVA
    Class.getDeclaredMethod(name, parametertype)
    ```

* invoke() : Runtime 떄 클래스의 메소드를 호출하려면 다음 방법을 사용합니다.

    ```JAVA
    Method.invoke(MethodObject, Parameter)
    ```

### 3.2 Reflextion을 통해 우리는 클래스 객체의 도움을 받아 클래스의 Private 변수와 메소드에 접근할 수 있다.

두 가지 메소드를 사용합니다.

* getDeclaredField(fieldname) - Private 필드를 가져오는데 사용합니다. 

  ```JAVA
  Class.getDeclaredField(Fieldname)
  ```

* Field.setAccessible(true) - 필드와 사용되는 접근 제어자에 관계없이 필드에 접근할 수 있도록 허용합니다.

  ```JAVA
  Field.setAccessible(true)
  ```

# 4. 장점 및 단점

* 장점

  * 확장성 : fully-qualified name을 통하여 외부에서 정의된 클래스를 사용할 수 있습니다.
  * 디버깅 및 테스트 도구 : Reflextion 속성을 사용하여 클래스의 Private 필드에 접근할 수 있습니다.

* 단점

  * Reflextion은 비용이 큰 작업이다. 성능을 위해 사용을 기피해야 합니다.
  * 에러를 확인하기 어렵고 디버깅도 어렵다.
  * 코드가 복잡하다.
