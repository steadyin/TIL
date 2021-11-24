# 2장 테스트

* 변화에 대응하는 전략 2가지
    
      1. 확장과 변화를 고려한 객체지향적 설계와 그것을 효과적으로 담아낼 수 있는 IoC/DI 같은 기술
      2. 만들어진 코드를 확신할 수 있게 해주고 변화에 유연하게 대처할 수 있는 자신감을 주는 테스트 기술

## 2.1 UserDaoTest 다시보기

### 2.1.1 테스트의 유용성

* 테스트란 ? 

      예상하고 의도했던 대로 코드가 정확히 동작하는지를 확인해서 만든 코드를 확신할 수 있게 해주는 작업

### 2.1.2 UserDaoTest 특징

#### 웹을 통한 DAO 테스트 방법의 문제점

사실 테스트하고 싶었던 건 UserDao였는데 다른 계층의 코드와 컴포넌트 심지어 서버의 설정 상태까지 
모두 테스트에 영향을 줄 수 있기 때문에 이런 방식으로 테스트하는 것은 번거롭고 오류가 있을 떄 빠르고 정확하게 대응하기 힘들다는 문제가 있다.

#### 작은 단위의 테스트

테스트는 가능하면 작은 단위로 쪼개서 집중해서 할 수 있어야 한다. 이는 관심사의 분리라는 프로그래밍 관점에서도 일치한다.

* 단위 테스트(Unit Test) 

#### 자동 수행 테스트 코드

테스트는 자동으로 수행되도록 코드로 만들어지는 것이 중요하다.

#### 지속적인 개선과 점직적인 개발을 위한 테스트

### 2.1.3 UserDaoTest의 문제점

1. 수동 확인 작업의 번거로움
2. 실행 작업의 번거로움

## 2.2 UserDaoTest 개선

### 2.2.1 테스트 검증의 자동화

### 2.2.2 테스트의 효율적인 수행과 결과 관리

* 테스팅 프레임워크

      일정한 패턴을 가진 테스트를 만들 수 있고 많은 테스트를 간단히 실행시킬 수 있으며 테스트 결과를 종합해서 볼 수 있고 테스트가 실패한 곳을 빠르게 찾을 수 있는 기능을 갖춘 테스트 지원 도구
      자바에서는 대표적으로 JUnit

#### JUnit 테스트로 전환

#### 테스트 메소드 전환

JUnit 프레임워크가 요구하는 조건 두가지를 따라야 한다. 

첫번째는 메소드가 public으로 선언돼야 하는 것이고 다른 하나는 메소드에 @Test라는 애노테이션을 붙여주는 것이다.
      
JUnit은 전통적으로 public 메소드만을 테스트 메소드로 허용하고 있다. 마지막으로 @Test 애노테이션을 붙여주면 된다.

#### 검증 코드 전환

* assertThat()

      assertThat() 메소드는 첫번째 파라미터의 값을 뒤에 나오는 매처(matcher)라고 불리는 조건으로 비교해서 일치하면 다음으로 넘어가고 아니면 테스트가 실패하도록 만들어준다.

* is()

      매처의 일종으로 equals()로 비교해주는 기능을 가졌다.

JUnit을 적용한 USerDaoTest
```JAVA
package springbook.project.user.dao;

import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;
import springbook.project.user.domain.User;

import java.sql.SQLException;

import static org.hamcrest.MatcherAssert.assertThat;
import static org.hamcrest.Matchers.is;

public class UserDaoTest {
    @Test
    public void addAndGet() throws SQLException {
        ApplicationContext applicationContext = new GenericXmlApplicationContext("applicationContext.xml");
        UserDao dao = (UserDao) applicationContext.getBean("userDao");

        User user = new User();
        user.setId("whiteship");
        user.setName("백기선");
        user.setPassword("married");

        dao.delete();

        dao.add(user);

        User user2 = dao.get(user.getId());

        assertThat(user2.getName(), is(user.getName()));
    }
}
```

#### JUnit 테스트 실행

스프링 컨테이너와 마찬가지로 JUnit 프레임워크도 자바 코드로 만들어진 프로그램 이므로 어디선가 한 번은 JUnit 프레임워크를 시작해주어야 한다.

