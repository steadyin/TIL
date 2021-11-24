# 3. 템플릿

```
템플릿이란

코드를 살펴보면 어떤 부분은 변경을 통해 기능의 다양성과 확장성을 가질려는 부분도 있고, 고정되어 외부의 변화에도 변하지 않으려는
부분이 있다. 템플릿이란 변경이 거의 일어나지 않으며 일정한 패턴으로 유지되는 특성을 가진 부분을 변경이 필요한 부분으로 독립시켜서
효과적으로 활용할 수 있는 방법이다.
```

## 3.1 다시 보는 초난감 DAO

### 3.1.1 예외처리 기능을 갖춘 DAO

DB 컨넥션이라는 제한적인 리소스를 공유해 사용하는 서버에서 동작하는 JDBC 코드에는 반드시 지켜야 할 원칙이 있다. 바로 예외처리다. 정상적인 JDBC 코드의 흐름을 따르지 않고 중간에 어떤 이유로든 예외가 발생했을 경우에도 사용한 리소스를 반드시 반환하도록 만들어야 하기 때문이다.

#### JDBC 수정 기능의 예외처리 코드

JDBC API를 이용한 DAO 코드인 deleteAll()
```JAVA
    public void deleteAll() throws SQLException {
        Connection c = dataSource.getConnection();

        PreparedStatement ps = c.prepareStatement("DELETE FROM users");
        ps.executeUpdate();

        ps.close();
        c.close();
    }
```

코드는 Connection과 PreparedStatement라는 두 개의 공유 리소스를 가져와서 사용한다. 처리 중 예외가 발생하면 어떻게 될까?
close() 메소드가 실행되지 않아서 리소스가 반환디ㅗ지 않을 수 있다.

일반적으로 서버에서 제한된 개수의 DB커넥션을 만들어서 재사용 가능한 풀로 관리한다.
DB 풀은 매번 getConnection()으로 가져간 커넥션을 명시적으로 close()해서 돌려줘야지만 다시 풀에 넣었다가 다음 커넥션 요청이 있을 때 재사용할 수 있다.

```
리소스 반환과 close()

Connection이나 PreparedStatement의 close()는 리소스를 반환한다는 의미이다. Connection과 PreparedStatement는 보통 
풀(Pool)방식으로 운영된다. 미리 정해진 풀 안에 제한된 수의 리소스(Connection, Statement)를 만들어두고 필요할 때 이를
할당하고 반환하면 다시 풀에 넣는 방식으로 운영한다.
```

JDBC 코드에서는 어떤 상황에서도 가져온 리소스를 반환하도록 try/catch/finally 구문 사용을 권장하고 있다.

예외 발생 시에도 리소스를 반환하도록 수정한 deleteAll()
```JAVA
    public void deleteAll() throws SQLException {
        Connection c = null;
        PreparedStatement ps = null;

        try {
            c = dataSource.getConnection();
            ps = c.prepareStatement("DELETE FROM users");

            ps.executeUpdate();
        } catch(SQLException e) {
            throw e;
        } finally {
            if (ps != null)
                try {
                    ps.close();
                } catch (SQLException e) {
                }
            if (c != null)
                try {
                    c.close();
                } catch (SQLException e) {
            }
        }
    }
```

어디서 예외가 발생했는지 모르기 때문에 finally 블록에서 null체크를 통해 각 자원을 반납한다. close()에서 예외가 발생해도 해줄 수 있는게 없기 때문에 catch 블록은 빈블록으로 처리한다. 
ps.close()와 c.close()은 둘다 예외가 발생할 수 있기 때문에 나누서 예외처리를 한다.

#### JDBC 조회 기능의 예외처리

조회를 수행하는 JDBC 코드는 ResultSet도 반환을 해야되기 때문에 더 복잡해진다.

```JAVA
public class UserDao {
...
    public int getCount() throws SQLException {
        Connection c = null;
        PreparedStatement ps = null;
        ResultSet rs = null;

        try {
            c = dataSource.getConnection();

            ps = c.prepareStatement("select count(*) from users");

            rs = ps.executeQuery();

            rs.next();

            int count = rs.getInt(1);

            return count;
        } catch (SQLException e) {
            throw e;
        } finally {
            if (rs != null) {
                try {
                    rs.close();
                } catch (SQLException e) {
                }
            }
            if (ps != null) {
                try {
                    ps.close();
                } catch (SQLException e) {
                }
            }
            if (c != null) {
                try {
                    c.close();
                } catch (SQLException e) {
                }
            }
        }
...
}
```

## 3.2 변하는 것과 변하지 않는 것

### 3.2.1 JDBC try/catch/finally 코드의 문제점

우리가 지금까지 작성한 JDBC 코드의 문제 핵심은 변하지 않는 그러나 많은 곳에서 중복되는 코드 와 로직에 따라 자꾸 확장되고 자주 변하는 코드를 잘 분리해내는 작업이다.

### 3.2.2 분리와 재사용을 위한 디자인 패턴 적용

### 메소드 추출

먼저 생각해볼 수 있는 방법은 변하는 부분을 메소드로 빼는 것이다. 변하지 않는 부분이 변하는 부분을 감싸고 있어서 반대로 수행하는 것이다.

변하는 부분을 메소드로 추출한 후의 deleteAll()
```JAVA
public class UserDao {
...
    public void deleteAll() throws SQLException {
        Connection c = null;
        PreparedStatement ps = null;
        
        try {
            c = dataSource.getConnection();

            ps = makeStatement(c);

            ps.executeUpdate();
        } catch (SQLException e) {
            throw e;
        } finally {
            if (ps != null)
                try {
                    ps.close();
                } catch (SQLException e) {
                }
            if (c != null)
                try {
                    c.close();
                } catch (SQLException e) {
                }
        }
    }

    private PreparedStatement makeStatement(Connection c) throws SQLException {
        PreparedStatement ps;
        ps = c.prepareStatement("DELETE FROM users");
        return ps;
    }
...
}
```

#### 템플릿 메소드 패턴의 적용

템플릿 메소드 패턴을 이용해서 분리해보자. 

```
    템플릿 메소드 패턴
    
    템플릿 메소드 패턴은 상속을 통해 기능을 확장해서 사용하는 부분이다. 변하지 않는 부분은 슈퍼클래스에 두고 변하는 부분은
    추상메소드로 정의해서 서브클래스에서 오버라이드하여 새롭게 정의해 쓰도록 하는 것이다.
```    

