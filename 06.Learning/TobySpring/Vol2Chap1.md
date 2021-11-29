# 1. IoC컨테이너와 DI

1장에서는 스프링이 제공하는 DI 설정 메타데이터를 다루는 여러 가지 방법을 살펴보고 상황에 맞게 어떤 설정 방시고가 응용 기술을 선택할지에 대해서 알아본다.

# 1.1 IoC 컨테이너 : 빈 팩토리와 애플리케이션 컨텍스트

스프링 애플리케이션에서는 오브젝트의 생성과 관계설정, 사용, 제거 등의 작업을 애플리케이션 코드 대신 독립된 컨테이너가 담당한다. 컨테이너가 코드 대신 오브젝트에 대한 제어권을 갖고 있다고 해서 IoC라고 부른다.

스프링에선 IoC를 담당하는 컨테이너를 빈 패곹리 또는 애플리케이션 컨텍스트라고 부르기도 한다. 오브젝트의 생성과 오브젝트 사이의 런타임 관계를 설정하는 DI 고나점으로 볼 때 컨테이너를 빈 팩토리라고 한다. DI를 위한 빈 팩토리에 엔터프라이즈 애플리케이션을 개발하는데 필요한 여러 가지 컨테이너 기능을 추가한 것을 애플리케이션 컨텍스트라고 부른다. 즉 애플리케이션 컨텍스트는 그 자체로 IoC와 DI을 위한 빈 팩토리이면서 그 이상의 기능을 가졌다고 보면 된다.

스프링의 빈 팩토리와 애플리케이션 컨텍스트는 각각 기능을 대표하는 BeanFactory와 ApplicationContext라는 두 개의 인터페이스로 정의되어 있다. ApplicationContext 인터페이스는 BeanFactory 인터페이스를 상속한 서브 인터페이스이다. 

```JAVA
public interface ApplicationContext extends EnvironmentCapable, ListableBeanFactory, HierarchicalBeanFactory
                                          , MessageSource, ApplicationEventPublisher, ResourcePatternResolver {
```

실제로 스프링 컨테이너 또는 IoC컨테이너라고 말하는 것은 이 ApplicationContext 인터페이스를 구현한 클래스의 오브젝트이다. 스프링 애플리케이션은 최소한 하나 이상의 IoC 컨테이너 즉 애플리케이션 컨텍스트 오브젝트를 갖고 있다. 

## 1.1.1 IoC 컨테이너를 이용해 애플리케이션 만들기

간단하게 IoC 컨테이너를 만드는 방법은 다음과 같이 ApplicationContext 구현 클래스의 인스턴스를 만드는 것이다.

```
StaticApplicationContext ac = new StaticApplicationContext();
```

이렇게 만들어진 컨테이너가 본격적인 IoC 컨테이너로서 동작하려면 두 가지가 필요하다. 바로 POJO 클래스와 설정 메타 정보다.

### POJO 클래스

애플리케이션의 핵심 코드를 담고 있는 POJO 클래스를 준비해야 한다. POJO 원칙을 따라 애플리케이션 코드를 작성한다. 각각의 POJO 특정 기술과 스팩에서 독립적일뿐더러 의존관계에 있는 다른 POJO와 느슨한 결합을 갖도록 만들어야 한다. 

각자 기능에 충실하게 독립적으로 설계된 POJO 클래스를 만들고 결합도가 낮은 유연한 관계를 가질 수 있도록 인터페이스를 이용해 연결해주는 것 까지가 IoC 컨테이너가 사용할 POJO를 준비하는 첫 단계다.

### 설정 메타 정보

두 번째 필요한 것은 앞에서 만든 POJO 클래스들 중에 애플리케이션에서 사용할 것을 선정하고 이를 IoC컨테이너가 제어할 수 있도록 적절한 메타 정보를 만들어 제공하는 작업이다. IoC 컨테이너의 가장 기초적인 역할은 오브젝트를 생성하고 이를 관리하는 것이다. 스프링 컨테이너가 관리하는 이런 오브젝트는 Bean이라고 부른다. IoC 컨테이너가 필요로 하는 설정 메타 정보는 바로 이 빈을 어떠헥 만들고 어떻게 동작하게 할 것인가에 관한 정보다.