JUnit을 이용해 테스트를 실행해주는 main() 메소드
```JAVA
package springbook.project;

import org.junit.runner.JUnitCore;
import org.springframework.boot.autoconfigure.SpringBootApplication;

import java.sql.SQLException;

@SpringBootApplication
public class ProjectApplication {

    public static void main(String[] args) throws SQLException, ClassNotFoundException {
        JUnitCore.main("springbook.project.user.dao.UserDaoTest");
    }

}
```

## 2.3 개발자를 위한 테스팅 프레임워크 JUnit

### 2.3.1 JUnit 테스트 실행 방법

JUit은 사실상 자바의 표준 테스팅 프레임워크

#### IDE

#### 빌드 툴

### 2.3.2 테스트 결과의 일관성

이전 테스트의 문제점은 외부 상태(DB)에 따라 성공하기도 하고 실패하기도 한다는 점이다.

#### deleteAll(), getCount() 추가

deleteAll(), getCount() 메소드
```JAVA
package springbook.project.user.dao;

import springbook.project.user.domain.User;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class UserDao {
    private ConnectionMaker connectionMaker;

    public void setConnectionMaker(ConnectionMaker connectionMaker) {
        this.connectionMaker = connectionMaker;
    }

    private DataSource dataSource;

    public void setDataSource(DataSource dataSource) {
        this.dataSource = dataSource;
    }

    public void add(User user) throws  SQLException {
        Connection c = dataSource.getConnection();

        PreparedStatement ps = c.prepareStatement("insert into users(id,name,password) values(?,?,?)");
        ps.setString(1, user.getId());
        ps.setString(2, user.getName());
        ps.setString(3, user.getPassword());

        ps.executeUpdate();
        c.close();
    }

    public User get(String id) throws  SQLException {
        Connection c = dataSource.getConnection();

        PreparedStatement ps = c.prepareStatement("select * from users where id = ?");
        ps.setString(1, id);

        ResultSet rs = ps.executeQuery();
        rs.next();
        User user = new User();
        user.setId(rs.getString("id"));
        user.setName(rs.getString("name"));
        user.setPassword(rs.getString("password"));

        rs.close();
        ps.close();
        c.close();
        return user;
    }

    public void deleteAll() throws SQLException {
        Connection c = dataSource.getConnection();

        PreparedStatement ps = c.prepareStatement("DELETE FROM users");
        ps.executeUpdate();

        ps.close();
        c.close();
    }
    
    public int getCount() throws  SQLException {
        Connection c = dataSource.getConnection();

        PreparedStatement ps = c.prepareStatement("select count(*) from users");

        ResultSet rs = ps.executeQuery();
        rs.next();
        int count = rs.getInt(1);

        rs.close();
        ps.close();
        c.close();

        return count;
    }    
}
```

### deleteAll()과 getCount()의 테스트

deleteAll() getCount()가 추가된 addAndGet() 테스트
```JAVA
package springbook.project.user.dao;

import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;
import springbook.project.user.domain.User;

import java.sql.SQLException;

import static org.hamcrest.MatcherAssert.assertThat;
import static org.hamcrest.Matchers.is;

public class UserDaoTest {
    @Test
    public void addAndGet() throws SQLException {
        ApplicationContext applicationContext = new GenericXmlApplicationContext("applicationContext.xml");
        UserDao dao = (UserDao) applicationContext.getBean("userDao");

        dao.deleteAll();
        assertThat(dao.getCount(), is(0));

        User user = new User();
        user.setId("whiteship");
        user.setName("백기선");
        user.setPassword("married");

        dao.add(user);
        assertThat(dao.getCount(), is(1));

        User user2 = dao.get(user.getId());

        assertThat(user2.getName(), is(user.getName()));
        assertThat(user2.getPassword(),a is(user.getPassword()));
    }
}
```

#### 동일한 결과를 보장하는 테스트

단위 테스트는 항상 일관성 있는 결과가 보장돼야 한다는 점을 잊어선 안된다. 외부 환경에 영향을 받지 말아야 하는 것은 물론이고 테스트를 실행하는 순서를 
바꿔도 동일한 결과가 보장되도록 만들어야 한다.