makeStatement()를 구현한 UserDao 서브클래스
```JAVA
public class UserDaoDeleteAll extends UserDao {
    @Override
    protected PreparedStatement makeStatement(Connection c) throws SQLException {
        PreparedStatement ps;
        ps = c.prepareStatement("delete from users");
        return ps;
    }
}
```

추상메소드로 변경한 UserDao.makeStatement()
```JAVA
public abstract class UserDao {
...
protected abstract PreparedStatement makeStatement(Connection c) throws SQLException;
```
}

가장 큰 문제는 DAO로직마다 상속을 통해서 새로운 클래스를 만들어야 한다는 점이다.

또한 서브클래스와 슈퍼클래스의 관계가 유연하게 연결되어 있지 상속을 통해 확장하느 템플릿 메소드 패턴의 단점이 고스란히 드러난다.

#### 전략 패턴의 적용

```
    전략 패턴
    
    개방 폐쇄 원칙(OCP)를 잘 지키는 구조이면서도 템플릿 메소드 패턴보다 유연하고 확장성이 뛰어난 것이 전략패턴이다.
    오브젝트를 아예 둘로 분리하고 클래스 레벨에서는 인터페이스를 통해서만 의존하도록 만드는 전략 패턴이다. 
    전략 패턴은 OCP 관점에서 보면 확장에 해당하는 변하는 부분을 별도의 클래스로 만들어 추상화된 인터페이스를 통해 위임하는 방식이다.
    
    전략 패턴에서 변하지 않는 부분을 `Context`라 한다. 변하는 부분을 `Strategy`라 한다.
```

StatementStrategy 인터페이스
```JAVA
package springbook.project.user.dao;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public interface StatementStrategy {
    PreparedStatement makePreparedStatement(Connection c) throws SQLException;
}
```

deleteAll() 메소드의 기능을 구현한 StatementStrategy 전략 클래스
```JAVA
package springbook.project.user.dao;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class DeleteAllStatement implements StatementStrategy {
    @Override
    public PreparedStatement makePreparedStatement(Connection c) throws SQLException {
        PreparedStatement ps = c.prepareStatement("delete * from users");
        return ps;
    }
}
```

전략패턴을 따라 DeleteAllStatement가 적용된 deleteAll() 메소드
```JAVA
public class UserDao {
....
     public void deleteAll() throws SQLException {
        Connection c = null;
        PreparedStatement ps = null;

        try {
            c = dataSource.getConnection();

            StatementStrategy strategy = new DeleteAllStatement();

            ps = strategy.makePreparedStatement(c);

            ps.executeUpdate();
        } catch (SQLException e) {
            throw e;
        } finally {
            if (ps != null) {
                try {
                    ps.close();
                } catch (SQLException e) {
                }
            }
            if (c != null) {
                try {
                    c.close();
                } catch (SQLException e) {
                }
            }
        }
    }
....
```
전략 패턴은 필요에 따라 컨텍스트는 그대로 유지되면서 전략을 바꿔 쓸 수 있다. 

그런데 컨텍스트 안에서 이미 구체적인 전략 클래스인 DeleteAllStatement를 사용하도록 고정되어 있다면 뭔가 이상하다.

#### DI 적용을 위한 클라이언트 컨텍스트 분리

Conetext가 어떤 전략을 사용하게 할 것인가는 Context를 사용하는 앞단의 Client가 결정하는게 일반적이다. 

Client가 구체적인 전략의 하나를 선택하고 오브젝트로 만들어서 Context에 전달하는 것이다.

Context는 전달받은 그 Strategy 구현 클래스의 오브젝트를 사용한다.

메소드로 분리한 try/catch/finally 컨텍스트 코드
```JAVA
public class UserDao { 
....
    public void jdbcContextWithStatementStrategy(StatementStrategy stmt) throws SQLException {
        Connection c = null;
        PreparedStatement ps = null;

        try {
            c = dataSource.getConnection();

            ps = stmt.makePreparedStatement(c);

            ps.executeUpdate();
        } catch (SQLException e) {
            throw e;
        } finally {
            if (ps != null) { try { ps.close(); } catch (SQLException e) {} }
            if (c != null) { try { ps.close(); } catch (SQLException e) {} }
        }
    }
....
}
```    

클라이언트 책임을 담당할 deleteAll() 메소드
```JAVA
    public void deleteAll() throws SQLException {
        StatementStrategy strategy = new DeleteAllStatement();

        jdbcContextWithStatementStrategy(strategy);
    }
```    

## 3.3 JDBC 전략 패턴의 최적화

### 3.1 전략 클래스의 추가정보

add() 메소드의 PreparedStaetment 생성 로직을 분리한 클래스
```JAVA
package springbook.project.user.dao;

import springbook.project.user.domain.User;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class AddStatement implements StatementStrategy {
    @Override
    public PreparedStatement makePreparedStatement(Connection c) throws SQLException {

        PreparedStatement ps = c.prepareStatement("insert into users(id,name,password) values(?,?,?)");

        ps.setString(1, user.getId());
        ps.setString(2, user.getName());
        ps.setString(3, user.getPassword());

        return ps;
    }
}
```
user 정보를 AddStatement에 전달해주는 add() 메소드
```JAVA
public class UserDao {
...
    public void add(User user) throws SQLException {
        StatementStrategy strategy = new AddStatement(user);
        jdbcContextWithStatementStrategy(strategy);
    }
...
}
```

User 정보를 생성자로부터 제공받도록 만들었다.

### 3.3.2 전략과 클라이언트 동거

현재 만들어 구조에 두가지 개선점이 보인다.

DAO 메소드마다 새로운 StatementStarategy 구현 클래스를 만들어야 한다는 점이다.

DAO 메소드에서 StatementStrategy에 전달한 User와 같은 부가적인 정보가 있는 경우 이를 위해 오브젝트를 전달받은 생성자와 이를 저장해둘 인스턴스 변수를 번거롭게 만들어야 한다는 점이다.

#### 로컬 클래스

클래스가 많아지는 문제는 UserDao 클래스 안에 내부 클래스로 정의해버리는 방법이 있다.