스프링 설정 메타 정보는 BeanDefinition 인터페이스로 표현되는 순수한 추상 정보다. 스프링 IoC 컨테이너, 즉 애플리케이션 컨텍스트는 바로 이 Definition으로 만들어진 메타정보를 담은 오브젝트를 사용해 IoC와 DI 작업을 수행한다. 따라서 스프링의 메타정보는 특정한 파일 포맷이나 형식에 제한되거나 종속되지 않는다. XML이든 소스코드 애노테이션이든 자바 코드이든 프로퍼티 파일이든 상관없이 BeanDefinition으로 정의되는 스프링 설정 메타정보의 내용을 표현한 것이 있다면 무엇이든 사용 가능하다. 원본의 포맷과 구조 자료의 특성에 맞게 읽어와 BeanDefinition 오브젝트로 변환해주는 BeanDefinitionReader가 있으면 된다. 

빈 메타정보는 빈 아이디, 이름, 별칭, 클래스, 스코프, 프로퍼티 값 또는 참조, 생성자 파라미터 값 또는 참조, 지연된 로딩 여부, 우선 빈 여부, 자동와이어링 여부 등이 있다.

스프링 IoC컨테이너는 각 빈에 대한 정보를 담은 설정 메타정보를 읽어들인 뒤에 이를 참고해서 빈 오브젝트를 생성하고 프로퍼티나 생성자를 통해 의존 오브젝트를 주입해주는 DI 작업을 수행한다. 이 작업을 통해 만들어지고 DI로 연결되는 오브젝트들이 모여서 하나의 애플리케이션을 구성하고 동작하게 된다. IoC 컨테이너의 역할은 바로 이것이다. 