### 2.3.3 포괄적인 테스트

#### getCount() 테스트

테스트 메소드는 한번에 한가지 검증 목적에만 충실한 것이 좋다.

JUnit은 @Test가 붙어있고 public 접근자가 있으며 리턴값이 void형이고 파라미터가 없다는 조건을 지키기만 하면 테스트 메소드로 동작할 수 있다.

파라미터가 있는 User 클래스 생성자
```JAVA
package springbook.project.user.domain;

public class User {

    String id;
    String name;
    String password;

    public User() {}

    public User(String id, String name, String password) {
        this.id = id;
        this.name = name;
        this.password = password;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}
```


```JAVA getCount() 테스트
package springbook.project.user.dao;

import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;
import springbook.project.user.domain.User;

import java.sql.SQLException;

import static org.hamcrest.MatcherAssert.assertThat;
import static org.hamcrest.Matchers.is;

public class UserDaoTest {
    @Test
    public void addAndGet() throws SQLException {
        ApplicationContext context = new GenericXmlApplicationContext("applicationContext.xml");
        UserDao dao = (UserDao) context.getBean("userDao");

        dao.deleteAll();
        assertThat(dao.getCount(), is(0));

        User user = new User("whiteship","백기선","married");

        dao.add(user);
        assertThat(dao.getCount(), is(1));

        User user2 = dao.get(user.getId());

        assertThat(user2.getName(), is(user.getName()));
        assertThat(user2.getPassword(), is(user.getPassword()));
    }

    @Test
    public void getCount() throws SQLException {
        ApplicationContext context = new GenericXmlApplicationContext("applicationContext.xml");

        UserDao dao = (UserDao) context.getBean("userDao");
        User user1 = new User("gyumee","박성철","springno1");
        User user2 = new User("leegw700","이길원","springno2");
        User user3 = new User("bumjin","박범진","springno3");

        dao.deleteAll();
        assertThat(dao.getCount(), is(0));

        dao.add(user1);
        assertThat(dao.getCount(), is(1));

        dao.add(user2);
        assertThat(dao.getCount(), is(2));

        dao.add(user3);
        assertThat(dao.getCount(), is(3));
    }
}
```

주의해야 할 점은 JUnit은 특정한 테스트 메소드의 실행 순서를 보장해주지 않는다.
테스트의 결과가 테스트의 실행 순서에 영향을 받는다면 테스트를 잘못 만든 것이다.

#### addAndGet() 테스트 보완

get() 테스트 기능을 보완한 addAndGet() 테스트
```JAVA
package springbook.project.user.dao;

import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;
import springbook.project.user.domain.User;

import java.sql.SQLException;

import static org.hamcrest.MatcherAssert.assertThat;
import static org.hamcrest.Matchers.is;

public class UserDaoTest {
    @Test
    public void addAndGet() throws SQLException {
        ApplicationContext context = new GenericXmlApplicationContext("applicationContext.xml");

        UserDao dao = (UserDao) context.getBean("userDao");
        User user1 = new User("gyumee","박성철","springno1");
        User user2 = new User("leegw700","이길원","springno2");

        dao.deleteAll();
        assertThat(dao.getCount(), is(0));

        dao.add(user1);
        dao.add(user2);
        assertThat(dao.getCount(), is(2));

        User userget1 = dao.get(user1.getId());
        assertThat(user1.getName(), is(userget1.getName()));
        assertThat(user1.getPassword(), is(userget1.getPassword()));

        User userget2 = dao.get(user2.getId());
        assertThat(user2.getName(), is(userget2.getName()));
        assertThat(user2.getPassword(), is(userget2.getPassword()));
    }

    @Test
    public void getCount() throws SQLException {
        ApplicationContext context = new GenericXmlApplicationContext("applicationContext.xml");

        UserDao dao = (UserDao) context.getBean("userDao");
        User user1 = new User("gyumee","박성철","springno1");
        User user2 = new User("leegw700","이길원","springno2");
        User user3 = new User("bumjin","박범진","springno3");

        dao.deleteAll();
        assertThat(dao.getCount(), is(0));

        dao.add(user1);
        assertThat(dao.getCount(), is(1));

        dao.add(user2);
        assertThat(dao.getCount(), is(2));

        dao.add(user3);
        assertThat(dao.getCount(), is(3));
    }
}
```

