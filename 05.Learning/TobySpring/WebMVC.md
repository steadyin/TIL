# 스프링 프레임워크 핵심 기술

* 스프링 프레임워크란 ?

      소규모 애플리케이션 또는 기업용 애플리케이션을 자바로 개발하는데 있어 유용하고 편리한 기능을 제공하는 프레임워크

* IoC컨테이너 기능
  * 빈 설정
  * 빈 의존성 주입
  * Autowired
  * ComponentScan
  * EnvironmentCapable 
  * MessageSource
  * ApplicationEventPublisher
  * ResourceLoader

## IoC 컨테이너(ApplicationContext)

* IoC : Inversion of Control ? 
 
      의존 관계 주입 (Dependency Injection)

      어떤 객체가 사용하는 의존 관계 객체를 직접 만들어(new) 사용하는게 아니라 외부에서 주입 받아 사용하는 방법
      
* IoC 컨테이너 ?

      애플리케이션 컴포넌트의 중앙 저장소, Bean들이 담겨 있고 IoC기능을 제공하는 컨테이너
      빈 인스턴스 생성, 의존관계 설정, 빈 제공 한다.

* BeanFactory ?

      스프링 IoC 컨테이너 최상위 인터페이스
      
      BeanFactory 라이프사이클 인터페이스를 제공

* ApplicationContext 구현체 ?
      
  * ClassPathXmlApplicationContext (XML)
  * AnnotationConfigApplicationContext (JAVA)

* 컴포넌트 스캔 방법

  * XML 설정에서 <context:component-scan>
  * 자바 설정에서 @ComponentScan

* 빈 설정 방법

  * XML 파일을 통한 Bean 설정 방법(application.xml)
  * XML 파일에 Component-Scan 등록
  * JAVA 설정 파일을 이용한 Bean 설정 파일
  * JAVA 설정 파일을 이용한 Component-Scan
  * JAVA 코드로 수동 등록

* @Service
    
      MVC 패턴 중 Service 컴포넌트
    
* @Autowired

  * 필요한 의존 객체의 타입에 해당하는 빈을 찾아 주입한다.
  * 필드, Setter, 생성자 지정할 수 있다.
  * @Autowired(required = false)

  * @Autowired 중복 대상 처리

    * @Primary 
    * 한 개 이상의 Bean을 주입받는 방법
    * @Qualifier

  * @Autowired 경우의 수
        
        해당 타입의 Bean을 먼저 Scan하고 해당 타입의 Bean이 여러개인 경우 Bean이름 Scan해 주입하고, Bean이름으로도 주입할 수 없으면 에러가 발생한다.
        
  * @Autowired 동작원리

        @AutowiredAnnotationBeanPostProcessor은 BeanPostProcessor 라이프 사이클에서 수행되는 BeanPostProcessor구현체입니다.
        @AutowiredAnnotationBeanPostProcessor에 의해 @Autowired가 적용된 대상에 Bean을 찾아 주입합니다.
        postProcessBeforeInitialization methods of BeanPostProcessors

* BeanPostProcessor 

      InitializingBean 라이프 사이클(새로운 Bean Instance을 생성) 이전/이후에 부가적인 작업을 할 수 있는 라이프 사이클 인터페이스 입니다.

* InitializingBean

      Bean 인스턴스를 생성하는 라이프 사이클 인터페이스
      
* @PostConstruct

      InitializingBean's afterPropertiesSet 라이프사이클 때 실행되는 애노테이션
      
* @ComponentScan

      @ComponentScan이 적용 된 클래스를 포함한 패키지 이하 모든 @Component를 Scan을 동작시키는 애노테이션
      @SpringBootApplication 애노테이션에도 적용되어있다.
      @Repository, @Service, @Controller, @Configuration 등.. 이 적용대상이다.
      BeanFactoryPostProcessor의 구현체 ConfigurationClassPostProcessor에 의해 동작된다.

* BeanFactoryPostProcessor와 BeanPostProcessor는 비슷하지만 실행시점이 다르다(???)