add() 메소드 내의 로컬 클래스로 이전한 AddStatement
```JAVA
public class UserDao {
....
    public void add(User user) throws SQLException {
        class AddStatement implements StatementStrategy {
            User user;

            public AddStatement(User user) {
                this.user = user;
            }

            @Override
            public PreparedStatement makePreparedStatement(Connection c) throws SQLException {

                PreparedStatement ps = c.prepareStatement("insert into users(id,name,password) values(?,?,?)");

                ps.setString(1, user.getId());
                ps.setString(2, user.getName());
                ps.setString(3, user.getPassword());

                return ps;
            }
        }

        StatementStrategy strategy = new AddStatement(user);
        jdbcContextWithStatementStrategy(strategy);
    }
....
}
```    

내부 클래스의 장점을 이용하면 user 정보를 전달받기 위해 만들었던 생성자와 인스턴스 변수를 제거할 수 있으므로 다음과같이 수정할 수 있다.

add() 메소드의 로컬 변수를 직접 사용하도록 수정한 AddStatement
```JAVA
public class UserDao {
....
    public void add(final User user) throws SQLException {
        class AddStatement implements StatementStrategy {
            @Override
            public PreparedStatement makePreparedStatement(Connection c) throws SQLException {

                PreparedStatement ps = c.prepareStatement("insert into users(id,name,password) values(?,?,?)");

                ps.setString(1, user.getId());
                ps.setString(2, user.getName());
                ps.setString(3, user.getPassword());

                return ps;
            }
        }

        StatementStrategy strategy = new AddStatement();
        jdbcContextWithStatementStrategy(strategy);
    }

...
}
```

#### 익명 내부 클래스

좀 더 간결하게 익명 내부 클래스를 활용할 수도 있다. 

```
    익명 내부 클래스(anonymous inner class) 이름을 갖지 않는 클래스이다.
    클래스 선언과 오브젝트 생성이 결합된 형태로 만들어지며, 상속할 클래스나 구현할 인터페이스를 생성자 대신 사용해서 사용한다.
    클래스를 재사용할 필요가 없고 구현한 인터페이스 타입으로만 사용할 경우에 유용하다.
    new 인터페이스이름() { 클래스 본문 }; 
```    
    
AddStatement를 익명 내부 클래스로 전환    
```JAVA
public class UserDao {
....    
    public void add(final User user) throws SQLException {
        StatementStrategy strategy = new StatementStrategy() {
            @Override
            public PreparedStatement makePreparedStatement(Connection c) throws SQLException {

                PreparedStatement ps = c.prepareStatement("insert into users(id,name,password) values(?,?,?)");

                ps.setString(1, user.getId());
                ps.setString(2, user.getName());
                ps.setString(3, user.getPassword());

                return ps;
            }
        };
        jdbcContextWithStatementStrategy(strategy);
    }
...
}
```    

메소드 파라미터로 이전한 익명 내부 클래스
```JAVA
public class UserDao {
....    
    public void add(final User user) throws SQLException {
        jdbcContextWithStatementStrategy(
                new StatementStrategy() {
                    @Override
                    public PreparedStatement makePreparedStatement(Connection c) throws SQLException {

                        PreparedStatement ps = c.prepareStatement("insert into users(id,name,password) values(?,?,?)");

                        ps.setString(1, user.getId());
                        ps.setString(2, user.getName());
                        ps.setString(3, user.getPassword());

                        return ps;
                    }
                }
        );
    }
...
}
``` 

익명 내부 클래스를 적용한 deleteAll() 메소드
```JAVA
public class UserDao {
....    
    public void deleteAll() throws SQLException {
        jdbcContextWithStatementStrategy(
                new DeleteAllStatement() {
                    @Override
                    public PreparedStatement makePreparedStatement(Connection c) throws SQLException {
                        PreparedStatement ps = c.prepareStatement("delete from users");
                        return ps;
                    }
                }
        );
    }
...
}
```

## 3.4 컨텍스트와 DI

### 3.4.1 JDBC Context의 분리

JDBC의 일반적인 작업 흐름을 담고 있는 jdbcContextWithStatementStrategy()는 다른 DAO에서도 사용 가능하다.

그러니 jdbcContextWithStatementStrategy()를 UserDao 클래스 밖으로 독립시켜서 모든 DAO가 사용할 수 있게 해보자.

#### 클래스 분리

작업 흐름을 분리해서 만든 JdbcContext 클래스
```JAVA
package springbook.project.user.dao;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class JdbcContext {
    private DataSource dataSource;

    public void setDataSource(DataSource dataSource) {
        this.dataSource = dataSource;
    }

    public void workWithStatementSTrategy(StatementStrategy stmt) throws SQLException {
        Connection c = null;
        PreparedStatement ps = null;

        try {
            c = dataSource.getConnection();

            ps = stmt.makePreparedStatement(c);

            ps.executeUpdate();
        } catch (SQLException e) {
            throw e;
        } finally {
            if (ps != null) { try { ps.close(); } catch (SQLException e) { } }
            if (c != null) { try { ps.close(); } catch (SQLException e) { } }
        }
    }
}
```
JdbcContext를 DI 받아서 사용하도록 만든 UserDao
```JAVA
package springbook.project.user.dao;

import org.springframework.dao.EmptyResultDataAccessException;
import springbook.project.user.domain.User;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class UserDao {
    private ConnectionMaker connectionMaker;

    private DataSource dataSource;

    private JdbcContext jdbcContext;

    public void setConnectionMaker(ConnectionMaker connectionMaker) {
        this.connectionMaker = connectionMaker;
    }

    public void setDataSource(DataSource dataSource) {
        this.dataSource = dataSource;
    }

    public void setJdbcContext(JdbcContext jdbcContext) {
        this.jdbcContext = jdbcContext;
    }

    public void add(final User user) throws SQLException {
        jdbcContext.workWithStatementSTrategy(
                new StatementStrategy() {
                    @Override
                    public PreparedStatement makePreparedStatement(Connection c) throws SQLException {

                        PreparedStatement ps = c.prepareStatement("insert into users(id,name,password) values(?,?,?)");

                        ps.setString(1, user.getId());
                        ps.setString(2, user.getName());
                        ps.setString(3, user.getPassword());

                        return ps;
                    }
                }
        );
    }

    public User get(String id) throws SQLException {
...
    }

    public void deleteAll() throws SQLException {
        jdbcContext.workWithStatementSTrategy(
                new StatementStrategy() {
                    @Override
                    public PreparedStatement makePreparedStatement(Connection c) throws SQLException {
                        PreparedStatement ps = c.prepareStatement("delete from users");
                        return ps;
                    }
                }
        );
    }

    public int getCount() throws SQLException {
...
    }
}
```