![image](https://user-images.githubusercontent.com/79847020/143800756-21d9f134-76d7-408f-93ce-0d4037850857.png)

결국 스프링 애플리케이션이란 POJO 클래스와 설정 메타정보를 이용해 IoC 컨테이너가 만들어주는 오브젝트의 조합이라고 할 수 있다. 

```JAVA
StaticApplication ac = new StaticApplicationContext();
ac.registerSingleton("hello1", Hello.class);

Hello hello1 = ac.getBean("hello1", Hello.class);
assertThat(hello1, is(notNullValue()));
```

StaticApplicationContext는 코드에 의해 설정 메타정보를 등록하는 기능을 제공하는 애플리케이션 컨텍스트다. 테스트용으로 사용하기에 적당하다. IoC컨테이너가 관리하는 빈은 오브젝트 단위지 클래스 단위가 아니라는 사실을 기억해야한다. 경우에 따라서 하나의 클래스를 여러 개의 빈으로 등록하기도 한다.

이번에는 BeanDefinition 타입의 설정 메타정보를 만들어서 IoC컨테이너에 등록하는 방법을 사용해보자. RootBeanDefinition은 가장 기본적인 BeanDefinition 인터페이스의 구현 클래스다.

BeanDefinition을 이용한 빈 등록
```JAVA
BeanDefinition helloDef = new RootBeanDefinition(Hello.class); 
helloDef.getPropertyValues().addPropertyValue("name", "String");
ac.registerBeanDefinition("hello2", helloDef);
```

IoC 컨테이너는 빈 설정 메타정보를 담은 BeanDefinition을 이용해 오브젝트를 생성하고 DI 작업을 진행한 뒤에 빈으로 사용할 수 있도록 등록해준다. 

이번에는 Hello 타입의 빈과 StringPrinter 타입의 빈을 hello와 printer라는 빈 이름으로 생성하고 printer 빈이 hello 빈에게 DI 되도록 만들어보자. addPropertyValue() 메소드에는 값 외에도 BeanReference타입의 레퍼런스 오브젝트를 넣을 수 있다.

DI정보 테스트
```JAVA
@Test
public void registerBeanWithDependency() {
  StaticApplicationContext ac = new StaticApplicationContext();
  ac.registerBeanDefinition("printer", new RootBeanDefinition(StringPrinter.class));
  
  BeanDefinition helloDef = new RootBeanDefinition(Hello.class);
  helloDef.getPropertyValues().addPropertyValue("name", "Spring");
  helloDef.getPropertyValues().addPropertyValue("printer", new RuntimeBeanReference("printer"));
  ac.registerBeanDefinition("Hello", helloDef);
  
  Hello hello = ac.getBean("hello", Hello.class);
  hello.print();
  
  assertThat(ac.getBean("printer").toString(), is("Hello Spring"));
}
```

## 1.1.2 Ioc 컨테이너의 종류와 사용 방법

ApplicationContext 인터페이스를 구현했다면 어떤 클래스든 스프링의 IoC 컨테이너로 사용할 수 있다. 이미 스프링에는 다양한 용도로 쓸 수 있는 십여 개의 ApplicationContext 구현 클래스가 존재한다. 

### StaticApplicationContext

코드를 통해 빈 메타정보를 등록하기 위해 사용, 학습 테스트를 만들 때를 제외하면 실제로 사용되지 않는다.

### GenericApplicationContext

가장 일반적인 애플리케이션 컨텍스트의 구현 클래스다. XML파일과 같은 외부의 리소스에 있는 빈 설정 메타정보를 리더를 통해 읽어들여서 메타 정보로 전환해서 사용한다. XML 파일과 같은 외부의 리소스에 있는 빈 설정 메타정보를 리더를 통해 읽어들여서 메타정보로 전환해서 사용한다.

* BeanDefinitionReader 
  
  특정 포맷의 빈 설정 메타정보를 읽어서 이를 애리케이션 컨텍스트가 사용할 수 있는 BeanDefinition 정보로 변환하는 기능을 가진 인터페이스. 빈 설정정보 리더라 불린다. 대표적인 빈 설정정보 리더는 XmlBeanDefinitionReader다. PropertiesBeanDefinitionReader 도 있다.

```JAVA
@Test
public void genericApplicationContext() {
  GenericApplicationContext ac = new GenericApplicationContext();
  XmlBeanDefinitionReader reader = new XmlBeanDefinitionReader(ac);

  reader.loadBeanDefinitions("springbook/learnigtest/spring/ioc/genericApplicationContext.xml");

  ac.refresh();
  
  Hello hello = ac.getBean("hello", Hello.class);
  hello.print();
  
  assertThat(ac.getBean("printer").toString, is("Hello Spring")); }
```

스프링에서는 대표적으로 XML 파일, 자바 소스코드 애노테이션, 자바 클래스 세 가지 방시긍로 빈 설정 메타 정보를 작성할 수 있다. 스프링은 처음부터 단 한 번도 특정 포맷의 설정에 종속되거나 제한된 적이 없다. 

GenericApplicationContext는 빈 설정 리더를 여러 개 사용해서 여러 리소스로부터 설정 메타정보를 읽어들이게도 할 수 있다. 모든 설정정보를 가져온 후에 refresh() 메소드를 한 번 호출해서 애플리케이션 컨텍스트가 필요한 초기화 ㅈ가업을 수행하게 해주면 된다.

스프링 테스트 컨텍스트 프레임워크를 활용하는 JUnit 테스트는 테스트내에서 사용할 수 있도록 애플리케이션 컨텍스트를 자동으로 만들어준다. 이 때 생성되는 애플리케이션 컨텍스트가 바로 GenericApplicationContext다. 다음과 같이 테스트 클래스를 만들었다면 테스트가 실행되면서 GenericApplicationContext가 생성되고 @ContextConfiguration에 지정한 XML 파일로 초기화돼서 테스트 내에서 사용할 수 있도록 준비된다.

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "/test-applicationContext.xml")
```JAVA
public class UserServiceTest {
    @Autowired
    ApplicationContext applicationContext;
```

### GenericXmlApplicationContext

GenericApplicationContext과 XmlBeanDefinitionReader가 결합된 형태

### WebApplicationContext