* Bean의 스코프
  
  * 다음과 같이 사용할 수 있다. 
  ```JAVA 
    @Component @Scope("prototype")
  ```  
  * 싱글톤
  * 프로토 타입
    * Request
    * Session
    * WebSocket

*  Prototype Bean이 Singleton Bean을 참조하는 경우
  
  1. 프로토타입 Bean에 싱글톤 Bean을 주입하면 아무 문제가 발생하지 않습니다. 하지만 그 반대의 경우, 싱글톤 Bean에 프로토 타입 Bean이 주입한다는 말은 싱글톤 Bean을 사용할 때마다 프로토타입 Bean이 새로 생성되기를 의도한 것입니다. 하지만 그렇게 동작하지 않습니다. 처음에 싱글톤 Bean에 프로토타입 Bean을 주입하고 다시 싱글톤 Bean을 불러와 사용하면 기존에 사용하던 싱글톤 Bean이기 때문에 내부에 프로토타입 Bean은 그대로 입니다. 

  2. 해결 방법은 다음과 같이 프록시타입 Bean의 Scope에 프록시모드를 지정하는 것입니다. 
  ```JAVA 
    @Scope(value = "prototype", proxyMode = ScopedProxyMode.TARGET_CLASS)
  ```
  
  3. proxyMode = ScopedProxyMode.TARGET_CLASS 사용한 경우에는 CGLibrary를 사용한 Dynamic-Proxy가 적용이 됩니다. Proxy 모드를 사용한다는 것은 해당 클래스를 Proxy 클래스로 감싸는 것입니다. 싱글톤 오브젝트가 프로토타입 오브젝트를 직접 참조하면 참조값을 바꿔줄 수 있는 여지가 없습니다. 싱글톤 Bean의 참조변수값을 변경해서 프로토타입 Bean을 계속 바꾸어주어야 하는데 바꿀 수 있는 방법이 없기 때문에 Prototype 클래스를 상속받은 Proxy 클래스를 참조하게 하고 Proxy 클래스의 참조값을 바꿔주는 형태로 동작하게 됩니다.

* Singleton은 라이프 사이클이 길다. Scope이 넓은 Bean에서 짧은 Scope을 가진 Bean들을 주입할 때에는 항상 우리가 의도한대로 동작할 수 있는지 염두해두어야 한다.    

* CGLibrary의 프록시

  JDK 안에 존재하는 다이나믹 프록시는 인터페이스 기반 프록시를 생성하지만 CGLibrary의 프록시는 클래스도 프록시를 생성할 수 있습니다.
  
* 싱글톤 Bean은 오직 한개만 존재하지만, 여러 곳에서 사용되므로 프로퍼티의 값을 thread-safe하지 않습니다. 이 점을 주의해야 합니다.

* EnvironmentCapable

      ApplicationContext 인터페이스가 상속하는 EnvironmentCapable 인터페이스, 프로퍼티(Properties)와 프로파일(Profile)을 설정한다.
      
* Profile(프로파일)
 
      ApplicationContext extends EnvironmentCapable 
      빈들의 그룹, 빈들을 그룹을 사용하여 특정 환경을 구성하는데 사용한다.
      예를 들면 테스트 환경 빈 그룹, 운영 환경 빈 그룹을 사용할 수 있다.
      ApplicationContext가 상속한 Environment는 프로파일 설정하는 기능을 제공한다. 
  
  * @Profile("test") 애노테이션 설정
    
* 프로퍼티(Property)

      ApplicationContext extends EnvironmentCapable 
      프로퍼티는 다양한 방법으로 애플리케이션에 Key-Value쌍으로 정의 할 수 있는 설정 값을 의미한다. 
      ApplicationContext가 상속한 Environment는 프로퍼티를 설정하고 값에 접근할 수 있는 기능을 제공한다. 
      계층적(hierarchical)이기 때문에 Key가 같으면 우선순위에 따라 값이 결정됩니다.

  * Properties 파일 사용
    ```JAVA
    @SpringBootApplication
    @PropertySource("classpath:/app.properties")
    public class Demospring51Application {
    ```

* MessageSource

      ApplicationContext extends MessageSource 
      국제화 (i18n) 기능을 제공하는 인터페이스. 
      messages.properties, messages_ko_kr.properties를 사용할 수 있음
      
