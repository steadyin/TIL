# 스프링 입문

# 1. 프로젝트 환경 설정

* 스프링 부트 스타터 페이지 : http://start.spring.io
* 스냅샵(SnapShop) : 아직 배포되지 정식 발표되지 않은 버전을 말한다.
* Maven, Gradle : 필요한 라이브러리를 가져오고 빌드 라이프 사이클을 관리하는 도구이다.
* Project Meta정보
  * groupdId : 기업
  * artifactId : 프로젝트명
* build.gradle
  ```text
  plugins {
    id 'org.springframework.boot' version '2.4.4'
    id 'io.spring.dependency-management' version '1.0.11.RELEASE'
    id 'java'
  }
  
  group = 'hello'
  version = '0.0.1-SNAPSHOT'
  sourceCompatibility = '11'
  
  configurations {
    compileOnly {
      extendsFrom annotationProcessor
    }
  }
  
  repositories {
    mavenCentral() // 라이브러리 다운로드 받는 곳
  }
  
  dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
  }
  
  test {
    useJUnitPlatform()
  }
  ```
* .gitignore : 필요없는 소스코드 관리
* 기본 스프링부트 라이브러리 구조
  * spring-boot-starter-web
    * spring-boot-starter-tomcat : 톰캣
    * spring-webmvc : 스프링 웹 MVC
  * spring-boot-starter-thymeleaf : 타임리프 템플릿 엔진
  * spring-boot-starter(공통) : 스프링 부트 + 스프링 코어 + 로깅
    * spring-boot
      * spring-core
    * spring-boot-starter-logging
      * log-back, slf4j
* 테스트 라이브러리
  * spring-boot-starter-test
    * junit : 테스트 프레임워크
    * mockito : 목 라이브러리
    * assertJ : 테스트 코드를 좀 더 편하게 작성하게 도와주는 라이브러리
    * spring-test : 스프링 통합 테스트 지원