#### get() 예외조건에 대한 테스트

일반적으로는 테스트 중에 예외가 던져지면 테스트 메소드의 실행은 중단되고 테스트는 실패한다. 
그런데 이번에는 반대로 테스트 진행 중에 특정 예외가 던져지면 테스트가 성공한 것이고 예외가 던져지지 않고 정상적으로 작업을 마치면 테스트가 실패했다고 판단해야 한다.

* @Test(expected = )

      @Test 애노테이션의 expected 엘리먼트다. expected는 테스트 메소드 실행 중에 발생하리라 기대하는 예외 클래스를 넣어주면 된다.

메소드의 예외상황에 대한 테스트
```JAVA
@Test
    @Test(expected = EmptyResultDataAccessException.class)
    public void getUserFailure() throws SQLException {
        ApplicationContext context = new GenericXmlApplicationContext("applicationContext.xml");

        UserDao dao = (UserDao) context.getBean("userDao");
        dao.deleteAll();
        assertThat(dao.getCount(), is(0));
        
        dao.get("unknow_id");
    }
```    

현재 테스트는 실패다. EmptyResultDataAccessException에 대한 예외처리를 하지 않았다.

#### Java 테스트를 성공시키기 위한 코드의 수정

데이터를 찾지 못하면 예외를 발생시키도록 수정한 get() 메소드
```JAVA
    public User get(String id) throws  SQLException {
        Connection c = dataSource.getConnection();

        PreparedStatement ps = c.prepareStatement("select * from users where id = ?");
        ps.setString(1, id);

        ResultSet rs = ps.executeQuery();

        User user = null;
        if(rs.next()) {
            user = new User();
            user.setId(rs.getString("id"));
            user.setName(rs.getString("name"));
            user.setPassword(rs.getString("password"));
        }

        rs.close();
        ps.close();
        c.close();

        if(user==null) throw new EmptyResultDataAccessException(1);
        return user;
    }
```

#### 포괄적인 테스트

스프링의 창시자인 로드 존슨은 "항상 네거티브 테스트를 먼저 만들어라"는 조언을 했다.
테스트를 작성할 때 부정적인 케이스를 먼저 만드는 습관을 들이는게 좋다.

### 테스트가 이끄는 개발

#### 기능설계를 위한 테스트

#### 테스트 주도 개발

* 테스트 주도 개발(TDD : Test Driven Development)

      테스트 코드를 먼저 만들고 테스트를 성공하게 해주는 코드를 작성하는 방식의 개발 방법이 있다. 테스트 우선 개발(Test First Development)라고도 한다.

### 2.3.5 테스트 코드 개선

JUnit 프레임워크는 테스트 메소드를 실행할 때 부가적으로 해주는 작업이 몇 가지 있다. 반복되는 준비 작업을 별도의 메소드에 넣게 해주고 이를 매번 테스트 메소드를 실행할하기 전에 먼저 실행시켜주는 기능이다.

### @Before

    JUnit이 제공하는 애노테이션. @Test 메소드가 실행되기 전에 먼저 실행돼야 하는 메소드를 정의한다.