* ApplicationEventPublisher

      ApplicationContext extends ApplicationEventPublisher 
      스프링에서 이벤트 기반 프로그래밍을 할 수 있도록 제공해주는 인터페이스
      
  * ContextRefreshedEvent: ApplicationContext를 초기화 했더나 리프래시 했을 때 발생. 
  * ContextStartedEvent: ApplicationContext를 start()하여 라이프사이클 빈들이 시작 신호를 받은 시점에 발생. 
  * ContextStoppedEvent: ApplicationContext를 stop()하여 라이프사이클 빈들이 정지 신호를 받은 시점에 발생. 
  * ContextClosedEvent: ApplicationContext를 close()하여 싱글톤 빈 소멸되는 시점에 발생. 
  * RequestHandledEvent: HTTP 요청을 처리했을 때 발생.

* ResourceLoader

      ApplicationContext extends ResourceLoader 
      리소스를 읽어오는 기능
      
* Resource 폴더안에 있는 것들이 빌드할 때 Target > Classes(Classpath 루트) 폴더로 옮겨진다. 

## Resource 추상화

* Resource

      org.springframework.core.io.Resource 
      다양한 리소스 파일들을 추상화해서 스프링으로 코드로 가져와서 사용할 수 있는 인터페이스이다.
      java.net.URL을 확장해서 구현했는데  classpath: 같은 접우어를 지원한다.

* Resource 사용법
* 
  ```JAVA
  @Component
  public class AppRunner implements ApplicationRunner {
      @Autowired
      ResourceLoader resourceLoader;

      @Override
      public void run(ApplicationArguments args) throws Exception {
          System.out.println(resourceLoader.getClass());

          Resource resource = resourceLoader.getResource("classpath:test.txt");
          System.out.println(resource.getClass());

          System.out.println(resource.exists());
          System.out.println(resource.getDescription());
          System.out.println(Files.readString(Path.of(resource.getURI())));
      }
  }
  ```

## Validation 추상화

* Validation

      org.springframework.validation.Validator 
      애플리케이션에서 사용하는 객체 검증용 인터페이스. 

* Validation 사용법

```JAVA
public class EventValidastor implements Validator {
    @Override
    public boolean supports(Class<?> clazz) {
        return Event.class.equals(clazz);
    }

    @Override
    public void validate(Object o, Errors errors) {
        ValidationUtils.rejectIfEmptyOrWhitespace(errors, "title", "not empty");
    }
}

@Component
public class AppRunner implements ApplicationRunner {

    @Override
    public void run(ApplicationArguments args) throws Exception {
        Event event = new Event();
        EventValidator eventValidator = new EventValidator();

        Errors errors = new BeanPropertyBindingResult(event, "event");

        eventValidator.validate(event, errors);

        System.out.println(errors.hasErrors());
        //에러 가져온 후 에러 순차적으로 출력
        errors.getAllErrors().forEach(e -> {
            System.out.println("===== error code =====");
            Arrays.stream(e.getCodes()).forEach(System.out::println);
            System.out.println(e.getDefaultMessage());
        });
    }
}
```

* Validation Anotation, LocalValidatorFactoryBean

      LocalValidatorFactoryBean은 Bean Validation Anotation을 지원하는 Validator이다. 

```JAVA
public class Event {

    Integer id;

    @NotEmpty
    String title;

    @NotNull
    @Min(8) //@Size는 Collect 개수 검증
    Integer limit;

    @Email
    String email;
    ...
```

## PropertyEditor 

      데이터 바인딩 추상화: PropertyEditor 
      프로퍼티 값을 타겟 객체에 설정하는 기능 
      사용자 입력값을 애플리케이션 도메인 모델에 동적으로 변환해 넣어주는 기능. 
      PropertyEditor은 Object와 String간의 변환만 할 수 있습니다. 
      
```JAVA
@RestController
public class EventController {
    @GetMapping("/event/{event}")
    public String getEvent(@PathVariable Event event) {
        System.out.println(event);
        return event.getId().toString();
    }
}
```
* {event} -> @PathVariable Event event