* Intellij 기본 설정
  * 빌드 설정
    * Preferences -> Build,Execution,Deployment -> Build Tools -> Gradle
    * Build and run using : Gradle -> Intellij IDEA
    * Runt tests using : Gradle -> Intellij IDEA
      ![img](https://user-images.githubusercontent.com/79847020/156157437-46f7f46b-de75-4d10-a4f2-5a77db96a06a.png)

  * 단축키 설정법
    * Preferences -> Keymap <br>
      ![img_1](https://user-images.githubusercontent.com/79847020/156157458-2078caa2-8f5d-4e66-b1c7-eb6ca75e628c.png)

  * JDK 설정
    * Project Structure(Ctrl + Alt + Shift + S) <br>
      ![img_2](https://user-images.githubusercontent.com/79847020/156157481-4104f658-19e3-4e1e-8510-51947b2803e8.png)

  * Html Recompile
    * spring-boot-devtools 라이브러리르 추가하면 html 파일을 컴파일만 해주면 서버 재시작 없이 View 파일 변경이 가능하다.
* 빌드하고 실행하기
  * Console창 gradlew build
* 윈도우에서 Git Bash 경험하기 <br>
  참조하기 : https://violetboralee.medium.com/intellij-idea%EC%99%80-git-bash-%EC%97%B0%EB%8F%99%ED%95%98%EA%B8%B0-63e8216aa7de

# 2. 스프링 웹 개발 기초

## 2.1 Welcome Page

기본경로 : static/index.html

레퍼런스 : http://docs.spring.io/spring-boot/docs/2.3.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-welcome-page

## 2.2 정적 컨텐츠

http://localhost:8080/hello-static.html

![img_4](https://user-images.githubusercontent.com/79847020/156157503-768f07ea-aa6b-48b8-afbe-dcc8ee248b27.png)

요청 시 hello-static 관련 컨트롤러가 존재하지 않다는 것을 확인 후 static에서 자원을 조회한다.

http://docs.spring.io/spring-boot/docs/2.3.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-static-content

## 2.3 MVC와 템플릿 엔진

http://localhost:8080/hello-mvc?name=spring

![img_5](https://user-images.githubusercontent.com/79847020/156157519-3afa26eb-68ef-4abb-a1bc-ffe8d176e867.png)

HelloController
```JAVA
@Controller
public class HelloController {
    @GetMapping("hello-mvc")
    public String helloMvc(@RequestParam("name") String name, Model model) {
        model.addAttribute("name", name);
        return "hello-template";
    }
}
```

요청 시 hello-mvc에 대한 컨트롤러가 존재하므로 반환한 "hello-template" 문자열을 기반으로 "template/hello-template.html"을 찾아가 템플릿엔진이 처리한다.

## 2.4 API

http://localhost:8080/hello-string?name=spring

![img_6](https://user-images.githubusercontent.com/79847020/156157528-8e7e41f0-a621-46a8-8c30-5dca32f16583.png)

* @ResponseBody
  * HTTP Body에 문자열로 직접 반환 
  * ViewResolver 대신 HttpMessageConver가 동작  

* HttpMessageConverter
  * 기본 문자 처리 : StringHttpMessageConverter
  * 기본 객체 처리 : MappingJacksong2HttpMessageConverter
  * 다양한 종류의 HttpMessageConverter가 기본으로 등록 되어 있음
  * 참고 : 클라이언트이 HttpAccept 헤더와 컨트롤러 반환 타입 정보를 조합해서 상황에 맞는 HttpMessageConverter가 선택된다.

* Jackson2 : 객체를 JSON으로 변환하는 라이브러리
* Gson : 구글이 제공하는 라이브러리


### 2.4.1 @ResponseBody 문자열 반환

http://localhost:8080/hello-string?name=spring

문자열이 StringHttpMessageConverter에 의해 HttpBody에 반환된다.

```JAVA
@Controller
public class HelloController {
    @GetMapping("hello-string")
    @ResponseBody
    public String helloMvc(@RequestParam("name") String name) {
        return "hello " + name;
    }
}
```

### 2.4.2 @Response 객체 반환

http://localhost:8080/hello-api?name=spring

객체가 MappingJacksong2HttpMessageConverter에 의해 json으로 컨버전되어 HttpBody에 반환된다.

```JAVA

@Controller
public class HelloController {
    @GetMapping("hello-api")
    @ResponseBody
    public Hello helloApi(@RequestParam("name") String name) {
        Hello hello = new Hello();
        hello.setName(name);
        return hello;
    }
}
```

# 3. 회원 관리 예제 - 백엔드 개발

일반적인 웹 애플리케이션 계층 구조

![img_7](https://user-images.githubusercontent.com/79847020/156157541-0bd771d1-9007-454d-bbd5-9c14c54ecf2f.png)

Controller : 웹 MVC 컨트롤러 

Service : 핵심 비즈니스 로직 구현 

Respotiroy : 데이터베이스 접근, 도메인 객체를 DB에 저장하고 관리 

Domain : 비즈니스 도메인 객체

클래스 의존관계

![img_10](https://user-images.githubusercontent.com/79847020/156157559-e554807f-935b-4cb0-a716-b8e8cc3500e9.png)

아직 데이터가 저장소가 선정되지 않아 우선 인터페이스로 구현 클래스를 변경할 수 있도록 설계  

데이터 저장소는 RDB, NoSQL 등 다양한 저장소를 고민중인 상황으로 가정  

개발을 진행하기 위해서 초기 개발 단계에서는 구현체로 가까운 메모리 기반의 데이터 저장소 사용  

## 3.1 회원 도메인과 리포지토리 만들기

Member.java
```JAVA
package hello.hellospring.domain;

public class Member {
    private Long id;
    private String name;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

MemberRepository.java
```java
public interface MemberRepository {
    Member save(Member member);

    Optional<Member> findById(Long id);

    Optional<Member> findByName(String name);

    List<Member> findAll();
}
```

MemoryMemberRepository.java
```java
public class MemoryMemberRepository implements MemberRepository {
    /**
     * 동시어 문제가 고려되어 있지 않음, 실무에서 ConcurrentHashMap, AtomicLong 사용 고려
     */
    private static Map<Long, Member> store = new HashMap<>();
    private static long sequence = 0L;

    @Override
    public Member save(Member member) {
        member.setId(++sequence);
        store.put(member.getId(), member);
        return member;
    }

    @Override
    public Optional<Member> findById(Long id) {
        return Optional.ofNullable(store.get(id));
    }

    @Override
    public Optional<Member> findByName(String name) {
        return store.values().stream().filter(member -> member.getName().equals(name))
                .findAny();
    }

    @Override
    public List<Member> findAll() {
        return new ArrayList<>(store.values());
    }
}
```

## 3.2 회원 리포지토리 테트슽 케이스 작성

자바는 JUnit이라는 테스트 프레임워크를 제공하고 있다.

테스트는 각각 독립적으로 실행되어야 한다. 테스트 순서에 의존관계가 있는 것은 좋은 테스트가 아니다.

@BeforeEach : 각 @Test 테스트 메소드가 시작 될 때마다 이 기능을 실행한다.
@AfterEach : 각 @Test 테스트 메소드가 종료 될 때마다 이 기능을 실행한다.

참고로 스프링부트 테스트는 기본적으로 테스트 순서는 보장하지 않는다. 좋은 테스트는 테스트 메소드 순서와 상관없이 독립적으로 테스트할 수 있어야 한다.

```java
class MemoryMemberRepositoryTest {
    MemoryMemberRepository repository = new MemoryMemberRepository();

    @AfterEach
    public void afterEach() {
        repository.clearStore();
    }

    @Test
    public void save() {
        //given
        Member member = new Member();
        member.setName("spring");
        //when
        repository.save(member);
        //then
        Member result = repository.findById(member.getId()).get();
        assertThat(result).isEqualTo(member);
    }

    @Test
    public void findByName() {
        //given
        Member member1 = new Member();
        member1.setName("spring1");
        repository.save(member1);
        Member member2 = new Member();
        member2.setName("spring2");
        repository.save(member2);
        //when
        Member result = repository.findByName("spring1").get();
        //then
        assertThat(result).isEqualTo(member1);
    }

    @Test
    public void findAll() {
        //given
        Member member1 = new Member();
        member1.setName("spring1");
        repository.save(member1);
        Member member2 = new Member();
        member2.setName("spring2");
        repository.save(member2);
        //when
        List<Member> result = repository.findAll();
        //then
        assertThat(result.size()).isEqualTo(2);
    }
}
```
## 3.3 회원 서비스 개발

서비스 명명 규칙은 비즈니스에 맞게 리포지토리 명명 규칙은 쿼리 목적에 맞게 명명하자.

Intellij 단축키

Ctrl + Alt + V : Introduct Variable
Ctrl + Alt + M : Extract Method

MemberService.java
```java
public class MemberService {
    private final MemberRepository memberRepository = new MemoryMemberRepository();

    /**
     * 회원 가입
     */
    public Long join(Member member) {
        validateDuplicateMember(member);
        memberRepository.save(member);
        return member.getId();
    }

    private void validateDuplicateMember(Member member) {
        memberRepository.findByName(member.getName()).ifPresent(m -> {
            throw new IllegalStateException("이미 존재하는 회원입니다");
        });
    }

    /**
     * 회원 전체 조회
     */
    public List<Member> findMembers() {
        return memberRepository.findAll();
    }

    public Optional<Member> findOne(Long memberId) {
        return memberRepository.findById(memberId);
    }
}
```
기존에는 회원 서비스가 메모리 회원 리포지토리를 직접 생성하게 했다. DI가 가능하게 변경한다.

MemberService.java
```java
public class MemberService {
    private final MemberRepository memberRepository;

    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
    ...
}
```

MemberServiceTest.java
```java
class MemberServiceTest {
    MemoryMemberRepository memberRepository;
    MemberService memberService;

    @BeforeEach
    public void beforeEach() {
        memberRepository = new MemoryMemberRepository();
        memberService = new MemberService(memberRepository);
    }

    @AfterEach
    public void afterEach() {
        memberRepository.clearStore();
    }

    @Test
    public void 회원가입() throws Exception {
        //given
        Member member = new Member();
        member.setName("spring");

        //when
        Long saveId = memberService.join(member);

        //then
        Member findMember = memberRepository.findById(saveId).get();
        assertEquals(member.getName(), findMember.getName());
    }

    @Test
    public void 중복_회원_예외() throws Exception {
        //given
        Member member1 = new Member();
        member1.setName("spring");

        //when
        Member member2 = new Member();
        member2.setName("spring");

        //then
        memberService.join(member1);
        IllegalStateException e = assertThrows(IllegalStateException.class, () -> memberService.join(member2));

        assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");
    }
}
```

회원 가입 테스트 형태, 예외 처리 형태 한번 봐두자. 

# 4. 스프링 빈과 의존관계

스프링 빈을 등록하는 2가지 방법
* 컴포넌트 스캔과 자동 의존관계 설정
* 자바 코드로 직접 스프링 빈 등록하기

## 4.1 컴포넌트 스캔과 자동 의존관계 설정

* 컴포넌트 스캔 원리
  * @Component 애노테이션이 있으면 스프링 빈으로 자동 등록된다.
  * @Controller 컨트롤러가 스프링 빈으로 등록되는 이유도 컴포넌트 스캔 덕분이다.
  * 컴포넌트 스캔의 범위는 @SpringBootApplication 애노테이션이 적용되어있는 Java파일부터 하위패키지로 스캔된다.       
  * 이는 @SpringBootApplication 정의에 @ComponentScan이 적용되어 있기 때문이다.
  * @Component를 포함하는 @Controller, @Service, @Repository도 컴포넌트 스캔에 의해 자동으로 스프링 빈으로 등록된다. 마찬가지로 해당 애노테이션 정의에 @Component이 적용되어있기 때문이다.

* @Autowired
  * 생성자에 @Autowired를 사용하면 객체 생성시점에 스프링 컨테이너에서 해당 스프링 빈을 찾아서 주입한다. 생성자가 1개만 있으면 @Autowired를 생략해도 자동 적용된다.
  * 이렇게 의존관계를 외부에서 넣어주는 것을 DI(Dependency Injection) 의존성 주입이라고 한다.
  * 이전 테스트 코드에서는 개발자가 직접 주입했고 여기서는 @Autowired를 사용해 스프링에 의해 자동 주입되었다.

MemberController
```java
@Controller
public class MemberController {

    private final MemberService memberService;

    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
    }
}
```

애플리케이션 가동
```text
Action:

Consider defining a bean of type 'hello.hellospring.service.MemberService' in your configuration.
```

MemberController는 @Controller에 의해 스프링 빈으로 자동등록 되고 생성자가 호출된다. 생성자에 적용된 @Autowired에 의해 MemberService가 자동주입되는데 현재 MemberService는 스프링 빈으로 등록되어있지 않다.

MemberService와 MemberRepository에 @Service와 @Repository를 적용하자.

참고로 스프링은 스프링 컨테이너에 스프링 빈을 등록할 때 기본으로 싱글톤으로 등록한다. 따라서 같은 스프링 빈이면 모두 같은 인스턴스이다. 

MemberService, MemberRepository
```java
@Service
public class MemberService {
    ...
}
```
```java
@Repository
public class MemberRepository {
    ...
}
```
## 4.2 자바 코드로 직접 스프링 빈 등록하기

다음과 같이 코드로 빈을 등록할 수도 있다.

```java
@Configuration
public class SpringConfig {
    @Bean
    public MemberService memberService() {
        return new MemberService(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }
}
```

참고
1. XML로 설정하는 방식도 있지만 최근에는 잘 사용하지 않는다.
2. DI에는 필드 주입, Setter 주입, 생성자 주입 3가지 방식이 있다. 의존관계가 실행중에 동적으로 변하는 경우는 거의 없으므로 생성자 주입을 권장한다.
3. 실무에서는 정형화된 컨트롤러, 서비스, 리포지토리 같은 코드는 컴포넌트 스캔을 활용하고 그 외 정형화 되지 않거나, 상황에 따라 구현 클래스를 변경해야 하면 설정을 통해 스프링 빈으로 등록한다.
4. @Autowired를 통한 DI는 스프링이 관리하는 객체에만 동작한다. 스프링 빈으로 등록하지 않고 내가 직접 생성한 객체에서는 동작하지 않는다.
5. memberRepository()에서 MemoryRepository의 구현체를 바꿔끼울 수 있다. 현재는 MemoryMemberRepository 인스턴스가 MemberRepository의 빈으로 등록된다.

# 5. 회원 관리 예제 - 웹 MVC 개발

## 5.1 홈 화면
HomeController.java
```java
@Controller
public class HomeController {
    @GetMapping("/")
    public String home() {
        return "home";
    }
}
```

home.html
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
</head>
<body>
<div class="container">
    <div>
        <h1>Hello Spring</h1>
        <p>회원 기능</p>
        <p>
            <a href="/members/new">회원 가입</a>
            <a href="/members">회원 목록</a>
        </p>
    </div>
</div>
</body>
</html>
```

## 5.2 회원 웹 기능 - 등록

MemberController.java
```JAVA
@Controller
public class MemberController {
    ...
    @GetMapping("/members/new")
    public String createForm() {
        return "members/createMemberForm";
    }

    @PostMapping("/members/new")
    public String create(MemberForm memberForm) {
        Member member = new Member();
        member.setName(memberForm.getName());
        memberService.join(member);
        return "redirect:/";
    }    
...
```
member/createMemberForm.html
```html
<!DOCTYPE html>
<html>
<body>
<div class="container">
    <form action="/members/new" method="post">
        <div class="form-group">
            <lable for="name">이름</lable>
            <input type="text" id="name" name="name" placeholder="이름을 입력하세요.">
        </div>
        <button type="submit">등록</button>
    </form>
</div>
</body>
</html>
```
MemberForm.java
```html
public class MemberForm {
private String name;

public String getName() {
return name;
}

public void setName(String name) {
this.name = name;
}
}
```

## 5.3 회원 웹 기능 - 조회

MemberController.java
```java
@Controller
public class MemberController {
    ...
    @GetMapping("/members")
    public String list(Model model) {
        List<Member> members = memberService.findMembers();
        model.addAttribute("members", members);
        return "members/memberList";
    }
    ...
```

memberList.html
```html
<!DOCTYPE HTML>
<html>
<body>
<div class="container">
    <div>
        <table>
            <thead>
            <tr>
                <th>#</th>
                <th>이름</th>
            </tr>
            </thead>
            <tbody>
            <tr th:each="member : ${members}">
                <td th:text="${member.id}"></td>
                <td th:text="${member.name}"></td>
            </tr>
            </tbody>
        </table>
    </div>
</div>
</body>
</html>
```

# 5. 스프링 DB 접근 기술

## 5.1 환경 설정

* JDBC : 자바에서 DB를 사용하는 라이브러리
* com.h2database:h2 : H2 DB 접속 클라이언트 라이브러리

build.gradle
```
runtimeOnly 'com.h2database:h2'
implementation 'org.springframework.boot:spring-boot-starter-jdbc'
```

### H2 데이터 베이스 서버 설치 시

* H2 데이터베이스 서버 버전은 스프링 부트 버전에 맞춘다.
* 권한 설정 : chmod 755 h2.sh (윈도우 필요 없음)
* 실행 : ./h2.sh (윈도우 : h2.bat)
* 재설치시 : ~/test.mv.db 파일 삭제 (윈도우 C:\Users\[user}\test.mv.db 파일 삭제)
* 데이터베이스 파일 생성 방법 
  * jdbc:h2:~/test (최초 한번)
  * 최초 접속시 C:\Users\[user}\test.mv.db 생성 확인
  * 이후 jdbc:h2:tcp://localhost/~/test

application.properties
```
spring.datasource.url=jdbc:h2:tcp://localhost/~/test
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.username=sa
```

h2-console

http://localhost:8082/h2-console

### inmemory H2 사용시

application.properties
```
spring.datasource.url=jdbc:h2:mem:test
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.username=sa
```

h2-console

http://localhost:8080/h2-console 

## 5.2 테이블 생성하기

sql/ddl.sql 작성
```sql
drop table if exists member CASCADE;

create table member
(
    id   bigint generated by default as identity,
    name varchar(255),
    primary key (id)
);
```

## 5.3 순수 Jdbc 리포지토리 구현

JDBC API로 직접 코딩하는 것은 20년전 이야기다. 살펴보기만 하자.

JdbcMemberRepository
```java
public class MemoryMemberRepository implements MemberRepository {
    /**
     * 동시어 문제가 고려되어 있지 않음, 실무에서 ConcurrentHashMap, AtomicLong 사용 고려
     */
    private static Map<Long, Member> store = new HashMap<>();
    private static long sequence = 0L;

    @Override
    public Member save(Member member) {
        member.setId(++sequence);
        store.put(member.getId(), member);
        return member;
    }

    @Override
    public Optional<Member> findById(Long id) {
        return Optional.ofNullable(store.get(id));
    }

    @Override
    public Optional<Member> findByName(String name) {
        return store.values().stream().filter(member -> member.getName().equals(name))
                .findAny();
    }

    @Override
    public List<Member> findAll() {
        return new ArrayList<>(store.values());
    }

    public void clearStore() {
        store.clear();
    }
}
```
Code
```java
public class JdbcMemberRepository implements MemberRepository {
    // application.properties에서 설정한 DataSource 주입
    private final DataSource dataSource;

    public JdbcMemberRepository(DataSource dataSource) {
        this.dataSource = dataSource;
    }
...
```
DataSource는 데이터베이스 커넥션을 획득할 때 사용하는 객체다. 스프링 부트는 데이터베이스 커넥션 정보를 바탕으로 DataSource를 스프링 빈으로 생성한다. 그래서 DI 받을 수 있다.

```java
    private Connection getConnection() {
        return DataSourceUtils.getConnection(dataSource);
    }
```

스프링에서 JDBC를 사용할 때 DataSourceUtils를 통해서 Connection을 얻어야 한다. Transaction이 정상동작하기 위해서는 같은 하나의 Connection을 유지해야 한다. 

```java
    private void close(Connection conn) {
        DataSourceUtils.releaseConnection(conn, dataSource);
    }
```

사용 후 Connection을 반납한다.

## 5.4 SpringConfig 수정

SpringConfig.java
```java
@Configuration
public class SpringConfig {

    private final DataSource dataSource;

    public SpringConfig(DataSource dataSource) {
        this.dataSource = dataSource;
    }

    @Bean
    public MemberService memberService() {
        return new MemberService(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository() {
//        return new MemoryMemberRepository();
        return new JdbcMemberRepository(dataSource);
    }
}
```

```java
    public SpringConfig(DataSource dataSource) {
        this.dataSource = dataSource;
    }
```
@Autowired가 적용되어있지 않지만 SpringConfig은 @Configuration에 의해 스프링 빈으로 등록된다. 그리고생성자가 하나면 알아서 생성자 주입한다.

```java
    @Bean
    public MemberRepository memberRepository() {
//        return new MemoryMemberRepository();
        return new JdbcMemberRepository(dataSource);
    }
```
MemoryMemberRepository에서 JdbcMemberRepository로 MemberRepository빈을 교체한다.


* 개방 폐쇠 원칙(OCP:Open-Closed Principle) 
  확장에는 열려있꼬 수정 변경에는 닫혀있다. 
* 의존성 주입(DI:Dependency Injection) 
  기존 코드를 전혀 손대지 않고 설정만으로 구현 클래스를 변경할 수 있다.

## 5.5 Test 수정

```java
@SpringBootTest
@Transactional
class MemberServiceIntegrationTest {
    @Autowired
    MemberService memberService;

    @Autowired
    MemberRepository memberRepository;

    @Test
    public void 회원가입() throws Exception {
        //given
        Member member = new Member();
        member.setName("spring");

        //when
        Long saveId = memberService.join(member);

        //then
        Member findMember = memberRepository.findById(saveId).get();
        assertEquals(member.getName(), findMember.getName());
    }

    @Test
    public void 중복_회원_예외() throws Exception {
        //given
        Member member1 = new Member();
        member1.setName("spring");

        //when
        Member member2 = new Member();
        member2.setName("spring");

        //then
        memberService.join(member1);
        IllegalStateException e = assertThrows(IllegalStateException.class, () -> memberService.join(member2));

        assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");
    }
}
```

@SpringBootTest : 스프링 컨테이너와 테스트를 함께 실행한다.

@Transactional : 테스트 케이스에 이 애노테이션이 있으면 테스트 시작 전에 트랜잭션을 시작하고 테스트 완료 후에 항상 롤백한다. DB에 데이터가 남지 않으므로 다음 테스트에 영향을 주지 않는다.

## 5.6 스프링 JdbcTemplate 구현

순수 JDBC와 동일한 환경설정을 하면된다. 스프링 JdbcTemplate과 MyBatis 같은 라이브러리는 JDBC API에서 본 반복코드를 대부분 제거해준다. 하지만 SQL은 직접 작성해야한다.

JdbcTemplateMemberRepository.java
```java
public class JdbcTemplateMemberRepository implements MemberRepository {
    private final JdbcTemplate jdbcTemplate;

    public JdbcTemplateMemberRepository(DataSource dataSource) {
        this.jdbcTemplate = new JdbcTemplate(dataSource);
    }

    @Override
    public Member save(Member member) {
        SimpleJdbcInsert jdbcInsert = new SimpleJdbcInsert(jdbcTemplate);
        jdbcInsert.withTableName("member").usingGeneratedKeyColumns("id");

        Map<String, Object> parameters = new HashMap<>();
        parameters.put("name", member.getName());
        Number key = jdbcInsert.executeAndReturnKey(new MapSqlParameterSource(parameters));
        member.setId(key.longValue());
        return member;
    }

    @Override
    public Optional<Member> findById(Long id) {
        List<Member> result = jdbcTemplate.query("select * from member where id = ?", memberRowMapper(), id);
        return result.stream().findAny();
    }

    @Override
    public Optional<Member> findByName(String name) {
        List<Member> result = jdbcTemplate.query("select * from member where name = ?", memberRowMapper(), name);
        return result.stream().findAny();
    }

    @Override
    public List<Member> findAll() {
        return jdbcTemplate.query("select * from member", memberRowMapper());
    }

    private RowMapper<Member> memberRowMapper() {
        return (rs, rowNum) -> {
            Member member = new Member();
            member.setId(rs.getLong("id"));
            member.setName(rs.getString("name"));
            return member;
        };
    }
}
```

```java
return result.stream().findAny();
```

List -> Optional로 바꿀 때 사용

SpringConfig.java
```java
...
    @Bean
    public MemberRepository memberRepository() {
//        return new MemoryMemberRepository();
//        return new JdbcMemberRepository(dataSource);
        return new JdbcTemplateMemberRepository(dataSource);
    }
...
```

MemberServiceIntegrationTest 정상 완료

## 5.7 JPA

JPA는 기존의 반복 코드, 기본 SQL을 직접 만들어서 실행해준다. JPA를 사용하면 SQL과 데이터 중심의 설계에서 객체 중심의 설계로 패러다임을 전환할 수 있다. 개발 생산성을 크게 높일 수 있다.

JPA는 인터페이스이다. JPA의 구현체 중 하나는 Hibernate이고 Spring이 기본으로 사용하는 JPA이다. JPA는 ORM(Object Relational Mapping) 객체와 관계형 테이블을 매핑하는 기술중 하나이다.

build.gradle 파일에 JPA, H@ 데이터베이스 관련 라이브러리 추가 

build.gradle
```
...
//    implementation 'org.springframework.boot:spring-boot-starter-jdbc'
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    runtimeOnly 'com.h2database:h2'
...
```

application.properties
```java
spring.datasource.url=jdbc:h2:tcp://localhost/~/test
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.username=SA

spring.jpa.show-sql=true // JPA SQL 보기
spring.jpa.hibernate.ddl-auto=none //JPA 자동 테이블 생성
```

spring.jpa.show-sql=true : JPA가 생성하는 SQL을 출력한다. 

spring.jpa.hibernate.ddl-auto=none : JPA는 테이블을 자동으로 생성하는 기능을 제공하는데 none를 사용하면 해당 기능을 끈다. create를 사용하면 엔티티정보를 바탕으로 테이블도 직접 생성해준다. 

JPA 엔티티 매핑

Member.java
```java
package hello.hellospring.domain;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
public class Member {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

JpaMemberRepository.java
```java 
public class JpaMemberRepository implements MemberRepository {

    private final EntityManager em;

    public JpaMemberRepository(EntityManager em) {
        this.em = em;
    }

    public Member save(Member member) {
        em.persist(member);
        return member;
    }

    public Optional<Member> findById(Long id) {
        Member member = em.find(Member.class, id);
        return Optional.ofNullable(member);
    }

    public Optional<Member> findByName(String name) {
        List<Member> result = em.createQuery("select m from Member m where m.name = :name", Member.class)
                .setParameter("name", name)
                .getResultList();
        return result.stream().findAny();
    }

    public List<Member> findAll() {
        return em.createQuery("select m from Member m", Member.class).getResultList();
    }
}
```

```java
    private final EntityManager em;

    public JpaMemberRepository(EntityManager em) {
        this.em = em;
    }
```
JPA는 EntityManager에 의해서 동작한다. JPA라이브러리를 추가하면 스프링부트가 자동으로 EntityManager을 생성하고 현재 데이터베이스랑 연결한다. 생성 된 EntityManaager를 JpaMemberRepository에 DI해서 사용한다.

```java
        List<Member> result = em.createQuery("select m from Member m where m.name = :name", Member.class)
```
JPQL이란 객체지향쿼리언어를 사용한다. 테이블이 아닌 객체를 대상으로 사용할 수 있는 쿼리이다.

JPA기술을 스프링이 추상화해서 제공하는 기술이 있다. 그것을 스프링 JPA라고 한다.

MemberService.java
```java
@Transactional
public class MemberService {
```

MemberService에 @Transactional을 적용한다. JPA를 통한 모든 데이터 변경은 트랜잭션 안에서 실행해야 한다. 

SpringConfig.java
```java
@Configuration
public class SpringConfig {
    private final DataSource dataSource;
    private final EntityManager em;

    public SpringConfig(DataSource dataSource, EntityManager em) {
        this.dataSource = dataSource;
        this.em = em;
    }
...
    @Bean
    public MemberRepository memberRepository() {
//        return new MemoryMemberRepository();
//        return new JdbcMemberRepository(dataSource);
//        return new JdbcTemplateMemberRepository(dataSource);
        return new JpaMemberRepository(em);
    }
}
```
EntityManager을 주입하고 JpaMemberRepository를 Repository로 사용한다.

정상적으로 테스트가 동작하는 것을 확인할 수 있다.

IntelliJ 단축키 

Ctrl + Alt + N : Inline Variable
Shift + Ctrl + Alt + N : Refactor


## 5.8 Spring JPA

스프링 데이터 JPA를 사용하면 기존의 한계를 넘어 리포지토리에 구현 클래스 없이 인터페이스만으로 개발을 완료할 수 있습니다. 반복 개발해온 기본 CRUD 기능도 스프링 데이터 JPA가 모두 제공합니다. 스프링 부트와 JPA라는 기반위에 스프링 데이터 JPA라는 프레임를 더하면 개발 코드들이 확연하게 줄어듭니다.

SpringDataJpaMemberRepository.java
```java
public interface SpringDataJpaMemberRepository extends JpaRepository<Member, Long>, MemberRepository {
    Optional<Member> findByName(String name);
}
```

SpringConfig.java
```java
@Configuration
public class SpringConfig {
    private final MemberRepository memberRepository;

    public SpringConfig(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    @Bean
    public MemberService memberService() {
        return new MemberService(memberRepository);
    }
}
```

테스트 역시 정상 동작합니다.

![img_11](https://user-images.githubusercontent.com/79847020/156157587-811c7943-ac3c-4469-89aa-76ba1fe1c7e8.png)

* 스프링 데이터 JPA 제공 기능
  * 인터페이스를 통한 기본 CRUD 
  * findByName(), findByEmail() 메소드 이름만으로 조회 기능 제공
  * 페이지 기능 자동 제공

실무에서는 JPA와 스프링 데이터 JPA를 기본으로 사용하고 복잡한 동적 쿼리는 Querydsl이라는 라이브러리를 사용하면 된다. Querydsl을 사용하면 쿼리도 자바 코드로 안전하게 작성할 수 있꼬 동적쿼리도 편리하게 작성할 수 있습니다. 이 조합으로 해결하기 어려운 쿼리는 JPA가 제공하는 네이티브 쿼리를 사용하거나 JdbcTemplate을 사용하면 된다.

# 6. AOP

## 6.1 AOP가 필요한 상황

AOP가 필요한 상황
1. 모든 메소드의 호출 시간을 측정하고 싶다면
2. 공통 관심 사항(Cross-Cutting Concern) vs 핵심 관심 사항(Core Concern)
3. 회원 가입 시간, 회원 조회 시간을 측정하고 싶다면?

![img_12](https://user-images.githubusercontent.com/79847020/156157606-cb258c47-0773-4bbd-b3fb-fdbd84b01341.png)

MemberService.java
```java
@Transactional
public class MemberService {
    ....

    /**
     * 회원 가입
     */
    public Long join(Member member) {
        long start = System.currentTimeMillis();

        try {
            validateDuplicateMember(member);
            memberRepository.save(member);
            return member.getId();
        } finally {
            long finish = System.currentTimeMillis();
            long timeMs = finish - start;
            System.out.println("join " + timeMs + "ms");
        }
    }

    /**
     * 회원 전체 조회
     */
    public List<Member> findMembers() {
        long start = System.currentTimeMillis();

        try {
            return memberRepository.findAll();
        } finally {
            long finish = System.currentTimeMillis();
            long timeMs = finish - start;
            System.out.println("join " + timeMs + "ms");
        }
    }
    ...
}
```

* 문제
  * 회원가입, 회원조회에 시간을 측정하는 기능은 핵심 관심사항이 아니다.
  * 시간을 측정하는 로직은 공통 관심사항이다.
  * 시간을 측정하는 로직과 핵심 비즈니스의 로직이 섞여서 유지보수가 어렵다.
  * 시간을 측정하는 로직을 별도의 공통로직으로 만들기 매우 어렵다.
  * 시간을 측정하는 로직을 변경 할 때 모든 로직을 찾아가면서 변경 해야한다.

## 6.2 AOP 적용

* AOP(Aspect Oriented Programming) : 관점 지향 프로그래밍
* 공통 관심 사항(Cross-Cutting Concern) vs 핵심 관심 사항(Core Concern)
* 횡단 관심사를 분리
* 비즈니스 로직에 집중 할 수 있다.

![img_13](https://user-images.githubusercontent.com/79847020/156157620-73a01060-7e14-401a-b5bd-453116bb05d2.png)

TimeTraceAop.java
```java
@Component
@Aspect
public class TimeTraceAop {

    @Around("execution(* hello.hellospring..*(..))")
    public Object execute(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();

        System.out.println("START: " + joinPoint.toString());

        try {
            return joinPoint.proceed();
        } finally {
            long finish = System.currentTimeMillis(); long timeMs = finish - start;
            System.out.println("END: " + joinPoint.toString()+ " " + timeMs + "ms");
        }
    }
}
 ```

* 해결
  * 회원가입, 회원조회 등 핵심 관심사항과 시간을 측정하는 공통 관심사항을 분리한다.
  * 시간을 측정하는 로직을 별도의 공통로직으로 만들었다.
  * 핵심 관심사항을 깔끔하게 유지 할 수 있다.
  * 변경이 필요하면 이 로직만 변경하면된다.
  * 원하는 적용 대상을 선택 할 수 있다.

## 6.3 스프링의 AOP 동작 방식  

### AOP 적용 전 의존관계

![img_14](https://user-images.githubusercontent.com/79847020/156157631-3c333665-673d-4abf-b9a9-9f07a70f2d4c.png)

### AOP 적용 후 의존관계

![img_15](https://user-images.githubusercontent.com/79847020/156157643-4b5c0e57-07c7-433c-80e2-be846764f09e.png)

### AOP 적용 전 전체 그림

![img_16](https://user-images.githubusercontent.com/79847020/156157652-a4e0366c-d29b-498f-bed9-38498f95cf04.png)

### AOP 적용 후 전체 그림

![img_17](https://user-images.githubusercontent.com/79847020/156157660-bd458f83-67d1-4d78-b0f8-589cbb04827d.png)

```text
START: execution(Long hello.hellospring.service.MemberService.join(Member))
START: execution(Optional hello.hellospring.repository.SpringDataJpaMemberRepository.findByName(String))
Hibernate: select member0_.id as id1_0_, member0_.name as name2_0_ from member member0_ where member0_.name=?
END: execution(Optional hello.hellospring.repository.SpringDataJpaMemberRepository.findByName(String)) 136ms
START: execution(Member hello.hellospring.repository.MemberRepository.save(Member))
Hibernate: insert into member (id, name) values (null, ?)
END: execution(Member hello.hellospring.repository.MemberRepository.save(Member)) 28ms
join 166ms
END: execution(Long hello.hellospring.service.MemberService.join(Member)) 174ms
START: execution(Long hello.hellospring.service.MemberService.join(Member))
START: execution(Optional hello.hellospring.repository.SpringDataJpaMemberRepository.findByName(String))
Hibernate: select member0_.id as id1_0_, member0_.name as name2_0_ from member member0_ where member0_.name=?
END: execution(Optional hello.hellospring.repository.SpringDataJpaMemberRepository.findByName(String)) 7ms
join 7ms
END: execution(Long hello.hellospring.service.MemberService.join(Member)) 8ms
```