#### 빈 의존관계 변경

UserDao는 이제 JdbcContext에 의존하고 있다. JdbcContext는 인터페이스인 DataSource와는 달리 구체 클래스다. 이 경우 JdbcContext는 그 자체로 독립적인 JDbc
컨텍스트를 제공해주는 서비스 오브젝트로서 의미가 있을 뿐 구현 방법이 바뀔 가능성이 없다. 따라서 인터페이스를 구현하도록 만들지 않았고 UserDao는 인터페이스를
사이에 두지안혹 DI를 적용하는 특별한 구조가 된다.

JdbcContext 빈을 추가하도록 수정한 설정파일(test-applicationContext.xml)
```XML
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="userDao" class="springbook.project.user.dao.UserDao">
        <property name="dataSource" ref="dataSource"/>
        <property name="jdbcContext" ref="jdbcContext"/>
    </bean>

    <bean id="jdbcContext" class="springbook.project.user.dao.JdbcContext">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <bean id="dataSource" class="org.springframework.jdbc.datasource.SimpleDriverDataSource">
        <property name="driverClass" value="org.mariadb.jdbc.Driver"/>
        <property name="url" value="jdbc:mariadb://localhost/tobechap01"/> <!--test-->
        <property name="username" value="bootuser"/>
        <property name="password" value="bootuser"/>
    </bean>
</beans>
```

### 3.4.2 JdbcContext의 특별한 DI

UserDao와 JdbcContext 사이에는 인터페이스를 사용하지 않고 DI를 적용했다. UserDao와 JdbcContext는 클래스 레벨에서 의존관계가 결정된다.
꼭 인터페이스로 의존관계를 주입할 필요는 없다.

#### 스프링 빈으로 DI

의존관계 주입 이라는 개념을 충실히 따르자면 인터페이스를 사이에 둬서 클래스 레벨에서는 의존관계가 고정되지 않게 하고 런타임 시에 의존할 오브젝트와의 관계를 다이내믹하게 주입해주는 것이 맞다. 따라서 인터페이스를 사용하지 않았다면 엄밀히 말해 온전한 DI라고 볼 수는 없다. 그러나 스프링의 DI는 넓게 보자면 객체와 생성과 관계설정에 대한 제어권한을 외부로 위임했다는 IoC 라는 개념을 포괄한다. 그런 의미에서 JdbcContext를 스프링을 이용해 UserDao 객체에서 사용하게 주입했다는 건 DI의 기본을 따르고 있다고 볼 수 있다.

JdbcContext를 USerDao와 DI구조로 만들어야 할 이유에 대해서 생각해보자

첫번째. JdbcContext가 스프링 컨테이너의 싱글톤 레지스트리에서 관리되는 싱글톤빈이 되기 때문이다. JdbcContext는 JDBC 컨텍스트 메소드를 제공해주는 일종의 서비스 오브젝트로서 의미가 있고 그래서 싱글톤으로 등록돼서 여러 오브젝트에서 공유해 사용되는 것이 이상적이다.

둘째는 JdbcContext가 DI를 통해 다른 빈에 의존하고 있기 때문이다. JdbcContext는 dataSource프로퍼티를 통해 DataSource 오브젝트를 주입받도록 되어 있다. DI를 위해서 주입되는 오브젝트와 주입받는 오브젝트 양쪽 모두 스프링 빈으로 등록돼야 한다. 스프링이 생성하고 관리하는 IoC 대상이어야 DI에 참여할 수 있기 때문이다.

왜 인터페이스를 사용하지 않았을까 ?

UserDao는 항상 JdbcContext 클래스와 함께 사용돼야 한다. 클래스는 구분되어 있지만 이 둘은 강한 응집도를 갖고 있다. 

#### 코드를 이용하는 수동 DI

코드를 이용하는 수동 DI를 하는 방법도 생각해 볼 수 있다. JdbcContext에 주입해줄 의존 오브젝트인 DataSource는 USerDao가 대신 DI받도록 하면 된다. 
UserDao는 직접 DAtaSource 빈을 필요로 하지 않지만 JdbcContext에 대한 DI 작업에 사용할 용도로 제공받는 것이다. 

jdbcContext 빈을 제거한 설정 파일
```XML
...
    <bean id="userDao" class="springbook.project.user.dao.UserDao">
        <property name="dataSource" ref="dataSource"/>
<!--        <property name="jdbcContext" ref="jdbcContext"/>-->
    </bean>

<!--    <bean id="jdbcContext" class="springbook.project.user.dao.JdbcContext">-->
<!--        <property name="dataSource" ref="dataSource"/>-->
<!--    </bean>-->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.SimpleDriverDataSource">
        <property name="driverClass" value="org.mariadb.jdbc.Driver"/>
        <property name="url" value="jdbc:mariadb://localhost/tobechap01"/> <!--test-->
        <property name="username" value="bootuser"/>
        <property name="password" value="bootuser"/>
    </bean>
```

설정파일만 보자면 USerDao가 직접 DAtaSource를 의존하고 있는 것 같지만 내부적으로는 JdbcContext를 통해 간접적으로 DataSource를 사용하고 있을 뿐이다.

JdbcContext생성과 DI작업을 수행하는 setDataSource() 메소드
```JAVA

public class UserDao {
...
    private JdbcContext jdbcContext;

    public void setDataSource(DataSource dataSource) {
        this.dataSource = dataSource;

        this.jdbcContext = new JdbcContext();
        jdbcContext.setDataSource(dataSource);
    }
...
}
```

굳이 인터페이스를 두지 않아도 될 만큼 긴밀한 관계를 갖는 DAO 클래스와 JdbcContext를 어색하게 빈으로 따로 분리하지 않고 내부에서 직접 만들어 사용하면서도 다른 오브젝트에 대한 DI를 적용할 수 있다는 점이다. 

일반적으로 어떤 방법이 낫다고 말할 수는 없다. 왜 그렇게 선택했는지에 대한 분명한 이유와 근거만 있다면 적절하다고 판단되는 방법을 선택해서 사용하면 된다.

## 3.5 템플릿과 콜백