## Converter

       S 타입을 T 타입으로 변환할 수 있는 매우 일반적인 변환기. 
        Converter<String, Event>는 String을 Event객체로 변환하겠다는 인터페이스입니다. 
   
* 구현 예   
```JAVA
public class EventConverter {
    public static class StringToEventConverter implements Converter<String, Event> {

        @Override
        public Event convert(String source) {
            return new Event(Integer.parseInt(source));
        }
    }

    public static class EventToStringConverter implements Converter<Event, String> {
        @Override
        public String convert(Event source) {
            return source.getId().toString();
        }
    }
}
```
* 등록 예
```JAVA
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addFormatters(FormatterRegistry registry) {
        registry.addConverter(new EventConverter.StringToEventConverter());
    }
}
```
* 사용 예
```JAVA
@RestController
public class EventController {
    @GetMapping("/event/{event}")
    public String getEvent(@PathVariable Event event) {
        System.out.println(event);
        return event.getId().toString();
    }
}
```


## Formatter

      Object와 String 간의 변환을 담당한다. 
      웹쪽같은 경우에 사용자 입력값이 문자열로 들어오고 객체들을 문자열로 내보내는 경우가 많습니다.
      그런 문자열들을 MessageSource(국가별언어변환)을 사용해서 내보내는 인터페이스입니다.
      ropertyEditor와 다른 것은 Locale정보를 기반으로 처리할 수 있다는 것입니다. 
      
* 구현
  ```JAVA
  @Component
  public class EventFormatter implements Formatter<Event> {
      @Autowired
      MessageSource messageSource;

      @Override
      public Event parse(String text, Locale locale) throws ParseException {
          return new Event(Integer.parseInt(text));
      }

      @Override
      public String print(Event object, Locale locale) {
          message.getMessage("title",local);

          return object.getId().toString();
      }
  }
  ```

* 등록
  ```JAVA
  @Configuration
  public class WebConfig implements WebMvcConfigurer {

      @Override
      public void addFormatters(FormatterRegistry registry) {
  //        registry.addConverter(new EventConverter.StringToEventConverter());
          registry.addFormatter((new EventFormatter()));
      }
  }
  ```

* 중요한 것은 Formatter와 Converter 빈은 자동으로 등록이 된다는 점입니다. Converter나 Formatter가 Bean으로 등록되어 있다면 스프링부트가 ConversionService에 자동으로 등록해줍니다.

## Null-safety

      스프링 프레임워크 5에 추가된 Null 관련 애노테이션
      컴파일 시점에 최대한 NullPointerException을 방지하는 것
  
* @NonNull
* @Nullable
* @NonNullApi (패키지 레벨 설정)
* @NonNullFields (패키지 레벨 설정)

## SpEL(Spring Expression Language)

      Spring이 제공하는 Expression Language

* 문법
  * #{"표현식"}
  ```JAVA
      @Value("#{ 1 + 2 }")
      int value;

      @Value("#{'hello ' + 'world'}")
      String greeting;

      @Value("#{1 eq 1}")
      boolean trueOrFalse;
  ```
  * ${"프로퍼티"}
    * 표현식은 프로퍼티를 안에서 사용할 수 있다.
  ```JAVA
  @Value("${my.value}")
  int myValue;

  @Value("#{${my.value} eq 100}")
  boolean isMyValue100;
  ```
  
  * 레퍼런스
    
    * 객체 참조를 지원합니다.
    * Properties, Arrays, Lists, Maps, and Indexers를 지원합니다.
    * Bean 또한 참조할 수 있습니다.
  ```JAVA
  @Component
  public class Sample {
      public int data = 99;
  }

  @Component
  public class AppRunner implements ApplicationRunner {
      @Value("#{sample.data}")
      int sampleValue;

      @Override
      public void run(ApplicationArguments args) throws Exception {
          System.out.println(sampleValue);
      }
  }
  ```

## AOP

# 스프링 웹 MVC

* @Controller

      애노테이션을 이용해서 MVC패턴의 Controller역할을 하는 클래스임을 정의한다.