스프링 애플리케이션에서 가장 많이 사용되는 애플리케이션 컨텍스트는 바로 WebApplicationContext다. 이름 그대로 웹 환경에서 사용할 때 필요한 기능이 추가된 애플리케이션 컨텍스트다. 스프링 애플리케이션은 대부분 서블릿 기반의 독립 웹 애플리케이션(WAR)으로 만들어지기 때문이다. 가장 많이 사용되는 건 XML 설정파일을 사용하도록 만들어진 XMLWebApplicationContext다. 애노테이션을 이용한 설정 리소스만 사용한다면 AnnotationConfigWebApplicationContext를 쓰면 된다.

WebApplicationContext의 사용 방법을 이해하려면 스프링의 IoC컨테이너를 적용했을 때 애플리케이션을 가동시키는 방법에 대해 살펴볼 필요가 있다. 스프링 IoC컨테이너는 빈 설정 메타정보를 이용해 빈 오브젝트를 만들고  DI 작업을 수행한다. 하짐나 그것만만으로 애플리케이션이 동작하지 않는다. 마치 자바 애플리케이션의 main() 메서드처럼 어디에선가 특정 빈 오브젝트를 호출함으로써 애플리케이션을 동작시켜야 한다. 

보통 이런 기동 역할을 맡은 빈을 사용하려면 IoC컨테이너에서 요청해서 빈 오브젝트를 가져와야 한다. 그래ㅓㅅ 간단한 스프링 애플리케이션을 만들고 IoC컨테이너를 직접 셋업했다면 다음 코드가 반드시 등장한다.

스프링 애플리케이션의 기동 코드
```JAVA
ApplicationContext ac = ...
Hello hello = ac.getBean("hello", Hello.class); // IoC컨테이너가 구성한 빈 오브젝트로 이뤄진 애플리케이션을 기동할 오브젝트를 getBean()으로 가져온다.
hello.print();
```

적어도 한 번은 IoC 컨테이너에게 요청해서 빈 오브젝트를 가져와야 한다.(그 Bean을 통해서 다른 관련 Bean을 호출한다고 생각하면 이해 하자). IoC 컨테이너의 역할은 이렇게 초기에 빈 오브젝트를 생성하고 DI 한후에 최초로 애플리케이션을 기동할 빈 하나를 제공해주는 것까지다. 

웹 애플리케이션은 동작하는 방식이 근본적으로 다르다. 독립 자바 프로그램은 자바 VM에게 main() 메소드를 가진 클래스를 시작시켜 달라고 요청할 수 있다. 하지만 웹에서는 main() 메소드를 호출할 방법이 없다. 게다가 사용자도 여럿이며 동시에 웹 애플리케이션을 사용한다. 

그래서 웹 환경에서는 main() 메소드 대신 서블릿 컨테이너가 브라우저로부터 오는 HTTP 요청을 받아서 해당 요청에 매핑되어 있는 서블릿을 실행해주는 방식으로 동작한다. 서블릿이 일종의 main() 메소드와 같은 역할을 하는 것이다. 

그렇다면 웹 애플리케이션에서 스프링 애플리케이션을 기동시키는 방법은 무엇일까? 일단 main() 메소드 역할을 하는 서블릿을 만들어두고, 미리 애플리케이션 컨텍스트를 생성해둔 다음 요청이 서블릿으로 들어올 때마다 getBean() 으로 필요한 빈을 가져와 정해진 메소드를 실행해주면 된다.