전략 패턴을 스프링에서는 템플릿/콜백 패턴이라고 부른다. 전략 패턴의 컨텍스트를 템플릿이라 부르고 익명 내부 클래스로 만들어지는 오브젝트를 콜백이라고 부른다

 ```
     템플릿(template)
     
     템플릿은 어떤 목적을 위해 미리 만들어둔 모양이 있는 틀을 가리킨다. 템플릿 메소드 패턴은 고정된 틀의 로직을 가진 템플릿 메소드를
     슈퍼클래스에 두고 바뀌는 부분을 서브 클래스의 메소드에 두는 구조로 이뤄진다.
     
     콜백(callback)
     
     콜백은 실행되는 것을 목적으로 다른 오브젝트의 메소드에 전달되는 오브젝트를 말한다. 파라미터로 전달되지만 값을 참조하기 위한 것이 아니라
     특정 로직을 담은 메소드를 실행시키기 위해 사용한다. 자바에서는 메소드 자체를 파라미터로 전달할 방법이 없기 때문에 
     메소드가 담긴 오브젝트를 전달해야 한다. 이를 펑셔널 오브젝트(functional object)라고 한다.
```

### 3.5.1 템플릿/콜백의 동작원리

템플릿은 고정된 작업 흐름을 가진 코드를 재사용한다는 의미에서 붙인 이름이다. 콜백은 템플릿 안에서 호출되는 것을 목적으로 만들어진 오브젝트를 말한다.

#### 템플릿/콜백의 특징

콜백은 일반적으로 하나의 메소드를 가진 인터페이스를 구현한 익명 내부 클래스로 만들어진다고 보면 된다. 콜백 인터페이스의 메소드에는 보통 파라미터가 있다.
파라미터는 템플릿의 작업 흐름 중에 만들어지는 컨텍스트 정보를 전달받을 때 사용된다. 

클라이언트 역할은 템플릿 안에서 실행될 로직을 담은 콜백 오브젝트를 만들고 콜백이 참조할 정보를 제공하는 것이다. 
 
템플릿은 정해진 작업 흐름을 따라 작업을 진행하다가 내부에서 생성한 참조정보를 가지고 콜백 오브젝트의 메소드를 호출한다. 콜백은 클라이언트 메소드에 있는 정보와 템플릿이 제공한 참조정보를
이용해서 작업을 수행하고 그 결과를 다시 템플릿에 돌려준다.

템플릿은 콜백이 돌려준 정보를 사용해서 작업을 마저 수행하고 클라이언트에게 돌려준다.

템플릿 콜백 방식에서는 매번 메서드 단위로 사용할 오브젝트를 새롭게 전달받는다는 것이 특징이다. **<U>콜백 오브젝트가 내부 클래스로서 자신을 생성한 클라이언트 메소드 내의 정보를 직접 참조한다는 것도 템
플릿/콜백의 고유한 특징이다.</U>**

#### JdbcContext에 적용된 템플릿/콜백
    
### 3.5.2 편리한 콜백의 재활용
    
#### 콜백의 분리와 재활용
    
이번에는 복잡한 익명 내부 클래스의 사용을 최소화 할 수 있는 방법을 찾아보자.
    
익명 내부 클래스를 사용한 클라이언트 코드
```JAVA
public class UserDao {
...    
    public void deleteAll() throws SQLException {
        jdbcContext.workWithStatementSTrategy(
                new StatementStrategy() {
                    @Override
                    public PreparedStatement makePreparedStatement(Connection c) throws SQLException {
                        return c.prepareStatement("delete from users");
                    }
                }
        );
    }
...   
```
    
단순 SQL을 필요로 하는 콜백이라면 SQL을 제외한 나머지 코드는 매번 동일할 것이다. SQL 문장만 파라미터로 받아서 바꿀 수 있게 하고 메소드 내용 전체를 분리해 별도의 메소드로 만들어보자.

변하지 않는 부분을 분리시킨 deleteAll() 메소드
```JAVA
public class UserDao {
...
    public void deleteAll() throws SQLException {
        executeSql("delete from users");
    }

    public void executeSql(final String query) throws SQLException {
        jdbcContext.workWithStatementSTrategy(
                new StatementStrategy() {
                    @Override
                    public PreparedStatement makePreparedStatement(Connection c) throws SQLException {
                        return c.prepareStatement(query);
                    }
                }
        );
    }
...
}
```

변하는 것과 변하지 않는 것을 분리하고 변하지 않는 건 유연하게 재활용 할 수 있게 만든다는 간단한 원리를 계속 적용했을 때 이렇게 단순하면서도 안전하게 작성 가능한 JDBC 활용 코드가 완성된다.

#### 콜백과 템플릿의 결합

재사용 가능한 콜백을 담고 있는 메소드라면 DAO가 공유할 수 있는 템플릿 클래스 안으로 옮겨도 된다. 템플릿은 JdbcContext 클래스가 아니라 workWithStatementStrategy() 메소드이므로
JdbcContext 클래스로 콜백 생성과 템플릿 호출이 담긴 executeSql() 메소드를 옮긴다고 해도 문제 될 것은 없다. 

JdbcContext로 옮긴 executeSql() 메소드
```Java
package springbook.project.user.dao;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class JdbcContext {
    private DataSource dataSource;

    public void setDataSource(DataSource dataSource) {
        this.dataSource = dataSource;
    }

    public void workWithStatementSTrategy(StatementStrategy stmt) throws SQLException {
        Connection c = null;
        PreparedStatement ps = null;

        try {
            c = dataSource.getConnection();

            ps = stmt.makePreparedStatement(c);

            ps.executeUpdate();
        } catch (SQLException e) {
            throw e;
        } finally {
            if (ps != null) { try { ps.close(); } catch (SQLException e) { } }
            if (c != null) { try { ps.close(); } catch (SQLException e) { } }
        }
    }

    public void executeSql(final String query) throws SQLException {
        workWithStatementSTrategy(
                new StatementStrategy() {
                    @Override
                    public PreparedStatement makePreparedStatement(Connection c) throws SQLException {
                        return c.prepareStatement(query);
                    }
                }
        );
    }

}
```

JdbcContext로 옮긴 executeSql()을 사용하는 deleteAll() 메소드
```Java
    public void deleteAll() throws SQLException {
        jdbcContext.executeSql("delete from users");
    }
```

결국 JdbcContext 안에 클라이언트와 템플릿, 콜백이 모두 함께 공존하면서 동작하는 구조가 됐다. 