중복 코드를 제거한 UserDaoTest
```JAVA
package springbook.project.user.dao;

import org.junit.Before;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;
import org.springframework.dao.EmptyResultDataAccessException;
import springbook.project.user.domain.User;

import java.sql.SQLException;

import static org.hamcrest.MatcherAssert.assertThat;
import static org.hamcrest.Matchers.is;

public class UserDaoTest {
    UserDao dao;

    @Before
    public void setUp() {
        ApplicationContext context = new GenericXmlApplicationContext("applicationContext.xml");
        this.dao =  context.getBean("userDao", UserDao.class);
    }

    @Test
    public void addAndGet() throws SQLException {
        User user1 = new User("gyumee", "박성철", "springno1");
        User user2 = new User("leegw700", "이길원", "springno2");

        dao.deleteAll();
        assertThat(dao.getCount(), is(0));

        dao.add(user1);
        dao.add(user2);
        assertThat(dao.getCount(), is(2));

        User userget1 = dao.get(user1.getId());
        assertThat(user1.getName(), is(userget1.getName()));
        assertThat(user1.getPassword(), is(userget1.getPassword()));

        User userget2 = dao.get(user2.getId());
        assertThat(user2.getName(), is(userget2.getName()));
        assertThat(user2.getPassword(), is(userget2.getPassword()));
    }

    @Test
    public void getCount() throws SQLException {
        User user1 = new User("gyumee", "박성철", "springno1");
        User user2 = new User("leegw700", "이길원", "springno2");
        User user3 = new User("bumjin", "박범진", "springno3");

        dao.deleteAll();
        assertThat(dao.getCount(), is(0));

        dao.add(user1);
        assertThat(dao.getCount(), is(1));

        dao.add(user2);
        assertThat(dao.getCount(), is(2));

        dao.add(user3);
        assertThat(dao.getCount(), is(3));
    }

    @Test(expected = EmptyResultDataAccessException.class)
    public void getUserFailure() throws SQLException {
        dao.deleteAll();
        assertThat(dao.getCount(), is(0));

        dao.get("unknow_id");
    }
}
```

JUnit이 하나의 테스트 클래스를 가져와 테스트를 수행하는 방식은 다음과 같다.

    1. 테스트 클래스에서 @Test가 붙은 public 이고 void형이고 파라미터가 없는 테스트 메소드를 모두 찾는다.
    2. 테스트 클래스의 오브젝트를 하나 만든다.
    3. @Before가 붙은 메소드가 있으면 실행한다.
    4. @Test가 붙은 메소드가 있으면 하나 호출하고 테스트 결과를 저장해둔다.
    5. @After가 붙은 메소드가 있으면 실행한다.
    6. 나머지 테스트 메소드에 대해 2~5번을 반복한다.
    7. 모든 테스트의 결과를 종합해서 돌려준다.

물론 @Before나 @After 메소드를 테스트 메소드에서 직접 호출하지 않기 때문에 서로 주고받을 정보나 오브젝트가 있다면 인스턴스 변수를 이용해야 한다.

꼭 한가지 기억해야 할 사항은 각 테스트 메소드를 실행할 때마다 테스트 클래스의 오브젝트를 새로 만든다는 점이다.

* 왜 테스트 메소드를 실행할 때 마다 새로운 오브젝트를 만드는 것일까?

      JUnit 개발자는 각 테스트가 서로 영향을 주지 않고 독립적으로 실행됨을 확실히 보장하기 위해 매번 새로운 오브젝트를 만들게 했다.
      덕분에 인스턴스 변수도 부담 없이 사용할 수 있다.

### 픽스처

* 픽스처

      테스트를 수행하는데 필요한 정보나 오브젝트를 픽스처라고 한다. 

픽스처를 적용한 UserDaoTest
```JAVA
~~~
public class UserDaoTest {
    private UserDao dao;
    private User user1;
    private User user2;
    private User user3;

    @Before
    public void setUp() {
        ApplicationContext context = new GenericXmlApplicationContext("applicationContext.xml");
        this.dao =  context.getBean("userDao", UserDao.class);

        this.user1 = new User("gyumee", "박성철", "springno1");
        this.user2 = new User("leegw700", "이길원", "springno2");
        this.user3 = new User("bumjin", "박범진", "springno3");
    }
~~~    
```    

## 2.4 스프링 테스트 적용

### 2.4.1 테스트를 위한 애플리케이션 컨텍스트 관리

#### 스프링 테스트 컨텍스트 프레임워크 적용

* RunWith

       JUnit 프레임워크의 테스트 실행 방법을 확장할 때 사용하는 애노테이션이다. SpringJUnit4ClassRunner라는 JUnit용 테스트 컨텍스트 프레임워크 확장 클래스를
       지정해두면 JUnit이 테스트를 진행하는 중에 테스트가 사용할 애플리케이션 컨텍스트를 만들고 관리하는 작업을 진행해준다.
       
* @ContextConfiguration

      자동으로 만들어줄 애플리케이션 컨텍스트의 설정파일 위치를 지정한 것이다.
      
