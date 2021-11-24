Spring의 싱글톤과 Java Static을 이용한 싱글톤 패턴은 하나의 인스턴스를 공유한다는 개념은 같지만 인스턴스의 생명주기(생성, 사용, 소멸)에서 큰 차이를 보인다.

Java Static Singleton의 Scope은 ClassLoader기준이고 Spring Singleton의 Scope은 ApplicationContext 기준이다.

## JAVA Singleton 구현

자바에서 싱글톤을 구현하는 방법은 보통 이렇다.

1. 클래스 밖에서는 오브젝트를 생성하지 못하도록 생성자를 private으로 만든다.

2. 생성된 싱글톤 오브젝트를 저장할 수 있는 자신과 같은 타입의 스태틱 필드를 정의하다.

3. Static 팩토리 메소드인 getInstance()를 만들고 이 메소드가 최초로 호출되는 시점에서 한번만 오브젝트가 만들어지게 한다. 생성된 오브젝트는 스태틱 필드에 저장된다. 또는 스태틱 필드의 초기값으로 오브제트를 미리 만들어둘 수도 있다.

4. 한번 오브젝트(싱글톤)가 만들어지고 난 후에는 getInstance() 메소드를 통해 이미 만들어져 스태틱 필드에 저장해둔 오브젝트를 넘겨준다.


```JAVA
public class Singleton {
    private static Singleton INSTANCE;

    private Singleton() {}
    
    public static synchronized Singleton getInstance() {
        if (INSTANCE == null) INSTANCE = new Singleton();
        
        return INSTANCE;
    }
}
```

## Java Static Singleton in Tomcat

Java Static의 Scope은 ClassLoader기준이라는 말을 Tomcat에서 한번 살펴보자. 

![image](https://user-images.githubusercontent.com/79847020/135992795-cb94cd36-8ffe-4f2d-a43f-030502eab29e.png)

Tomcat의 ClassLoader 구조도 이다. WebApplication(WAR)마다 ClassLoader가 따로 존재한다. 그리고 상위 클래스 로더로 Common, System, BootStrap 가 계층형 구조로 존재한다.

알다시피 하위 계층의 클래스로더는 상위 계층의 클래스로더 자원을 참조할 수 있지만 상위 계층의 클래스로더는 하위 계층의 클래스로더의 자원을 참조할 수 없다.

결국 WebApplication 클래스로더간의 클래스는 서로 공유 되지 않는 다는 것이다. 따라서 Java Static Singleton의 공유범위는 WebApplication 내라고 보면 된다.

## Singleton in Spring

Sprng의 Singleton Scope이 ApplicationContext 기준이란 것은 무엇일까 ?

한개의 WebApplication 안에는 여러개의 Servlet이 등록될 수 있는데 Spring은 DispatcherServlet이라는 Servlet을 사용한다. 

당연히 DispatcherServlet도 여러개가 등록될 수 있으며 다음과 같은 구조를 구성한다.

![image](https://user-images.githubusercontent.com/79847020/135995540-2f7eba4e-3a83-41c0-a123-9881d2da5eb3.png)

일반적으로 SpringMVC에서는 contextConfigLocation으로 지정해주는 RootContext, Servlet으로 등록해주는 DispatcherServletContext 두가지가 있다.

DispatcherServlet은 각자 하나의 Context를 갖게 되며 여러 DispatcherServlet을 서블릿으로 등록해줄 경우 독립된 context를 구성하면서 서로간의 참조가 불가능해진다.

물론 classloader에서와 같이 상위 context인 root context는 서로 접근가능하다.

Spring의 Singleton은 하나의 Spring IoC Container 안에서 하나만 생성되고 공유된다. 즉 ApplicationContext에서만 공유된다는 의미이다.

web.xml에 RootContext 하나와 ServletContext 여러개를 선언할 수 있다고 했다.

이 각각의 context들이 Spring 기준의 Singleton 범위가 된다.



f



여러 객체들이 하나의 인스턴스를 공유한다는 개념을 같지만 

자바 Staic의 공유범위는 ClassLoader 기준이고 Spring Sington 공유범위는 ApplicationContext 기준이다. 
톰캣은 Web Application(WAR) 마다 ClassLoader를 따로 사용한다. Web Application간의 클래스들은 공유할 수 없다. 



Singleton 패턴은 클래스 로더당 특정 클래스의 인스턴스 하나만 생성됨을 나타냅니다. 스프링 싱글톤의 범위는 "빈당 용기당"으로 설명됩니다. 
스프링IoC 컨테이너당 단일 개체 인스턴스에 대한 빈 정의의 범위입니다. Spring의 기본 범위는 Singleton입니다. 
기본 범위가 싱글톤인 경우에도 <bean ../> 요소의 범위 속성을 지정하여 빈의 범위를 변경할 수 있습니다. <bean id=>.."class=".."scope="contines"/>

만약 스프링 컨테이너에 클래스 로더가 2개 이상 있다면, 각 클래스 로더는 자신의 인스턴스를 가질 것이다. "콩 1개당 1클래스 로더당 용기당"이라는 뜻입니까? 명확히 해 주세요!!

https://enterkey.tistory.com/300