* Model 객체

      Java.Collection.Map이라고 생각하면 된다. Model 객체는 화면에 전달해줄 데이터를 담으면 됩니다. Key-Value형식으로 담아주면 된다. 
      model.addAttribute("name", value);

* @RequestMapping

      애노테이션을 이용해서 특정한 요청이 들어올 때 요청을 처리하는 핸들러가 됩니다. 

* @RequestMapping(value = "/evens", method = RequestMethod.GET) 
 
      RequestMethod.Get요청으로 /event라는 요청 핸들러

* @GetMapping(value = "/event")
 
      @GetMapping 위에 @RequestMapping 애노테이션이 설정되어있는 것을 볼 수 있다.

* Lombok 애노테이션 
      
      @Getter @Setter @Builder @NoArgsConstructor @AllArgsConstructor 
      컴파일 시점에 자동으로 생성해준다.

* @Service 
 
      서비스 애노테이션

* @Autowired 

      빈 자동 주입

* builder() 

      객체 생성을 좀 더 편리하게 생성할 수 있는 메소드 지원
      
* 동적인 View는 /resource/template/ 에서 해당 파일을 찾습니다.

* Model1 모델

  * View/Controller + Model 로 구성 된 모델

* MVC 모델

  * MODEL 모델 

        도메인 객체 또는 DTO로 화면에 전달할 또는 화면에서 전달 받은 데이터를 담고 있는 객체(JAVA POJO객체)

  * VIEW 뷰

        뷰로 전달되는 데이터를 어떻게 보여줄지 결정 역할, 다양한 형태로 보여줄 수 있다.(HTML, JSP, Thymeleaf)

  * Controller 컨트롤러

        사용자 입력을 받아 Model 객체의 데이터를 변겨하거나 Model 객체를 View에 전달하는 역할

  * MVC 패턴의 장점

    * 동시 다발적(Simultaneous) 개발 - 백엔드 개발자와 프론트 엔드 개발자가 독립적으로 개발을 진행할 수 있다.
    * 높은 결합도 - 논리적으로 관련있는 기능을 하나의 컨트롤러로 묶거나 특정 모델과 관련있는 뷰를 그룹화 할 수 있다. 
    * 낮은 의존도(decoupling, loosely coupling) - View, Model, Controller는 각각 독립적이다.
    * 개발 용이성 - 책임이 구분되어 있어 코드 수정하는 것이 
    
  * MVC 패턴 단점

    * 코드 네비게이션 복잡함, MVC구조를 다 살펴보아야 한다.
    * 코드 일관성 유지에 노력이 필요함
    * 높은 학습 곡선

* @Controller에서 기본 문자열 return -> View를 찾는다.

* 서블릿(Servlet)

      자바 엔터프라이즈 에디션은 Java 웹 애플리케이션 개발용 스펙, API 제공
      요청 당 프로세스내 자원을 공유하는 스레드를 생성해서 사용하기 때문에 효율적이다.
      HttpServlet, HttpServletRequest, HttpServletResponse 클래스가 사용된다.
      
* 서블릿 엔진, 서블릿 컨테이너

      우리가 작성한 서블릿 프로그램은 직접 실행할 수 없고 서블릿 컨테이너를 사용해야만 한다. 
      서블릿의 라이프사이클을 관리하는 서블릿 스팩을 준수하는 컨테이너 
  
  * 기능
    * 세션 관리
    * 네트워크 서비스
    * MIME 기반 메시지 인코딩 디코딩
    * 서블릿 생명주기 관리
    * 톰캣, 제티
  
* 서블릿 생명주기  

  * init() : 서블릿 컨테이너가 서블릿 인스턴스를 생성하고 init() 메소드를 호출하여 초기화한다.
    * 최초 요청을 받았을 때 한번 초기화 하고 나면 그 다음 요청부터는 이 과정을 생략한다.
    * 웜업 : 자주 사용하거나 특정 서블릿을 초기에 서블릿 인스턴스를 생성해놓는 작업
  
  * service() : 서블릿이 초기화 된 다음부터 클라이언트의 요청을 처리할 수 있다. 각 요청은 별도의 스레드로 처리하고 이때 서블릿 인스턴스의 service() 메소드를 호출한다.
    * HTTP 요청과 HTTP 응답을 생성한다.
    * service()는 HTTP Method에 따라 doGet()과 doPost() 등으로 처리를 위임한다.
  
  * destory() : 서블릿 컨테이너 판단에 따라 서블릿을 메모리에서 내려야할 시점에 destroy()를 호출한다.