일반적으로 관심사가 다른 코드들은 가능한 분리하는 편이 낫지만 이 경우는 반대로 하나의 목적을 위해 서로 긴밀하게 연관되어 동작하는 응집력이 강한 코드들이기 때문에 한군데 모여 있는게 유리하다.

### 3.5.3 템플릿/콜백의 응용

#### 테스트와 try/catch/finally

파일의 숫자 합을 계산하는 코드의 테스트
```JAVA
package springbook.project.learningtest.template;

import org.junit.Test;

import java.io.IOException;
import static org.hamcrest.MatcherAssert.assertThat;
import static org.hamcrest.Matchers.is;

public class CalcSumTest {
    @Test
    public void sumOfNumbers() throws IOException {
        Calculator calculator = new Calculator();
        int sum = calculator.calcSum(getClass().getResource("numbers.txt").getPath());
        assertThat(sum, is(10));
    }
}
```

처음 만든 Calculator
```JAVA
package springbook.project.learningtest.template;

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class Calculator {
    public Integer calcSum(String path) throws IOException {
        BufferedReader br = new BufferedReader(new FileReader(filepath));
        Integer sum = 0;
        String line = null;
        while((line=br.readLine()) != null) {
            sum += Integer.valueOf(line);
        }
        
        br.close();
        return sum;
    }
}
```

초난감 DAO와 마찬가지로 calcSum() 메소드로 파일을 읽거나 처리하다가 예외가 발생하면 파일이 정상적으로 닫히지 않고 메소드를 빠져나가는 문제가 발생한다.

try/catch/finally를 적용한 calcSum() 메소드
```JAVA
...
    public Integer calcSum(String filepath) throws IOException {
        BufferedReader br = null;

        try {
            br = new BufferedReader(new FileReader(filepath));
            Integer sum = 0;
            String line = null;

            while ((line = br.readLine()) != null) {
                sum += Integer.valueOf(line);
            }
            return sum;
        } catch (IOException e) {
            System.out.println(e.getMessage());
            throw e;
        } finally {
            if(br!=null) {
                try {
                    br.close();
                } catch (IOException e) {
                    System.out.println(e.getMessage());
                }
            }
        }
    }
...
```    

#### 중복의 제거와 템플릿/콜백 설계

템플릿/콜백을 적용해보자. 템플릿/콜백 패턴을 적용할 때는 템플릿과 콜백의 경계를 정하고 템플릿이 콜백에게 콜백이 템플릿에게 각각 전달하는 내용이 무엇인지 가장 중요하다.
그에 따라 인터페이스를 정의해야되기 때문이다.

BufferedReader를 전달받는 콜백 인터페이스
```JAVA
...
public interface BufferReaderCallBack {
     Integer doSomethingWithReader(BufferedReader br) throws IOException;
 }
 ```
 
 BufferedReaderCallback을 사용하는 템플릿 코드
 ```JAVA
 public class Calculator {
 ...
     public Integer fileReadTemplate(String filepath, BufferedReaderCallBack callBack) throws IOException {
        BufferedReader br = null;

        try {
            br = new BufferedReader(new FileReader(filepath));

            Integer sum = callBack.doSomethingWithReader(br);

            return sum;
        } catch (IOException e) {
            System.out.println(e.getMessage());
            throw e;
        } finally {
            if (br != null) {
                try {
                    br.close();
                } catch (IOException e) {
                    System.out.println(e.getMessage());
                }
            }
        }
    }
...
```

템플릿/콜백을 적용한 calcSum() 메소드
```JAVA
public class Calculator {
...
    public Integer calcSum(String filepath) throws IOException {
        BufferedReaderCallBack callBack = new BufferedReaderCallBack() {
            @Override
            public Integer doSomethingWithReader(BufferedReader br) throws IOException {
                Integer sum = 0;
                String line = null;

                while ((line = br.readLine()) != null) {
                    sum += Integer.valueOf(line);
                }
                return sum;
            }
        };

        return fileReadTemplate(filepath, callBack);
    }
...    
```

새로운 테스트 메소드를 추가한 CalcSumTest
```JAVA
package springbook.project.learningtest.template;

import org.junit.Before;
import org.junit.Test;

import java.io.IOException;
import static org.hamcrest.MatcherAssert.assertThat;
import static org.hamcrest.Matchers.is;

public class CalcSumTest {
    Calculator calculator;
    String numFilepath;

    @Before
    public void setUp() {
        this.calculator = new Calculator();
        this.numFilepath = getClass().getResource("/numbers.txt").getPath();
    }
    @Test
    public void sumOfNumbers() throws IOException {
        int sum = calculator.calcSum(numFilepath);
        assertThat(sum, is(10));
    }

    @Test
    public void multiplyOfNumbers() throws IOException {
        int sum = calculator.calcMultiply(numFilepath);
        assertThat(sum, is(24));
    }
}
```

곱을 계싼하는 콜백을 가진 calcMuliply 메소드
```JAVA
    public Integer calcMultiply(String filePath) throws IOException {
        BufferedReaderCallBack callBack = new BufferedReaderCallBack() {
            @Override
            public Integer doSomethingWithReader(BufferedReader br) throws IOException {
                Integer multiply = 1;
                String line = null;

                while ((line = br.readLine()) != null) {
                    multiply *= Integer.valueOf(line);
                }

                return multiply;
            }
        };

        return fileReadTemplate(filePath, callBack);
    }
```

#### 템플릿/콜백의 재설계

라인별 작업을 정의한 콜백 인터페이스
```JAVA
package springbook.project.learningtest.template;

public interface LineCallBack {
    Integer doSomethingWithLine(String line, Integer value);
}
```
LineCallback을 사용하는 템플릿
```JAVA
public class Calculator {
....
    public Integer lineReadTemplate(String filepath, LineCallBack callBack, Integer initVal) throws IOException {
        BufferedReader br = null;

        try {
            br = new BufferedReader(new FileReader(filepath));

            Integer res = initVal;
            String line = null;

            while ((line = br.readLine()) != null) {
                res = callBack.doSomethingWithLine(line, res);
            }

            return res;
        } catch (IOException e) {
            System.out.println(e.getMessage());
            throw e;
        } finally {
            if (br != null) {try {br.close();} catch (IOException e) {System.out.println(e.getMessage());}
            }
        }
    }
... 
```