![image](https://user-images.githubusercontent.com/79847020/143810185-983171ef-376b-4958-9879-1a475ffed6b2.png)

서블릿 컨테이너는 브라우저와 같은 클라이언트로부터 들어오는 요청을 받아서 서블릿을 동작시켜주는 일을 맡는다. 서블릿은 웹 애플리케이션이 시작될 때 미리 ㅁ나들어둔 웹 애플리케이션 컨텍스트에게 빈 오브젝트로 구성된 애플리케이션의 기동 역할을 해줄 빈을 요청해서 받아둔다. 그리고 지정된 메소드를 호출함으로서 스프링 컨테이너가 DI 방식으로 구성해둔 애플리케이션의 기능이 시작되는 것이다.

다행히도 스프링은 이런 웹환경에서 애플리케이션 컨텍스트를 생성하고 설정 메타정보로 초기화해주고 클라이언트로부터 들어오는 요청마다 적절한 빈을 찾아서 이를 실행해주는 기능을 가진 DispatcherServlcet이라는 이름의 서블릿을 제공한다. 스프링이 제공해준 서블릿을 web.xml에 등록하는 것ㅁ나으로 웹 환경에서 스프링 컨테이너가 만들어지고 애플리케이션을 실행하는데 필요한 대부분의 준비는 끝이다.

웹 애플리케이션에서 만들어지는 스프링 IoC 컨테이너는 WebApplicationContext 인터페이스를 구현한 것임을 기억해두자. 

## 1.1.3 IoC 컨테이너 계층구조

한 개 이상의 IoC 컨테이너를 만들어두고 사용해야 할 경우가 있다. 바로 트리 모양의 계층 구조를 만들 때다.

### 부모 컨텍스트를 이용한 계층구조 효과

![image](https://user-images.githubusercontent.com/79847020/143811125-3ba9b905-5194-4ac4-93d6-8dd09b81a080.png)

계층구조 안의 모든 컨텍스트는 각자 독립적인 설정정보를 이용해 빈 오브젝트를 만들고 관리한다. 각자 독립적으로 자신이 관리하는 빈을 갖고 있긴 하지만 DI를 위해 빈을 찾을 때는 부모 애플리케이션 컨텍스트의 빈까지 모두 검색한다. 먼저 자신이 관리하는 빈 중에서 피룡한 빈을 찾아보고 없으면 부모 컨텍스트에게 빈을 찾아달라고 요청한다. 중요한건 자신의 부모 컨텍스트에게만 빈 검색을 요청하지 부모의 다른 자식 컨텍스트에게는 요청하지 않는다는 점이다. 

검색 순서는 항상 자신이 먼저이고 다음 직계 부모의 순서다. 같은 이름의 빈이 있다면 자신이 가진 것이 우선이고 부모 컨텍스트가 정의한 것은 무시된다. 

계층 구조를 이용하는 또 한 가지 이유는 여러 애플리케이션 컨텍스트가 공유하는 설정을 만들기 위해서다. 애플리케이션 안에 성격이 다른 설정을 분리해서 두 개 이상의 컨텍스트를 구성하면서 각 컨텍스트가 공유하고 싶은 게 있을 때 계층구조를 이용한다. 

애플리케이션 컨텍스트의 계층구조를 사용할 때는 주의하지 않으면 자칫 예상치 못한 방식으로 동작할 수도 있기 때문에 조심해야 한다. AOP처럼 컨텍스트 안의 많은 빈에 일괄적으로 적용되는 기능은 대부분 해당 컨텍스트로 제한된다는 점도 주의하자. 

### 컨텍스트 계층구조 테스트

## 1.1.4 웹 애플리케이션의 IoC 컨테이너 구성

서버에서 동작하는 애플리케이션에서 스프링 IoC 컨테이너를 사용하는 방법은 크게 세가지로 구분해볼 수 있다. 두 가지 방법은 웹 모듈 안에서 컨테이너를 두는 것이고 나머지 하나는 엔터프라이즈 애플리케이션 레벨에 두는 방법이다. 먼저 웹 애플리케이션 안에 WebApplicationContext 타입의 IoC 컨테이너를 두는 방법을 알아보자.

* 프론트 컨트롤러 패턴

몇 개의 서블릿이 중앙집중식으로 모든 요청을 다 받아서 처리하는 방식

웹 애플리케이션 안에서 동작하는 IoC컨테이너는 두가지 방법으로 만들어진다. 하나는 스프링 애플리케이션의 요청을 처리하는 서블릿 안에서 만들어지는 것이고 다른 하나는 웹 애플리케이션 레벨에서 만들어지는 것이다. 그래서 스프링 웹 애플리케이션에는 두 개의 컨테이너 즉 WebApplicationContext 오브젝트가 만들어진다. 