스프링 테스트 컨텍스트를 적용한 UserDaoTest
```JAVA
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "/applicationContext.xml")
public class UserDaoTest {
    @Autowired
    private ApplicationContext context;

    private UserDao dao;
    private User user1;
    private User user2;
    private User user3;

    @Before
    public void setUp() {
        this.dao =  context.getBean("userDao", UserDao.class);

        this.user1 = new User("gyumee", "박성철", "springno1");
        this.user2 = new User("leegw700", "이길원", "springno2");
        this.user3 = new User("bumjin", "박범진", "springno3");
    }
```    

#### 테스트 메소드의 컨텍스트 공유

확인용 코드 추가
```JAVA
@Before
public void setUp() {
    System.out.println(this.context);
    System.out.println(this);
}
```

다음 코드를 UserDaoTest에 추가하고 결과를 확인해보자

```
org.springframework.context.support.GenericApplicationContext@52af26ee, started on Wed Oct 06 21:15:59 KST 2021
springbook.project.user.dao.UserDaoTest@fba92d3
org.springframework.context.support.GenericApplicationContext@52af26ee, started on Wed Oct 06 21:15:59 KST 2021
springbook.project.user.dao.UserDaoTest@53667cbe
org.springframework.context.support.GenericApplicationContext@52af26ee, started on Wed Oct 06 21:15:59 KST 2021
springbook.project.user.dao.UserDaoTest@464a4442
```

* UserDaoTest 객체가 서로 다른 이유는 ? ApplicationContext 객체가 같은 이유는 ?

      @Before setup() 안에서 애플리케이션 컨텍스트는 3개 모두 같은 객체이다.
      하나의 애플리케이션 컨텍스트가 만들어져 모든 테스트 메소드에서 사용되고 있음을 알고 있다.
    
      @Before setup() 안에서 UserDaoTest는 서로 다른 객체이다.
      JUnit은 테스트 메소드를 실행할 때마다 새로운 테스트 오브젝트를 만들기 때문이다.

* 그렇다면 context 변수에 어떻게 애플리케이션 컨텍스트가 들어가는 것일까?

    스프링의 JUnit 확장기능은 테스트가 실행되기 전에 딱 한번만 애플리케이션 컨텍스트를 만들어두고, 테스트 오브젝트가 만들어질 때마다 특별한 방법을 이용해 애플리케이션 컨텍스트
    자신을 테스트 오브젝트의 특정 필드에 주입해주는 것이다. 일종의 DI라고 볼 수 있는데 조금 성격이 다르다.
    
#### 테스트 클래스의 컨텍스트 공유

```JAVA
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "/applicationContext.xml")
```

같은 테스트 클래스와 같은 설정 파일을 사용하는 경우에는 테스트 수행 중에 단 한개의 애플리케이션 컨텍스트가 만들어진다.

#### @Autowired

* @Autowired

      스프링의 DI에 사용되는 특별한 애노테이션이다. @Autowired 가 붙은 인스턴스 변수가 있으면 테스트 컨텍스트 프레임워크는 변수 타입과 일치하는 컨텍스트 내의 빈을 찾는다. 
      타입과 일치하는 빈이 있으면 인스턴스 변수에 주입해준다. 
    
      일반적으로 생성자나 수정자 메소드 같은 메소드가 있어야 하지만 이 경우에는 메소드가 없어도 주입이 가능하다. 별도의 Di 설정 없이 필드의 타입정보를 이용해 빈을 자동으로 가져올 수 있는데
      이런 방법을 타입에 의한 자동 와이어링이라고 한다.
      
* ApplicationContext를 Autowired로 가져올 수 있는 이유는 ?
    
      스프링 ApplicationContext는 초기화할 때 자기 자신도 빈으로 등록한다.
    
UserDao를 직접 DI 받도록 만든 테스트 UserDaoTest 
```JAVA
......
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "/applicationContext.xml")
public class UserDaoTest {
    @Autowired
    private ApplicationContext context;

    @Autowired
    private UserDao dao;

    private User user1;
    private User user2;
    private User user3;

    @Before
    public void setUp() {
        System.out.println(this.context);
        System.out.println(this);

        this.user1 = new User("gyumee", "박성철", "springno1");
        this.user2 = new User("leegw700", "이길원", "springno2");
        this.user3 = new User("bumjin", "박범진", "springno3");
    }
......
```    