lineReadTemplate()를 사용하도록 수정한 calcSum(), calcMultiply() 메소드
```    
public class Calculator {
...
    public Integer calcSum(String filepath) throws IOException {
        LineCallBack callBack = new LineCallBack() {
            @Override
            public Integer doSomethingWithLine(String line, Integer res) {
                res += Integer.valueOf(line);
                return res;
            }
        };
        return lineReadTemplate(filepath, callBack, 0);
    }

    public Integer calcMultiply(String filePath) throws IOException {
        LineCallBack callBack = new LineCallBack() {
            @Override
            public Integer doSomethingWithLine(String line, Integer res) {
                return res*= Integer.valueOf(line);
            }
        };

        return lineReadTemplate(filePath, callBack, 1);
    }
...
```

### 제네릭스를 이용한 콜백 인터페이스

타입 파라미터를 추가해서 제네릭 메소드로 만든 lineReadTemplate()
```JAVA
public class Calculator {
...
    public <T> T lineReadTemplate(String filepath, LineCallBack<T> callBack, T initVal) throws IOException {
        BufferedReader br = null;

        try {
            br = new BufferedReader(new FileReader(filepath));

            T res = initVal;
            String line = null;

            while ((line = br.readLine()) != null) {
                res = callBack.doSomethingWithLine(line, res);
            }

            return res;
        } catch (IOException e) {
            System.out.println(e.getMessage());
            throw e;
        } finally {
            if (br != null) {try {br.close();} catch (IOException e) {System.out.println(e.getMessage());}
            }
        }
    }
...
```

문자열 연결 기능 콜백을 이용해 만든 concatenate() 메소드
```JAVA
    public String concatenate(String filePath) throws IOException {
        LineCallBack<String> callBack = new LineCallBack<String>() {
            @Override
            public String doSomethingWithLine(String line, String value) {
                value += line;
                return value;
            }
        };

        return lineReadTemplate(filePath, callBack, "");
    }
```    

concatenate() 메소드에 대한 테스트
```
public class CalcSumTest {
...
    @Test
    public void concatenateStrings() throws IOException {
        String res = calculator.concatenate(numFilepath);
        assertThat(res, is("1234"));
    }
...    
```    

## 3.6 스프링의 JdbcTemplate

스프링에서 제공하는 JDBC 템플릿/콜백을 살펴보자.

JdbcTemplate의 초기화를 위한 코드
```JAVA
public class UserDao {
...
    private JdbcTemplate jdbcTemplate;
...
    public void setDataSource(DataSource dataSource) {
        this.dataSource = dataSource;

        this.jdbcTemplate = new JdbcTemplate();
        jdbcTemplate.setDataSource(dataSource);
    }
...
}
```

### 3.6.1 update()

JdbcTemplate의 콜백은 PreparedStatementCreator 인터페이스의 createPreparedStatement() 메소드다.
템플릿으로부터 Connection을 제공받아서 

JdbcTemplate을 적용한 deleteAll() 메소드
```JAVA
public class UserDao {
...
    public void deleteAll() throws SQLException {
        this.jdbcTemplate.update(
            new PreparedStatementCreator() {
                @Override
                public PreparedStatement createPreparedStatement(Connection con) throws SQLException {
                    return con.prepareStatement("delete from users");
                }
            }
        );
    }...
```

내장 콜백을 사용하는 update()로 변경한 deleteAll() 메소드
```JAVA
public class UserDao {
    public void deleteAll() throws SQLException {    
        this.jdbcTemplate.update("delete from users");
    }
}
```

내장 콜백을 사용하는  update()로 변경한 deleteAll() 메소드
```JAVA
    public void add(final User user) throws SQLException {
        jdbcTemplate.update("insert into users(id,name,password) values(?,?,?)",user.getId(),user.getName(),user.getPassword());
    }
```    

### 3.6.2 QueryForInt

getCount()는 SQL 쿼리를 실행하고 ResultSet을 통해 결과 값을 가져오는 코드다. 이런 작업 흐름을 가진 코드에서 사용할 수 있는 템플릿은 PreparedStatementCreator콜백과 REsultSetExtractory 콜백을 파라미터로 받는 query() 메소드다. ResultSetExtractor는 PreparedStatement의 쿼리를 실행해서 얻은 ResultSet을 전달 받는 콜백이다.

첫번쨰 PreparedStatementCreator 콜백은 템플릿으로부터 Connection을 받고 PreparedStatement를 돌려준다. 두번째 ResultSetExtractor는 템플릿으로부터 ResultSet을 받고 거기서 추출한 결과를 돌려준다.

JdbcTemplate을 이용해 만든 getCount()
```Java
    public int getCount() throws SQLException {
        return this.jdbcTemplate.query(
                new PreparedStatementCreator() {
                    @Override
                    public PreparedStatement createPreparedStatement(Connection connection) throws SQLException {
                        return connection.prepareStatement("select count(*) from users");
                    }
                },
                new ResultSetExtractor<Integer>() {
                    @Override
                    public Integer extractData(ResultSet rs) throws SQLException, DataAccessException {
                        rs.next();
                        return rs.getInt(1);
                    }
                }
        );
    }
```

queryForObject()를 사용하도록 수정한 getCount()
```JAVA
    public Integer getCount() throws SQLException {
        return this.jdbcTemplate.queryForObject("select count(*) from users", Integer.class);
    }
```

jdbcTemplate은 스프링이 제공하는 클래스이지만 DI컨테이너를 굳이 필요로 하지 않는다. UserDao에서처럼 직접 JdbcTemplate 오브젝트를 생성하고 필요한 DataSource를 전달해주기만 하면 JdbcTemplate의 모든 기능을 자유롭게 활용할 수 있다.

### 3.6.3 queryForObject, RowMapper활용

getCount()에서 적용했던 ResultSetExtractor와 RowMapper 콜백을 사용하겠다. ResultSetExtractor와 RowMapper 모두 템플릿으로부터 ResultSet을 전달받고 필요한 정보를 추출해서 리턴하는 방식으로 동작한다. 다른 점은 ResultSetExtractor는 REsultSet을 한 번 전달받아 알아서 추출 작업을 모두 진행하고 최종 결과만 리턴해주면 되는 데 반해 RowMapper는 ResultSet의 로우 하나를 매핑하기 위해 사용되기 때문에 여러 번 호출될 수 있다는 점이다.