* 서블릿 애플리케이션 개발

* Servlet 클래스

  ```JAVA
  public class HelloServlet extends HttpServlet {
      @Override
      public void init() throws ServletException {
          System.out.println("init");
      }

      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          System.out.println("doGet");
          resp.getWriter().write("Hello Servlet");
      }

      @Override
      public void destroy() {
          System.out.println("destroy");
      }
  }
  ```

* Deployment 

      Application을 톰캣에서 가동할 때 배포하는 방법 설정

* pom.xml Servlet 의존성 추가

  ```XML
  <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
  <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>4.0.1</version>
      <scope>provided</scope>
  </dependency>
  ```
* web.xml Servlet 등록
  ```XML
  <!DOCTYPE web-app PUBLIC
          "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
          "http://java.sun.com/dtd/web-app_2_3.dtd" >

  <web-app>
      <display-name>Archetype Created Web Application</display-name>
      <servlet>
          <servlet-name>hello</servlet-name>
          <servlet-class>me.stead95.HelloServlet</servlet-class>
      </servlet>

      <servlet-mapping>
          <servlet-name>hello</servlet-name>
          <url-pattern>/hello</url-pattern>
      </servlet-mapping>
  </web-app>
  ```
  
* 서블리 리스너

      서블릿 리스너는 애플리케이션 Servlet 컨테이너에서 발생한 이벤트 서블릿 라이프 사이클 변화, 애트리부트의 변화
      세션의 변화 같은 주요 이벤트를 감지하고 각 이벤트에 특별한 작업 실행이 필요한 경우 사용할 수 있는 기술스팩
      
  * 서블릿 종류

    * 서블릿 컨텍스트 수준의 이벤트
      * 컨텍스트 라이프 사이클 이벤트(ServletContextListener -> contextInitialized() contextDestroyed()
      * 컨텍스트 애트리뷰트 변경 이벤트
    * 세션 수준의 이벤트
      * 세션 라이프사이클 이벤트
      * 세션 애트리뷰트 변경 이벤트
 
 * 서블릿 리스너 등록
    ```XML
    <listener>
      <listener-class>me.stead95.MyListener</listener-class>
    </listener>
    ```
      
* 서블릿 필터

      들어온 요청을 서블릿으로 보내고 서블릿이 작성한 응답을 클라이언트로 보내기 전에 특별한 처리가 필요한 경우 사용
      체인 형태의 구조
  * 필터 등록
    ```XML
    <filter>
        <filter-name>myFilter</filter-name>
        <filter-class>me.stead95.MyFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>myFilter</filter-name>
        <servlet-name>hello</servlet-name>
    </filter-mapping>
    ```



     

# 18. 스프링 MVC 구성 요소

![image](https://user-images.githubusercontent.com/79847020/140864373-6245a80e-0abc-497c-a4ae-50221813b0b8.png)

* MultipartResolver
* LocaleResolver
* ThemeResolver
* HandlerMappings
* HandlerAdapters
* HandlerExceptionResolvers
* RequestToViewNameTranslator
* ViewResolvers
* FlashMapManager

## DispatcherServlce의 기본 전략

* DispatcherServlet.properties 참조

## MultipartResolver

* 파일 업로드 요청 처리에 필요한 인터페이스
* HttpServletRequest를 MultipartHttpServletRequest로 변환해주어 요청이 담고 있는 FILE을 꺼낼 수 있는 API 제공
 
## LocaleResolver

* 클라이언트의 위치(Locale) 정보를 파악하는 인터페이스
* 기본 전략은 Request의 accept-language를 보고 판단

## ThemeResolver

* 
## HandlerMappings
## HandlerAdapters
## HandlerExceptionResolvers
## RequestToViewNameTranslator
## ViewResolvers
## FlashMapManager

  

















# 스프링 MVC 설정

# 스프링 MVC 활용