@Autowired는 변수에 할당 가능한 타입을 가진 빈을 자동으로 찾는다. 단, @Autowired는 같은 타입의 빈이 두 개 이상 있는 경우에는 타입만으로는
어떤 빈을 가져올지 결정할 수 없다. @Autowired는 타입으로 가져올 빈 하나를 선택할 수 없는 경우에는 변수의 이름과 같은 이름의 빈이 있는지 확인한다.
변수 이름으로도 빈을 찾을 수 없는 경우에는 예외가 발생한다.

### 2.4.2 DI와 테스트

우리는 구체적인 클래스보다는 인터페이스를 두고 DI를 적용해야 한다. 그래야 하는 이유를 한번 생각해보자

    1. 소프트웨어 개발에서 절대로 바뀌지 않는 것은 없기 때문이다.
    2. 클래스의 구현 방식은 바뀌지 않는다고 하더라도 인터페이스를 두고 적용하게 해두면 다른 차원의 서비스 기능을 도입할 수 있기 때문이다.
    3. 테스트 때문이다. 단지 효율적인 테스트를 손쉽게 만들기 위해서라도 DI를 적용해야한다. (테스트 클래스로 대체)
       DI는 테스트가 작은 단위의 대상에 대해 독립적으로 만들어지고 실행되게 하는데 중요한 역할을 한다.
       
#### 테스트 코드에 의한 DI

* SingleConnectionDataSource 
    
    Db커넥션을 하나만 만들어두고 계속 사용하기 때문에 매우 빠르다.
    
* @DirtiesContext

      이 애노테이션은 스프링의 테스트 컨텍스트 프레임워크에게 해당 클래스의 테스트에서 애플리케이션 컨텍스트이 상태를 변경한다는 것을 알려준다. 
      테스트 컨텍스트는 이 애노테이션이 붙은 테스트 클래스에는 애플리케이션 컨텍스트 공유를 허용하지 않는다.
      테스트 메소드를 수행하고 나면 매번 새로운 애플리케이션 컨텍스트를 만들어서 다음 테스트가 사용하게 해준다.
      테스트 중에 변경한 컨텍스트가 뒤의 테스트에 영향을 주지 않기 위해서다.

테스트를 위한 수동 DI를 적용한 UserDaoTest
```JAVA
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "/applicationContext.xml")
@DirtiesContext
public class UserDaoTest {
    @Autowired
    private ApplicationContext context;

    @Autowired
    private UserDao dao;

    private User user1;
    private User user2;
    private User user3;
    
    @Before
    public void setUp() {
        DataSource dataSource = new SingleConnectionDataSource(
                "jdbc:mariadb://localhost:3306/tobechap01", "bootuser", "bootuser", true
        );
        dao.setDataSource(dataSource);
.....
```    

#### 테스트를 위한 별도의 DI설정

테스트 DB 변경 설정 test-applicationContext.xml 
```JAVA
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
        <bean id="userDao" class="springbook.project.user.dao.UserDao">
            <property name="dataSource" ref="dataSource"/>
        </bean>
        <bean id="dataSource" class="org.springframework.jdbc.datasource.SimpleDriverDataSource">
            <property name="driverClass" value="org.mariadb.jdbc.Driver"/>
            <property name="url" value="jdbc:mariadb://localhost/testdb"/> <!--test-->
            <property name="username" value="bootuser"/>
            <property name="password" value="bootuser"/>
        </bean>
</beans>
```

테스트용 설정 파일 적용
```JAVA
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "/test-applicationContext.xml")
//@DirtiesContext
public class UserDaoTest {
....
```

#### 컨테이너 없는 DI 테스트

애플리케이션 컨텍스트 없는 DI테스트
```JAVA
public class UserDaoTest {
    @Autowired
    private UserDao dao;
...
    @Before
    public void setUp() {
        dao = new UserDao();
        DataSource dataSource = new SingleConnectionDataSource(
                "jdbc:mariadb://localhost:3306/tobechap01", "bootuser", "bootuser", true
        );
        dao.setDataSource(dataSource);
        ....
```