queryForObject()와 RowMapper를 적용한 get(0 메소드
```JAVA
    public User get(String id) throws SQLException {
        return this.jdbcTemplate.queryForObject(
                "select * from users where  id = ?",
                new Object[]{id}, //SQL에 바인딩할 파라미터 값, 가변인자 댓니 배열을 사용한다.
                new RowMapper<User>() {
                    @Override
                    public User mapRow(ResultSet rs, int i) throws SQLException {
                        User user = new User();
                        user.setId(rs.getString("id"));
                        user.setName(rs.getString("name"));
                        user.setPassword(rs.getString("password"));
                        return user;
                    }
                }
        );
    }
```

queryForOBject() SQL을 실행해서 받은 로우의 개수가 하나가 아니라면 예외를 던지도록 만들어져 있다. 이 때 EmptyResultDataAccessException이다.

### 3.6.4 query()
#### 기능 정의와 테스트 작성

getAll()에 대한 테스트
```JAVA
    @Test
    public void getAll() throws SQLException {
        dao.deleteAll();

        dao.add(user1);
        List<User> users1 = dao.getAll();
        assertThat(users1.size(), is(1));
        checkSamUser(user1, users1.get(0));

        dao.add(user2);
        List<User> users2 = dao.getAll();
        assertThat(users2.size(), is(2));
        checkSamUser(user1, users2.get(0));
        checkSamUser(user2, users2.get(1));

        dao.add(user3);
        List<User> users3 = dao.getAll();
        assertThat(users3.size(), is(3));
        checkSamUser(user3, users3.get(0));
        checkSamUser(user1, users3.get(1));
        checkSamUser(user2, users3.get(2));
    }
```

#### query() 템플릿을 이용하는 getAll() 구현

queryForObject() 는 쿼리의 결과가 로우 하나일 때 사용하고, query()는 여러 개의 로우가 결과로 나오는 일반적인 경우에 쓸 수 있다.
query()는 제네릭 메소드로 타입은 파라미터로 넘기는 RowMapper<\T> 콜백 오브젝트에서 결정된다. query()의 리턴 타입은 List<\T>다.
    
getAll() 메소드
```JAVA
    public List<User> getAll() {
        return this.jdbcTemplate.query(
                "select * from users order by id",
                new RowMapper<User>() {
                    @Override
                    public User mapRow(ResultSet rs, int i) throws SQLException {
                        User user = new User();
                        user.setId(rs.getString("id"));
                        user.setName(rs.getString("name"));
                        user.setPassword(rs.getString("password"));
                        return user;
                    }
                }
        );
    }
```

### 테스트 보완
    
데이터가 없는 경우에 대한 검증 코드가 추가된 getAll() 테스트

```JAVA
public void getAll() {
    dao.deleteAll();
    
    List<User> users0 = dao.getAll();
    assertThat(users0.size(), is(0));
    ...
}
```

### 3.6.5 재사용 가능한 콜백의 분리

#### DI를 위한 코드 정리

불필요한 DataSource 변수를 제거하고 남은 UserDao의 DI코드

```JAVA
public class UserDao {
...
    private JdbcTemplate jdbcTemplate;

    public void setDataSource(DataSource dataSource) {
        this.jdbcTemplate = new JdbcTemplate(dataSource);
    }
...
```

#### 중복 제거

재사용 가능하도록 독립시킨 RowMapper
```JAVA
    private RowMapper<User> rowMapper =
            new RowMapper<User>() {
                @Override
                public User mapRow(ResultSet rs, int i) throws SQLException {
                    User user = new User();
                    user.setId(rs.getString("id"));
                    user.setName(rs.getString("name"));
                    user.setPassword(rs.getString("password"));
                    return user;
                }
            };
```

userMapper를 사용하도록 수정한 get(), getAll()
```JAVA
    public User get(String id) throws SQLException {
        //SQL에 바인딩할 파라미터 값, 가변인자 대신 배열을 사용한다.
        return this.jdbcTemplate.queryForObject("select * from users where  id = ?",new Object[]{id},rowMapper);
    }

    public List<User> getAll() {
        return this.jdbcTemplate.query("select * from users order by id",rowMapper);
    }
```

#### 템플릿/콜백 패턴과 UserDao

JdbcTemplate을 적용한 UserDao 클래스

```JAVA
package springbook.project.user.dao;

import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;
import springbook.project.user.domain.User;

import javax.sql.DataSource;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.List;

public class UserDao {
    private JdbcTemplate jdbcTemplate;

    private RowMapper<User> rowMapper =
            new RowMapper<User>() {
                @Override
                public User mapRow(ResultSet rs, int i) throws SQLException {
                    User user = new User();
                    user.setId(rs.getString("id"));
                    user.setName(rs.getString("name"));
                    user.setPassword(rs.getString("password"));
                    return user;
                }
            };

    public void setDataSource(DataSource dataSource) {
        this.jdbcTemplate = new JdbcTemplate(dataSource);
    }

    public void add(final User user) throws SQLException {
        jdbcTemplate.update("insert into users(id,name,password) values(?,?,?)", user.getId(), user.getName(), user.getPassword());
    }

    public User get(String id) throws SQLException {
        //SQL에 바인딩할 파라미터 값, 가변인자 대신 배열을 사용한다.
        return this.jdbcTemplate.queryForObject("select * from users where  id = ?",new Object[]{id},rowMapper);
    }

    public List<User> getAll() {
        return this.jdbcTemplate.query("select * from users order by id",rowMapper);
    }

    public void deleteAll() throws SQLException {
        this.jdbcTemplate.update("delete from users");
    }

    public Integer getCount() throws SQLException {
        return this.jdbcTemplate.queryForObject("select count(*) from users", Integer.class);
    }
}
```

UserDao에는 User정보를 DB에 넣거나 가져오거나 조작하는 방법에 대한 핵심적인 로직만 담겨 있다. 사용할 테이블과 필드정보가 바뀌면 UserDao의 거의 모든 코드가 함께 바뀐다. 따라서 응집도가 높다고 볼 수 있다.

JDBC API를 사용하는 방식, 예외처리, 리소스의 반납, DB 연결을 어떻게 가져올지에 관한 책임과 관심은 모두 JdbcTemplate에게 있다. 따라서 변경이 일어난다고 해도 USerDAo 코드에는 아무런 영향을 주지 않는다. 그런 면에서 책임이 다른 코드와는 낮은 결합도를 유지하고 있다.


    