DI를 위해 컨테이너가 반드시 필요한 것은 아니다. DI 컨테이너나 프레임워크는 DI를 편하게 적용하도록 도움을 줄 뿐 컨테이너가 DI를 가능하게 해주는 것은 아니다.

#### DI를 이용한 테스트 방법 선택

항상 스프링 컨테이너 없이 테스트할 수 있는 방법을 가장 우선적으로 고려하자. 

하지만 복잡한 의존관계를 위해 스프링 컨테이너로 DI를 사용할 수도 있고, 테스트용 의존관계를 지정할 수도 있다.

## 2.5 학습 테스트로 배우는 스프링

### 2.5.1 학습 테스트의 장점

### 2.5.2 학습 테스트 예제

#### JUnit 테스트 오브젝트 테스트
    
JUnit 테스트 오브젝트 생성에 대한 학습 테스트
```JAVA
package springbook.project.learningtest.junit;

import org.junit.Test;

import static org.hamcrest.MatcherAssert.assertThat;
import static org.hamcrest.Matchers.*;

public class JUnitTest {
    static JUnitTest testObject;

    @Test
    public void test1() {
        assertThat(this, is(not(sameInstance(testObject))));
        testObject = this;
    }

    @Test
    public void test2() {
        assertThat(this, is(not(sameInstance(testObject))));
        testObject = this;
    }

    @Test
    public void test3() {
        assertThat(this, is(not(sameInstance(testObject))));
        testObject = this;
    }
}
```

* not() : 뒤에 나오는 결과를 부정하는 매처

* is() : equals() 비교를 해서 같은 성공

* sameInstance() : 실제로 같은 오브젝트인지를 비교

* hasItem() : 컬렉션의 원소인지를 검사한다.


개선한 JUnit 테스트 오브젝트 생성에 대한 학습 테스트
```JAVA
package springbook.project.learningtest.junit;

import org.junit.Test;

import java.util.HashSet;
import java.util.Set;

import static org.hamcrest.MatcherAssert.assertThat;
import static org.hamcrest.Matchers.*;

public class JUnitTest {
    static Set<JUnitTest> testObject = new HashSet<JUnitTest>();

    @Test
    public void test1() {
        assertThat(testObject, not(hasItem(this)));
        testObject.add(this);
    }

    @Test
    public void test2() {
        assertThat(testObject, not(hasItem(this)));
        testObject.add(this);
    }

    @Test
    public void test3() {
        assertThat(testObject, not(hasItem(this)));
        testObject.add(this);
    }
}
```

#### 스프링 테스트 컨텍스트 테스트


```JAVA
package springbook.project.learningtest.junit;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.ApplicationContext;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import java.util.HashSet;
import java.util.Set;

import static org.hamcrest.MatcherAssert.assertThat;
import static org.hamcrest.Matchers.*;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration
public class JUnitTest {
    @Autowired
    ApplicationContext context;

    static Set<JUnitTest> testObject = new HashSet<JUnitTest>();
    static ApplicationContext contextObject = null;

    @Test
    public void test1() {
        assertThat(testObject, not(hasItem(this)));
        testObject.add(this);

        assertThat(contextObject==null || contextObject==this.context, is(true));
        contextObject = this.context
    }

    @Test
    public void test2() {
        assertThat(testObject, not(hasItem(this)));
        testObject.add(this);

        assertThat(contextObject==null || contextObject==this.context, is(true));
        contextObject = this.context
    }

    @Test
    public void test3() {
        assertThat(testObject, not(hasItem(this)));
        testObject.add(this);

        assertThat(contextObject==null || contextObject==this.context, is(true));
        contextObject = this.context
    }
}
```

### 2.5.3 버그 테스트

* 버그 테스트(bug test)

코드에 오류가 있을 때 그 오류를 가장 잘 드러내줄 수 있는 테스트를 말한다. 버그 테스트는 실패하도록 만든다. 그리고 버그를 성공할 때 까지 코드를 개선한다.





    
    
