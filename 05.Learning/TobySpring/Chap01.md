# 1장 오브젝트와 의존관계

## 1.1 초난감 DAO

* DAO(Data Access Object)

      DAO는 DB를 사용해 데이터를 조회하거나 조작하는 기능을 전담하도록 만든 오브젝트를 말한다.
        
### 1.1.1 User

```JAVA
package springbook.project.user.domain;

public class User {

    String id;
    String name;
    String password;

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

* 자바빈(JavaBean)
    
      다음 두 관례를 따라 만들어진 오브젝트를 가리킨다.

      1. 디폴트 생성
       
        자바빈은 파라미터가 없는 디폴트 생성자를 갖고 있어야 한다. 리플렉션을 이용해 오브젝트를 생성하기 때문에 필요하다.
    
      2. 프로퍼티
         
         자바빈이 노출하는 이름을 가진 속성을 프로퍼티라고 한다.
         수정자 메소드(Setter)와 접근자 메소드(Getter)을 이용해 조회 또는 수정할 수 있다.

### 1.1.2 UserDao

* JDBC 작업 순서

    1. DB 드라이버를 로드한다. <br>
          ```JAVA        
        Class.forName("com.mysql.jdbc.Driver"); 
        ```
    2. DB 연결을 위한 Connection을 가지고 온다.
        ```JAVA        
        Connection c = DriverManager.getConnection("jdbc url","id","password");
        ```
    3. SQL을 담은 Statement를 만든다. 
        ```JAVA
        PreparedStatement ps = c.prepareStatement("insert into users(id, name, password) values(?,?,?)");
        ```
    4. Statement를 실행한다. 
        ```JAVA
        ps.executeUpdate();
        ```
    5. Connection, Statement, ResultSet같은 리소스는 작업을 마친 후 닫아준다. 
        ```
        ps.close();
        c.close();
        ```
    6. JDBC API가 만들어내는 예외를 잡아서 직접 처리하거나 메소드 밖으로 던지게 한다. <br>

* Class.forName 동작원리
    ```JAVA       
        Class.forName("com.mysql.jdbc.Driver")
        Connection c = DriverManager.getConnection("jdbc:mysql://localhost/springbook", "spring", "book");
    ```
    
      JVM에게 해당 클래스 정보를 로드한다. 위 코드는 com.mysql.jdbc.Driver 클래스 메타 정보를 로드하는 것이다.
      DriveManager 클래스의 Static영역을 살펴보면 로드된 Driver클래스를 생성하는 것을 알 수 있다.

```JAVA
package springbook.project.user.dao;

import springbook.project.user.domain.User;

import java.sql.*;

public class UserDao {
    public void add(User user) throws ClassNotFoundException, SQLException {
        Class.forName("org.mariadb.jdbc.Driver");
        Connection c = DriverManager.getConnection("jdbc:mariadb://localhost:3306/tobechap01?characterEncoding=UTF-8&serverTimezone=UTC",
                "steadyin", "steadyin");

        PreparedStatement ps = c.prepareStatement("insert into users(id,name,password) values(?,?,?)");
        ps.setString(1, user.getId());
        ps.setString(2, user.getName());
        ps.setString(3, user.getPassword());

        ps.executeUpdate();
        c.close();
    }

    public User get(String id) throws ClassNotFoundException, SQLException {
        Class.forName("org.mariadb.jdbc.Driver");
        Connection c = DriverManager.getConnection("jdbc:mariadb://localhost:3306/tobechap01?characterEncoding=UTF-8&serverTimezone=UTC",
                "steadyin", "steadyin");

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
}
```

### 1.1.3 main()을 이용한 DAO 테스트 코드

```Java
package springbook.project;

import org.springframework.boot.autoconfigure.SpringBootApplication;
import springbook.project.user.dao.UserDao;
import springbook.project.user.domain.User;

import java.sql.SQLException;

@SpringBootApplication
public class ProjectApplication {

    public static void main(String[] args) throws SQLException, ClassNotFoundException {
        UserDao dao = new UserDao();

        User user = new User();
        user.setId("whiteship");
        user.setName("백기선");
        user.setPassword("married");

        dao.add(user);

        System.out.println(user.getId() + " 등록 성공");

        User user2 = dao.get(user.getId());
        System.out.println(user2.getName());
        System.out.println(user2.getPassword());

        System.out.println(user.getId() + " 조회 성공");
    }
}
```

## DAO의 분리

### 관심사의 분리

`변화를 어떻게 대비할 것인가` 고민으로 부터 나온다.
변화는 대체로 집중된 한가지 관심에 대해 일어나지만 그에 따른 작업은 집중되지 않는 경우가 많다는 점이다.
변화가 한 번에 한 가지 관심에 집중돼서 일어난다면 우리가 준비해야 할 일은 한가지 관심이 한 군데에 집중되게 하는 것이다.

* 관심사의 분리(Separation of Concerns)

      관심이 같은 것끼리는 하나의 객체 안으로 또는 친한 객체로 모이게 하고, 관심이 다른 것은 가능한 따로 떨어져서 서로 영향을 주지 않도록 분리하는 것이라고 할 수 있다.
      (비슷한 관심사를 하나의 객체에 모으라는 말은 아니다. 하나의 객체에 집중 될 수록 종속성이 커질 수도 있으므로 관심사를 올바르게 분리하는 것이 중요하다.)

### 1.2.2 커넥션 만들기 호출

#### UserDao의 관심사항

1. DB와 연결을 위한 커넥션을 어떻게 가져올 것인가?
2. 사용자 등록을 위해 DB에 보낼 SQL 문장을 담을 Statement를 만들고 실행하는 것
3. 작업이 끝나면 사용한 리소스의 Statement와 Connection 오브젝트를 닫아줘서 소중한 공유 리소르를 시스템에 돌려주는 것이다.

#### 중복 코드의 메소드 호출

가장 먼저 할 일은 커넥션을 가져오는 중복된 코드를 분리하는 것이다. 

```JAVA
package springbook.project.user.dao;

import springbook.project.user.domain.User;

import java.sql.*;

public class UserDao {
    public void add(User user) throws ClassNotFoundException, SQLException {
        Connection c = this.getConnection();

        PreparedStatement ps = c.prepareStatement("insert into users(id,name,password) values(?,?,?)");
        ps.setString(1, user.getId());
        ps.setString(2, user.getName());
        ps.setString(3, user.getPassword());

        ps.executeUpdate();
        c.close();
    }

    public User get(String id) throws ClassNotFoundException, SQLException {
        Connection c = this.getConnection();

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

    private Connection getConnection() throws ClassNotFoundException, SQLException {
        Class.forName("org.mariadb.jdbc.Driver");
        Connection c = DriverManager.getConnection("jdbc:mariadb://localhost:3306/tobechap01?characterEncoding=UTF-8&serverTimezone=UTC",
                "bootuser", "bootuser");
        return c;
    }
}
```

기능이 추가되거나 바뀐 것은 없지만 이전보다 훨씬 깔끔해졌고 미래의 변화에 좀 더 손쉽게 대응할 수 있는 코드가 됐다. 이를 리팩토링이라 한다. 또한 이런 공통의 기능을 담당하는 메소드로 중복된 코드를 뽑아내는 것을 리팩토링에서는 메소드 추출(extract method) 기법이라고 한다.

* 리팩토링

    리팩토링은 기존의 코드를 외부의 동작방식에는 변화 없이 내부 구조를 변경해서 재구성, 개선하는 작업을 말한다.

### 1.2.3 DB 커넥션 만들기의 독립

문제가 발생했다. 두 회사에서 서로 다른 종류의 DB를 사용하고 있고, DB 커넥션을 가져오는 데 있어 독자적인 방법을 적용하고 싶어한다는 점이다. UserDao 소스코드를 제공하지 않고도 고객사가 DB 커넥션 생성 방식을 적용해가면서 UserDao를 사용하게 할 수 있을까?

#### 상속을 통한 확장

UserDao에서 메소드의 구현 코드를 제거하고 getConnection()을 추상 메소드로 만들어 놓는다. 이제 UserDao 클래스를 상속해서 각각 NUserDao와 DUserDao라는 서브클래스를 만들어 getConnection()을 구현하면 된다. DB 커넥션 연결이라는 관심을 이번에는 상속을 통해 서브클래스로 분리해버리는 것이다.

```JAVA
package springbook.project.user.dao;

import springbook.project.user.domain.User;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public abstract class UserDao {
    public void add(User user) throws ClassNotFoundException, SQLException {
        Connection c = this.getConnection();

        PreparedStatement ps = c.prepareStatement("insert into users(id,name,password) values(?,?,?)");
        ps.setString(1, user.getId());
        ps.setString(2, user.getName());
        ps.setString(3, user.getPassword());

        ps.executeUpdate();
        c.close();
    }

    public User get(String id) throws ClassNotFoundException, SQLException {
        Connection c = this.getConnection();

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

    public void delete() throws ClassNotFoundException, SQLException {
        Connection c = this.getConnection();

        PreparedStatement ps = c.prepareStatement("DELETE FROM users");
        ps.executeUpdate();

        ps.close();
        c.close();
    }

    public abstract Connection getConnection() throws ClassNotFoundException, SQLException;
}


public class DUserDao extends UserDao {
    @Override
    public Connection getConnection() throws ClassNotFoundException, SQLException {
        //D사 DB Connection 생성코드
        Class.forName("org.mariadb.jdbc.Driver");
        Connection c = DriverManager.getConnection("jdbc:mariadb://localhost:3306/tobechap01?characterEncoding=UTF-8&serverTimezone=UTC",
                "bootuser", "bootuser");
        return c;
    }
}

public class NUserDao extends UserDao {
    @Override
    public Connection getConnection() throws ClassNotFoundException, SQLException {
        // N사 DB Connection 생성코드
        Class.forName("org.mariadb.jdbc.Driver");
        Connection c = DriverManager.getConnection("jdbc:mariadb://localhost:3306/tobechap01?characterEncoding=UTF-8&serverTimezone=UTC",
                "bootuser", "bootuser");
        return c;
    }
}
```
클래스 계층 구조를 통해 두 개의 관심이 독립적으로 분리되면서 확장에 유용한 코드가 되었다. 슈퍼클래스에 기본적인 로직의 흐름을 만들고 그 기능의 일부를 추상 메소드나 오버라딩이 가능한 protected 메소드 등으로 만든 뒤 서브클래스에서 이런 메소드를 필요에 맞게 구현해서 사용하도록 하는 템플릿 메소드 패턴을 적용했다.

서브클래스에서 구체적인 오브젝트 생성 방법을 결정하게 하는 것을 팩토리 메소드 패턴이라고도 한다. getConnection() 의 구현방법은 서로 다르겠지만 UserDao는 Connection 인터페이스 타입의 오브젝트라는 것 외에 관심을 두지 않는다.

템플릿 메소드 패턴, 팩토리 메소드 패턴으로 관심사항이 다른 코드를 분리해내고 서로 독립적으로 변경 또는 확장할 수 있도록 만드는 것은 간단하면서도 매우 효과적인 방법이다. 하지만 상속을 사용했다는 단점이 있다. 다중상속을 지원하지 않으므로 상속을 통한 확장을 사용하면 더 이상 확장을 할 수 없다. 상하위 클래스의 관계가 생각보다 밀접하다. DB 커넥션을 생성하는 코드가 다른 DAO에서 중복되고 공유할 수 없다. 또한 슈퍼클래스 내부의 변경이 있을 때 서브클래스를 함께 수정하거나 다시 개발해야 할 수도 있다. 상속을 통한 확장에 단점이다.

* 디자인 패턴

    소프트웨어 설계시 특정 상황에서 문제를 해결하기 위한 설계 패턴, 주로 객체지향설계 원칙을 이용해 문제를 해결한다. 대부분 두 가지 구조로 정리된다. 하나는 클래스 상속이고 다른 하나는 오브젝트 합성이다. 패턴에서 가장 중요한 것은 각 패턴의 핵심이 담긴 목적,의도이다.

* 템플릿 메소드 패턴(Template Method Pattern)

    상속을 통해 슈퍼클래스의 기능을 확장할 때 사용하는 디자인패턴, 변하지 않는 기능은 슈퍼클래스에 두고 자주 변경 되며 확장ㅅ할 기능은 서브 클래스에서 구현하도록 한다. 슈퍼클래스에서는 미리 추상 메소드 또는 오버라이드 가능한 메소드를 정의해두고 이를 활용해 코드의 기본 알고리즘을 담고 있는 템플릿 메소드를 만든다.

* 훅(Hook) 메소드

    서브클래스에서 선택적으로 오버라이드 할 수 있도록 만들어둔 메소드를 훅(hook)라고 한다.

* 팩토리 메소드 패턴(Factory Method Pattern)

    팩토리 메소드 패턴도 상속을 통해 기능을 확장하는 패턴이다. 템플릿 메소드 패턴과 비슷한데 슈퍼클래스 코드에서는 서브클래스에서 구현할 메소드를 호출해서 필요한 타입의 오브젝트를 가져와 사용한다. 주로 인터페이스 타입으로 오브젝트를 리턴하므로 서브클래스에서 어떤 클래스의 오브젝트를 만들어 리턴할지 슈퍼클래스는 알지 못한다. 사실 관심도 없다. 서브클래스에서 오브젝트 생성방법과 클래스를 결정할 수 있도록 정의한 메소드를 팩토리 메소드라고 한다. 

1.3 DAO의 확장

관심사에 따라서 분리한 오브젝트들은 제각기 독특한 변화의 특징이 있다. 추상클래스를 만들고 이를 상속한 서브클래스에서 변화가 필요한 부분을 바꿔서 쓸 수 있게 만든 이유는 변화의 성격이 다른 것을 분리해서 서로 영향을 주지 않은 채로 각각 필요한 시점에 독립적으로 변경할 수 있게 하기 위해서다. 그러나 여러가지 단점이 많은 상속이라는 방법을 사용했다는 사실이 불편하게 느껴진다.

1.3.1 클래스의 분리

상속관계가 아닌 완전 독립적인 클래스로 분리해보겠다. 

독립된 SimpleConnectionMaker를 사용하게 만든 UserDao
```JAVA
package springbook.project.user.dao;

import springbook.project.user.domain.User;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class UserDao {

    private SimpleConnectionMaker simpleConnectionMaker;

    public UserDao() {
        simpleConnectionMaker = new SimpleConnectionMaker();
    }

    public void add(User user) throws ClassNotFoundException, SQLException {
        Connection c = simpleConnectionMaker.makeNewConnection();

        PreparedStatement ps = c.prepareStatement("insert into users(id,name,password) values(?,?,?)");
        ps.setString(1, user.getId());
        ps.setString(2, user.getName());
        ps.setString(3, user.getPassword());

        ps.executeUpdate();
        c.close();
    }

    public User get(String id) throws ClassNotFoundException, SQLException {
        Connection c = simpleConnectionMaker.makeNewConnection();

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

    public void delete() throws ClassNotFoundException, SQLException {
        Connection c = simpleConnectionMaker.makeNewConnection();

        PreparedStatement ps = c.prepareStatement("DELETE FROM users");
        ps.executeUpdate();

        ps.close();
        c.close();
    }
}
```

독립시킨 DB 연결 기능인 SimpleConnectionMaker
```JAVA
package springbook.project.user.dao;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class SimpleConnectionMaker {
    public Connection makeNewConnection() throws ClassNotFoundException, SQLException {
        Class.forName("org.mariadb.jdbc.Driver");
        Connection connection = DriverManager.getConnection("jdbc:mariadb://localhost/tobechap01","bootuser","bootuser");

        return connection;
    }
}
```

기능에 변화를 준 것은 없다. 단지 내부 설계를 변경해서 좀 더 나은 코드로 개선했을 뿐이다. 다른 문제가 발생했다. N사와 D사에 UserDao클래스만 공급하고 상속을 통해 DB 커넥션 기능을 확장해서 사용하게 했던게 불가능해졌다. UserDao 코드가 SimpleConnectionMaker라는 특정 클래스에 종속되어 있기 때문에 상속을 사용했을 때처럼 UserDao코드의 수정 없이 DB 커넥션 생성 기능을 변경할 방법이 없다.

UserDao는 다음과 같이 SimpleConnectionMaker코드에 종속되어 있다.
```JAVA
    simpleConnectionMaker = new SimpleConnectionMaker();
```

두 가지 문제점이 있다. 하나는 SimpleConnectionMaker은 makeNexConnection()을 사용하고 있는데 N사와 D사는 다른 메소드를 사용할 수도 있다. 두번째는 DB커넥션을 제공하는 클래스가 어떤 것인지를 UserDao가 구체적으로 알고 있어야 한다는 점, 종속적이라는 점이다. 

1.3.2 인터페이스의 도입

클래스를 분리하면서도 이런 문제를 해결할 수는 없을까? 가장 좋은 방법은 연결관계를 느슨하게 만들어주는 것이다. 인터페이스는 자신을 구현한 클래스에 대한 구체적인 정보는 모두 감춰버린다. 결국 오브젝트를 만들려면 구체적인 클래스 하나를 선택해야겠지만 인터페이스로 추상화해놓은 최소한의 통로를 통해 접근하는 쪽에서는 오브젝트를 만들때 사용할 클래스가 무엇인지 몰라도된다.

인터페이스는 어떤 일을 구현하겠다는 기능만 정의해놓은 것이다. 구현방법은 구현 클래스들이 알아서 할 일이다. 인터페이스를 사용하게 되면 실행할 수 있는 기능에만 관심이 있지 그 기능이 어떻게 구현했는지는 관심을 둘 필요가 없다.

ConnectionMaker 인터페이스
```JAVA
package springbook.project.user.dao;

import java.sql.Connection;
import java.sql.SQLException;

public interface ConnectionMaker {
    public Connection makeConnection() throws ClassNotFoundException, SQLException;
}

```

D사와 N사는 해당 인터페이스를 토대로 구현 클래스를 만들어서 제공한다.

ConnectionMaker 구현 클래스
```JAVA
package springbook.project.user.dao;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class DConnectionMaker implements ConnectionMaker {
    @Override
    public Connection makeConnection() throws ClassNotFoundException, SQLException {
        Class.forName("org.mariadb.jdbc.Driver");
        Connection connection = DriverManager.getConnection("jdbc:mariadb://localhost/tobechap01", "bootuser", "bootuser");
        return connection;
    }
}
```

ConnectionMaker 인터페이스를 사용하도록 개선한 UserDao
```JAVA
package springbook.project.user.dao;

import springbook.project.user.domain.User;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class UserDao {

    private ConnectionMaker connectionMaker;

    public UserDao() {
        connectionMaker = new DConnectionMaker();
    }

    public void add(User user) throws ClassNotFoundException, SQLException {
        Connection c = connectionMaker.makeConnection();

        PreparedStatement ps = c.prepareStatement("insert into users(id,name,password) values(?,?,?)");
        ps.setString(1, user.getId());
        ps.setString(2, user.getName());
        ps.setString(3, user.getPassword());

        ps.executeUpdate();
        c.close();
    }

    public User get(String id) throws ClassNotFoundException, SQLException {
        Connection c = connectionMaker.makeConnection();

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

    public void delete() throws ClassNotFoundException, SQLException {
        Connection c = connectionMaker.makeConnection();

        PreparedStatement ps = c.prepareStatement("DELETE FROM users");
        ps.executeUpdate();

        ps.close();
        c.close();
    }
}
```

아직 코드에 구체적인 클래스명이 남아있다. 초기에 한 번 어떤 구현 클래스의 오브젝트를 사용할지를 결정하는 생성자의 코드는 제거되지 않고 남아있다.

```JAVA
connectionMaker = new DConnectionMaker();
```

1.3.3 관계설정 책임의 분리

이 때문에 인터페이스를 이용한 분리에도 불구하고 여전히 UserDao 변경 없이는 DB 커넥션 기능의 확장이 자유롭지 못한데 그 이유는 UserDao 안에 분리되지 않은 또 다른 관심사항이 존재하고 있기 때문이다. 

new DConectionMaker() 코드는 짧고 간단하지만 그 자체로 충분히 독립적인 관심사를 담고 있다. 바로 UserDao가 어떤 ConnectionMaker 구현 클래스의 오브젝트를 이용하게 할지를 결정하는 것이다. 분리하지 않으면 UserDao는 독립적으로 확장 가능한 클래스가 될 수 없다.

UserDao의 클라이언트라고 하면 UserDao를 사용하는 오브젝트를 말하는데 UserDao에 UserDao와 ConnectinoMaker 구현 클래스의 관계를 결정해주는 기능을 분리해서 두는 것을 생각해볼 수 있다.

**<u>클래스 사이에 관계가 만들어진다는 것은 한 클래스가 인터페이스 없이 다른 클래스를 직접 사용한다는 뜻이다. 따라서 오브젝트와 오브젝트 사이의 관계를 설정해주어 한다. 오브젝트 사이의 관계는 런타임시 한쪽이 다른 오브젝트의 레퍼런스를 갖고 있는 방식으로 만들어진다. connectionMaker = new DConnectionMaker 코드로 '사용'이라는 관계를 맺게 해준다.</u>**

오브젝트는 얼마든지 메소드 파라미터 등을 이용해 전달할 수 있으니 외부에서 만든 걸 가져올 수도 있다. 이 때 파라미터를 전달받을 오브젝트의 인터페이스로 선언해뒀다고 해보자. 이런 경우 파라미터로 전달되는 오브젝트의 클래스는 해당 인터페이스를 구현하기만 했다면 어떤 것이든 상관없다. 

**<u>물론 UserDao 오브젝트가 동작하려면 특정 클래스의 오브젝트와 관계를 맺어야 한다. 하지만 클래스 사이에 관계가 만들어진 것은 아니고 단지 오브젝트 사이에 다이내믹 관계가 만들어지는 것이다. 클래스 사이의 관계는 코드에 다른 클래스 이름이 나타나기 때문에 만들어지는 것이다. 하지만 오브젝트 관계는 그렇지 않다. 코드에서는 특정 클래스를 전혀 알지 못하더라도 해당 클래스가 구현한 인터페이스를 사용했다면 그 클래스의 오브젝트를 인터페이스 타입으로 받아서 사용할 수 있다. 객체지향 프로그램에서는 `다형성`이라는 특징 덕분이다.</u>**

UserDao 오브젝트가 DConnectionManager 오브젝트를 사용하게 하려면 두 클래스의 오브젝트 사이에 런타임 사용관계 또는 의존관계라고 불리는 관계를 맺어주면 된다. 그리고 UserDao의 클라이언트는 런타임 오브젝트 관계를 갖는 구조로 만들어주는 게 클라이언트의 핵심이다. 클라이언트는 UserDao를 사용해야 할 입장이기 때문에 UserDao의 구현 클래스를 선택하고 연결해줄 수 있다. 

UserDaoTest라느 이름을 만들어 기존 테스트 소스를 옮기고 UserDao의 생성자를 수정해서 클라이언트가 미리 만들어둔 ConnectionMaker의 오브젝트를 전달 받을 수 있도록 해보자.

관계설정 책임이 추가된 UserDao 클라이언트인 main() 메소드
```JAVA
package springbook.project.user.dao;

import springbook.project.user.domain.User;

import java.sql.SQLException;

public class UserDaoTest {
    public static void main(String[] args) throws SQLException, ClassNotFoundException {
        ConnectionMaker connectionMaker = new DConnectionMaker();
        UserDao dao = new UserDao(connectionMaker);

        User user = new User();
        user.setId("whiteship");
        user.setName("백기선");
        user.setPassword("married");

        dao.delete();

        dao.add(user);

        System.out.println(user.getId() + " 등록 성공");

        User user2 = dao.get(user.getId());
        System.out.println(user2.getName());
        System.out.println(user2.getPassword());

        System.out.println(user.getId() + " 조회 성공");
    }
}
```

수정한 생성자
```JAVA
    public UserDao(ConnectionMaker connectionMaker) {
        this.connectionMaker = connectionMaker;
    }
```

UserDaoTest는 UserDao와 ConnectionMaker 구현 클래스와의 런타임 오브젝트 의존관계를 설정하는 책임을 담당해야 한다.

DAO가 많아져도 DB 접속 방법에 대한 관심은 한 군데에 집중되게 할 수 있고 DB 접속 방법을 변경해야 할 때도 오직 한 곳의 코드만 수정하면 된다.

* 개방 폐쇠 원칙(The Open Closed Principle)

    클래스나 모듈은 확장에는 열려 있어야 하고 변경에는 닫혀 있어야 한다. 기능을 확장하는 데는 열려 있고 그런 변화에 영향을 받지 않고 최대한 유지할 수 있으므로 변경에는 닫혀 있다고 말할 수 있다.

* 높은 응집도와 낮은 결합도(High coherence and low coupling)

    개방 폐쇠 원칙은 높은 응집도와 낮은 결합도라는 소프트웨어 개발의 고전적인 원리로도 설명이 가능하다. <br>
    응집도가 높다는 것은 변화가 일어날 때 해당 모듈에서 변하는 부분이 크다는 것으로 설명할 수 있다. 즉 하나의 모듈, 클래스가 하나의 책임 또는 관심사에만 집중되어 있다는 뜻이다. <br>
    결합도가 낮다는 것은 하나의 오브젝트가 변경이 일어날 때에 관계를 맺고 있는 다른 오브젝트에게 변화를 요구하는 정도가 낮다고 설명할 수 있다.

* 전략 패턴(Strategy Pattern)

    전략 패턴은 자신의 기능 맥락(Context)에서 필요에 따라 변경이 필요한 알고리즘을 인터페이스를 통해 통째로 외부로 분리시키고 이를 구현한 구체적인 알고리즘 클래스를 필요에 따라 바꿔서 사용할 수 있게 하는 디자인 패턴이다.

## 1.4 제어의 역전(IoC)

    IoC(Inversion of Control) 제어의 역전

### 1.4.1 오브젝트 팩토리

    클라이언트 UserDaoTest에는 두 가지 관심사가 포함되어 있다. UserDao의 기능이 잘 동작하는 지를 테스트하고 UserDao에 어떤 ConnectionMaker 구현 클래스를 사용할지를 결정한다. 

#### 팩토리

팩토리 클래스의 역할은 객체의 생성 방법을 결정하고 그렇게 만들어진 객체를 돌려주는 일을 하는 객체를 말한다. 

UserDao, ConectionMaker 관련 생성 작업을 DaoFactory로 옮기고 UserDaoTest에서는 DaoFactory에 요청해서 미리 만들어지 UserDao 오브젝트를 가져와 사용하게 만든다.

UserDao의 생성 책임을 맡은 팩토리 클래스
```JAVA
package springbook.project.user.dao;

public class DaoFactory {
    public UserDao userDao() {
        ConnectionMaker connectionMaker = new DConnectionMaker();
        UserDao userDao = new UserDao(connectionMaker);
        return userDao;
    }
}
```

팩토리를 사용하도록 수정한 UserDaoTest
```JAVA
public class UserDaoTest {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        UserDao dao = new DaoFactory().userDao();
        ...
    }
}
```

#### 설계도로서의 팩토리

UserDao와 ConnectionMaker는 데이터 로직과 기술 로직을 담당하고 있다. 전자가 실질적인 로직을 담당하는 컴포넌트라면 후자는 애플리케이션을 구성하는 컴포넌트의 구조와 관계를 정의한 설계도와 같은 역할을 한다고 볼 수 있다.

이제 N사와 D사에 UserDao를 공급할 때 UserDao, ConnectionMaker와 함께 DaoFactory도 제공한다. DaoFactory는 소스를 제공한 후 ConnectionMaker의 구현 클래스를 변경해서 DaoFactory를 작성할 수 있다.

### 1.4.2 오브젝트 팩토리의 활용

DaoFactory에 UserDao가 아닌 다른 DAO 생성 기능을 넣으면 어떻게 될까 ? AccountDao, MessageDao를 생성하는 메소드를 작성하면 ConnectionMaker 구현체가 메소드마다 중복된단는 것을 알 수 있다. 

ConnectionMaker의 구현 클래스를 결정하고 오브젝트를 만드는 코드를 별도의 메소드로 뽑아내자.

생성 오브젝트 코드 수정
```JAVA
package springbook.project.user.dao;

public class DaoFactory {
    public UserDao userDao() {
        return new UserDao(connectionMaker());
    }

    public ConnectionMaker connectionMaker() {
        return new DConnectionMaker();
    }
}
```

### 1.4.3 제어권의 이전을 통한 제어관계 역전

일반적으로 오브젝트가 능동적으로 자신이 사용할 클래스를 결정하고 언제 어떻게 그 오브젝트를 만들지를 스스로 관장한다. 모든 종류의 작업을 사용하는 쪽에서 제어하는 구조다. 제어의 역전이란 이런 제어의 흐름의 개념을 거꾸로 뒤집는 것이다. 오브젝트가 자신이 사용할 오브젝트를 스스로 선택하지 않는다. 당연히 생성하지도 않는다. 어디서 어떻게 만들어지고 어떻게 구현되어있는지를 알 수 없다. 모든 제어권한을 자신이 아닌 다른 대상에게 위임하기 때문이다. 

**<u>서블릿을 생각해보자. 자바 프로그램은 main() 메소드에서 시작해서 개발자가 미리 정한 순서를 따라 오브젝트가 생성되고 실행된다. 그런데 서블릿을 개발해서 서버가 배포할 수는 있지만 그 실행을 개발자가 직접 제어할 수 있는 방법은 없다. 서블릿 안에 main() 메소드가 있어서 직접 실행시킬 수 있는 것도 아니다. 서블릿에 대한 제어 권한을 가진 컨테이너가 적절한 시점에 서블릿 클래스의 오브젝트를 만들고 그 안의 메소드를 호출한다. 이 역시 제어의 역전개념이 적용되어 있다.</u>**

**<u>템플릿 메소드 패턴, 프레임워크도 여기에 해당 된다. 라이브러리를 사용하는 애플리케이션 코드는 애플리케이션 흐름을 직접 제어한다. 반면에 프레임워크는 거꾸로 애플리케이션 코드가 프레임워크에 의해 사용된다</u>**

## 1.5 스프링

스프링의 핵심을 담당하는 건 바로 빈 팩토리 또는 애플리케이션 컨텍스트라고 불리는 것이다.

### 1.5.1 오브젝트 팩토리를 이용한 스프링 IoC

#### 애플리케이션 컨텍스트와 설정정보 

이제 DaoFactory를 스프링에서 사용이 가능하도록 변신시켜보자.

* 빈(Bean)

   스프링이 제어권을 가지고 직접 만들고 관계를 부여하는 오브젝트를 빈(Bean)이라고 부른다. 스프링 컨테이너가 생성과 관계설정, 사용 등을 제어해주는 제어의 역전이 적용된 오브젝트를 가리키는 말이다.

* 빈 팩토리(Bean Factory)

    빈의 생성과 관계설정 같은 제어를 담당하는 IoC 오브젝트를 빈 팩토리(Bean Factory)라고 부른다. 보통 빈 팩토리보다는 이를 좀 더 확장한 애플리케이션 컨텍스트(ApplicationContext)를 주로 사용한다. 

빈 팩토리라고 말할 때는 빈 팩토리라는 이름대로 빈을 생성하고 관계를 설정하는 IoC의 기본 기능에 초점을 맞춘 것이고 애플리케이션 컨텍스트라고 말할 때는 애플리케이션 전반에 걸쳐 모든 구성요소의 제어 작업을 담당하는 IoC엔진이라는 의미가 좀 더 부각된다고 보면 된다.

#### DaoFactory를 사용하는 애플리케이션 컨텍스트

DaoFactory를 스프링의 빈 팩토리가 사용할 수 있는 본격적인 설정정보를 만들어보자. 스프링이 <u>**빈 팩토리를 위한 오브젝트 설정을 담당하는 클래스라고 인식할 수 있도록 @Configuration이라는 애노테이션**</u>을 추가한다. 그리고 <u>**오브젝트를 만들어주는 메소드에는 @Bean이라는 애노테이션**</u>을 붙여준다. 

이 두가지 애노테이션만으로 스프링 프레임워크는 빈 팩토리 또는 애플리케이션 컨텍스트가 IoC방식의 기능을 제공할 때 사용할 완벽한 설정정보가 된 것이다. 자바 코드의 탈을 쓰고 있지만 사실은 XML과 같은 스프링 전용 설정정보라고 보는 것이 좋다.

스프링 빈 팩토리가 사용할 설정정보를 담은 DaoFactory 클래스
```JAVA
package springbook.project.user.dao;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class DaoFactory {
    @Bean
    public UserDao userDao() {
        return new UserDao(connectionMaker());
    }

    @Bean
    public ConnectionMaker connectionMaker() {
        return new DConnectionMaker();
    }
}
```

DaoFactory를 설정정보로 사용하는 애플리케이션 컨텍스트를 만들어보자. 애플리케이션 컨텍스트는 ApplicationContext 타입의 오브젝트다. ApplicationContext를 구현한 클래스는 여러 가지가 있는데 DaoFactory 처럼 @Configuration이 붙은 자바코드를 설정정보롤 사용하려면 AnnotationConfigApplicationContext를 이용하면 된다.

애플리케이션 컨텍스트를 적용한 UserDaoTest
```Java
package springbook.project.user.dao;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import springbook.project.user.domain.User;

import java.sql.SQLException;

public class UserDaoTest {
    public static void main(String[] args) throws SQLException, ClassNotFoundException {
        ApplicationContext applicationCointext = new AnnotationConfigApplicationContext(DaoFactory.class);

        UserDao dao = applicationCointext.getBean("userDao", UserDao.class);

        User user = new User();
        user.setId("whiteship");
        user.setName("백기선");
        user.setPassword("married");

        dao.delete();

        dao.add(user);

        System.out.println(user.getId() + " 등록 성공");

        User user2 = dao.get(user.getId());
        System.out.println(user2.getName());
        System.out.println(user2.getPassword());

        System.out.println(user.getId() + " 조회 성공");
    }
}
```

### 1.5.2 애플리케이션 컨텍스트의 동작방식

스프링에서는 이 애플리케이션 컨텍스트를 IoC컨테이너, 스프링 컨테이너, 빈 팩토리라고 부른다. 애플리케이션 컨텍스트는 ApplicationContext 인터페이스를 구현하는데 ApplicationContext는 빈 팩토리가 구현하는 BeanFactory 인터페이스를 상속했으므로 애플리케이션 컨텍스트는 일종의 빈 팩토리인 셈이다. 애플리케이션 컨텍스트는 애플리케이션에서 IoC를 적용해서 관리할 모든 오브젝트에 대한 생성과 관계설정을 담당한다. 그리고 생성정보와 연관관계 정보를 별도의 설정정보를 통해 얻는다.

### 1.5.3 스프링 IoC 용어 정리

* Bean 빈

빈 또는 빈 오브젝트는 스프링이 IoC방식으로 관리하는 오브젝트라는 뜻이다. 애플리케이션에서 만들어지는 모든 오브젝트가 빈은 아니다. 그중에서 스프링이 직접 생성과 제어를 담당하는 오브젝트만을 빈이라고 부른다.

* BeanFactory 빈 팩토리

IoC를 담당하는 핵심 컨테이너를 말한다. 빈을 등록하고 생성하고 조회하고 돌려주고 그 외에 부가적인 빈을 관리하는 기능을 담당한다. 보통은 이 빈팩토리를 바로 사용하지 않고 이를 확장한 애플리케이션 컨텍스트를 이용한다. 

* ApplicationContext 애플리케이션 컨텍스트

빈 팩토리를 확장한 IoC컨테이너다. 빈 팩토리라고 부를 때는 빈 생성과 제어의 관점에서 이야기하는 것이고 애플리케이션 컨텍스트라고 할 때는 스프링이 제공하는 애플리케이션 지원 기능을 모두 포함해서 이야기하는 것이라고 보면 된다.

* 설정정보/설정 메타정보 

스프링 설정정보란 애플리케이션 컨텍스트 또는 빈 팩토리가 IoC를 적용하기 위해 사용하는 메타정보를 말한다. 

* Container 컨테이너 또는 IoC 컨테이너

IoC 방식으로 빈을 관리한다는 의미에서 애플리케이션 컨텍스트나 빈 팩토리를 컨테이너 또는 IoC컨테이너라고도 한다. 

* 스프링 프레임워크

개발자가 환경, 기술에 종속되지 않고 객체지향 프로그래밍으로 애플리케이션 개발을 기능을 지원해주는 프레임워크라고 할 수 있다. 

# 1.6 싱글톤 레지스트리와 오브젝트 스코프

스프링의 애플리케이션 컨텍스트는 기존에 직접 만들었던 오브젝트 팩토리와는 중요한 차이점이 있다. 

직접 생성한 DaoFactory 오브젝트 출력 코드
```JAVA
    DaoFactory factory = new DaoFactory();
    UserDao dao1 = factory.userDao();
    UserDao dao2 = factroyuserDao();

    System.out.println(dao1);
    System.out.println(dao2);
```

다음과 같은 결과가 나온다.

```Consol
springbook.dao.UserDao@118f375
springbook.dao.UserDao@117a8bd
```

출력 결과에서 알 수 있듯이 두 개는 각기 다른 값을 가진 동일하지 않은 오브젝트다.

스프링 컨텍스트로부터 가져온 오브젝트 출력코드

```JAVA
ApplicationContext applicationCointext = new AnnotationConfigApplicationContext(DaoFactory.class);

UserDao dao3 = applicationCointext.getBean("userDao", UserDao.class);
UserDao dao4 = applicationCointext.getBean("userDao", UserDao.class);

System.out.println(dao3);
System.out.println(dao4);
```

```Consol
springbook.dao.UserDao@113f3z3
springbook.dao.UserDao@113f3z3
```

getBean()을 두 번 호출해서 가져온 오브젝트가 동일하다는 사실을 알 수 있다. 스프링은 여러 번에 걸쳐 빈을 요청하더라도 매번 동일한 오브젝트를 돌려준다.

### 1.6.1 싱글톤 레지스트리로서의 애플리케이션 컨텍스트

애플리케이션 컨텍스트는 싱글톤을 저장하고 관리하는 싱글톤 레지스트리(Singleton Registry)이기도 하다. 스프링은 내부에서 생성하는 빈 오브젝트를 특별한 설정을 하지 않으면 모두 싱글톤으로 만든다.

#### 서버 애플리케이션과 싱글톤

왜 스프링은 싱글톤으로 빈을 만드는 것일까? 스프링이 주로 적용되는 대상이 자바 엔터프라이즈 기술을 사용하는 서버환경이기 때문이다. 스프링은 엔터프라이즈 시스템을 위해 고안된 기술이기 때문에 서버환경에서 사용될 때 그 가치가 있다. 매번 클라이언트에서 요청이 올 때마다 각 로직을 담당하는 오브젝트를 새로 만들어서 사용한다고 생각해보자. 부하가 걸리며 서버가 감당하기 힘들 것이다.

그래서 엔터프라이즈 분야에서는 서비스 오브젝트라는 개념을 일찍부터 사용해왔다. 서블릿은 자바 엔터프라이즈 기술의 가장 기본이 되는 서비스 오브젝트라고 할 수 있다. <u>**서블릿은 대부분 멀티스레드 환경에서 싱글톤으로 동작한다. 서블릿 클래스당 하나의 오브젝트만 만들어두고 사용자의 요청을 담당하는 여러 스레드에서 하나의 오브젝트를 공유해 동시에 사용한다.**</u> 

* 싱글톤 패턴(Singleton Pattern)

싱글톤 패턴은 어떤 클래스를 애플리케이션 내에서 제한된 인스턴스 개수, 이름처럼 주로 하나만 존재하도록 강제하는 패턴이다. 하나만 만들어지는 클래스의 오브젝트는 애플리케이션 내에서 전역적으로 접근이 가능하다. 단일 오브젝트만 존재해야 하고 이를 애플리케이션의 여러 곳에서 공유하는 경우에 주로 사용한다.

### 싱글톤 패턴의 한계

자바에서 싱글톤을 구현하는 방법은 보통 이렇다.

1. 클래스 밖에서는 오브젝트를 생성하지 못하도록 <u>생성자를 private</u>으로 만든다.

2. 생성된 싱글톤 오브젝트를 저장할 수 있는 자신과 <u>같은 타입의 스태틱 필드</u>를 정의하다.

3. 스태틱 팩토리 메소드인 getInstance()를 만들고 이 메소드가 최초로 호출되는 시점에서 한번만 오브젝트가 만들어지게 한다. 생성된 오브젝트는 스태틱 필드에 저장된다. 또는 스태틱 필드의 초기값으로 오브제트를 미리 만들어둘 수도 있다.

4. 한번 오브젝트(싱글톤)가 만들어지고 난 후에는 getInstance() 메소드를 통해 이미 만들어져 스태틱 필드에 저장해둔 오브젝트를 넘겨준다.

```JAVA
public class UserDao {
    private static UserDao INSTANCE;

    private UserDao(ConnectionMaker connectionMaker) {
        this.connectionMaker = connectionMaker;
    }
    public static synchronized UserDao getInstance() {
        if (INSTANCE == null) INSTANCE = new UserDao(???);
        return INSTANCE;
    }
}
```
싱글톤 패턴 구현 방식에는 다음과 같은 문제가 있다.

1. private 생성자를 갖고 있기 때문에 상속할 수 없다.

    private생성자를 가진 클래스는 상속이 불가능하다. 객체지향의 장점인 상속과 이를 이용한 다형성을 적용할 수 없다. 

2. 싱글톤은 테스트하기가 힘들다.

    싱글톤을 만들어지는 방식이 제한적이기 때문에 테스트에서 사용될 때 목오브젝트 등 테스트용 오브젝트로 대체하기가 힘들다.

3. 서버환경에서는 싱글톤이 하나만 만들어지는 것을 보장하지 못한다.

4. 싱글톤의 사용은 전역상태를 만들 수 있기 때문에 바람직하지 못하다.

    아무 객체나 자유롭게 접근하고 수정하고 공유할 수 있는 전역 상태를 갖는 것은 객체지향 프로그래밍에서는 권장되지않는 프로그래밍 모델이다.

#### 싱글톤 레지스트리

스프링은 직접 싱글톤 형태의 오브젝트를 만들고 관리하는 기능을 제공한다. 그것이 바로 싱글톤 레지스트리다. 스프링 컨테이너는 싱글톤을 생성하고 관리하고 공급하는 싱글톤 관리 컨테이너이기도 하다. 싱글톤 레지스트리의 장점은 스태틱 메소드와 private 생성자를 사용해야 하는 비정상적인 클래스가 아니라 평범한 자바 클래스를 싱글톤으로 활용하게 해준다는 점이다. 

스프링의 싱글톤 레지스트리 덕분에 싱글톤 방식으로 사용될 애플리케이션 클래스라도 public 생성자를 가질 수 있고 싱글톤으로 사용돼야 하는 환경이 아니라면 간단히 오브젝트를 생성해서 사용할 수 있다. 따라서 테스트 환경에서 자유롭게 오브젝트를 만들 수 있고 테스트를 위한 목 오브젝트로 대체하는 것도 간단하다.

싱글톤 패턴과 달리 스프링이 지지하는 객체지향적인 설계 방식과 원칙 디자인 패턴 등을 적용하는데 아무런 제약이 없다는 점이다.


### 1.6.2 싱글톤과 오브젝트의 상태

싱글톤은 멀티스레드 환경이라면 여러 스레드가 동시에 접근해서 사용할 수 있다. 싱글톤이 멀티스레드 환경에서 서비스 형태의 오브젝트로 사용되는 경우에는 상태정보를 내부에 갖고 있지 않은 무상태(Stateless)방식으로 만들어져야 한다. 다중 사용자의 요청을 한꺼번에 처리하는 스레드들이 동시에 싱글톤 오브젝트의 인스턴스 변수를 수정하는 것은 매우 위험하다.

상태가 없는 방식으로 클래스를 만드는 경우에는 각 요청에 대한 정보나 DB나 서버의 리소스로부터 생성한 정보는 어떻게 다뤄야 할까? 파라미터와 로컬 변수 리턴 값을 활용하면 된다. 메소드 안에서 생성되는 로컬 변수는 매번 새로운 값을 저장할 독립적인 공간이 만들어지기 때문에 싱글톤이라고 해도 여러 스레드가 변수의 값을 덮어쓸일은 없다.

### 1.6.3 스프링빈의 스코프

* 빈의 스코프(Scope)

스프링이 관리하는 오브젝트, 빈이 생성되고 존재하고 적용되는 범위를 Scope라고 한다. 기본 스코프는 싱글톤이다. 프로토타입 Scope은 컨테이너 빈을 요청할 때마다 매번 새로운 오브젝트를 만들어준다. 그 외에도 요청 스코프가 있고 세션 스코프도 존재한다.

## 1.7 의존관계 주입(DI)

### 1.7.1 제어의 역전(IoC)과 의존관계 주입

### 1.7.2 런타임 의존관계 설정

#### 의존관계

    의존한다는 건 의존 대상이 변하면 의존하는 오브젝트도 영향을 미친다는 뜻이다. 의존관계는 방향성이 있다. 

#### UserDao의 의존관계

UserDao가 ConnectionMaker에 의존하고 있는 형태이다. UserDao는 ConnectionMaker인터페이스에 의존하고 있다. ConnectionMaker 인터페이스가 변한다면 그 영향을 UserDao가 직접적으로 받게 된다. 하지만 ConnectionMaker 인터페이스를 구현한 클래스 DConnectionMaker 등이 다른 것으로 바뀌거나 그 내부에서 사용하는 메소드에 변화가 생겨도 UserDao에 영향을 주지 않는다. 

**<u>인터페이스에 대해서만 의존관계를 만들어두면 인터페이스 구현 클래스와의 관계는 느슨해지면서 변화에 영향을 덜 받는 상태가 된다. 결합도가 낮다고 설명할 수 있다. 의존관계란 한쪽의 변화가 다른 쪽에 영향을 주는 것이라고 했으니 인터페이스를 통해 의존관계를 제한해주면 그만큼 변경에서 자유로워지는 셈이다.</u>**

런타임시에 오브젝트 사이에서 만들어지는 의존관계도 있다. 런타임 의존관계 또는 오브젝트 의존관계인데, 설계 시점의 의존관계가 실체화된 것이라고 볼 수 있다. 오브젝트가 만들어지고 나서 런타임 시에 의존관계를 맺는 대상, 실제 사용대상인 오브젝트를 의존 오브젝트라고 말한다. 

* 의존관계 주입

  다음 3가지 조건을 충족하는 작업을 의미한다. 

  1. 클래스 모델이나 코드에는 런타임 시전의 의존관계가 드러나지 않는다. 그러기 위해서는 인터페이스에만 의존하고 있어야 한다.
  2. 런타임 시점의 의존관계는 컨테이너나 팩토리 같은 제3의 존재가 결정한다.
  3. 의존관계는 사용할 오브젝트에 대한 레퍼런스를 외부에서 제공(주입)해줌으로서 만들어진다.
  
의존관계 주입의 핵심은 설계 시점에는 알지 못했던 두 오브젝트의 관계를 맺도록 도와주는 제3의 존재가 있다는 것이다. 스프링의 애플리케이션 컨텍스트, 빈팩토리, IoC 컨테이너 등이 모두 외부에서 오브젝트 사이의 런타임 관계를 맺어주는 책임을 지닌 제3의 존재라고 볼 수 있다.

#### UserDao의 의존관계 주입

DaoFactory는 런타임 시점에 UserDao가 사용할 ConnectionMaker 타입의 오브젝트를 결정하고 이를 생성한 후에 UserDao의 생성자 파라미터로 주입해서 UserDao가 DConnectionMaker의 오브젝트와 런타임 의존관계를 맺게 해준다. 

DaoFactory는 여기서 두 오브젝트 사이의 런타임 의존관계를 설정해주는 의존관계 주입 작업을 주도하는 존재이며, 동시에 IoC방식으로 오브젝트의 생성과 초기화 제공 등의 작업을 수행하는 컨테이너다.

DI는 자신이 사용할 오브젝트에 대한 선택과 생성 제어권을 외부로 넘기고 자신은 수동적으로 주입받은 오브젝트를 사용한다는 점에서 IoC개념과 잘 들어맞는다.

1.7.3 의존관계 검색과 주입

의존관계를 맺는 방법이 외부로부터의 주입이 아니라 스스로 검색을 이용하기 때문에 의존관계 검색(dependency lokkup)이라고 불리는 개념도 있다. 의존관계 검색은 런타임 시 의존관계를 맺을 오브젝트를 결정하는 것과 오브젝트 생성 작업은 외부 컨테이너에게 IoC로 맡기지만 이를 가져올 때는 메소드나 생성자를 통한 주입 대신 스스로 컨테이너에게 요청하는 방법을 사용한다.

```Java
public UserDao() {
    DaoFactory daoFactory = new DaoFactory();
    this.connectionMaker = daoFactory.connectionMaker(); //요청
}
```

이렇게 해도 UserDao는 여전히 자신이 어떤 ConnectionMaker 오브젝트를 사용할지 미리 알지 못한다. 런타임 시에 DaoFactory가 만들어서 돌려주는 오브젝트와 다이내믹하게 런타임 의존관계를 맺는다. 하지만 요청 방법은 외부로부터의 주입이 아니라 스스로 IoC 컨테이너인 DaoFactory에게 요청하는 것이다. 스프링의 IoC 컨테이너인 애플리케이션 컨텍스트는 getBean()이라는 메소드를 제공한다. 이 메소드가 의존관계 검색에 사용되는 것이다. 

의존관계 검색을 이용하는 UserDao 생성자
```Java
public UserDao() {
    AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(DaoFactory.class);
    this.connectionMaker = context.getBean("connectionMaker", ConnectionMaker.class);
}
```

의존관계보다는 의존관계 주입이 단순하고 깔끔하다. 하지만 의존관계 검색 방식을 사용해야 할 때가 있다. 애플리케이션 기동시점에 적어도 한번은 의존관계 검색 방식을 사용해 오브젝트를 가져와야 한다. 스태틱 메소드인 main()에서는 DI를 이용해 오브젝트를 주입받을 방법이 없기 때문이다. 서버에서도 마찬가지이다. 서블릿에서 스프링 컨테이너에 담긴 오브젝트를 사용하려면 한 번은 의존관계 검색 방식을 사용해 오브젝트를 가져와야 한다. 다행히 이런 서블릿은 미리 만들어서 스프링이 제공하기 때문이 직접 구현할 필요는 없다. 

의존관계 검색(DL)과 의존관계 주입(DI)은 중요한 차이점이 있다. 의존관계 검색 방식에서는 검색하는 오브젝트는 자신이 스프링의 빈일 필요가 없다는 점이다. 반면에 의존관계 주입은 자기 자신이 컨테이너 관리하는 빈이 돼야 한다.

### 1.7.4 의존관계 주입의 응용
#### 기능 구현의 교환
#### 부가기능 추가

연결횟수 카운팅 기능이 있는 클래스
```JAVA
package springbook.project.user.dao;

import java.sql.Connection;
import java.sql.SQLException;

public class CountingConnectionMaker implements ConnectionMaker {
    int count = 0;
    private ConnectionMaker realConnectionMaker;

    public CountingConnectionMaker(ConnectionMaker realConnectionMaker) {
        this.realConnectionMaker = realConnectionMaker;
    }

    @Override
    public Connection makeConnection() throws ClassNotFoundException, SQLException {
        this.count++;
        return realConnectionMaker.makeConnection();
    }

    public int getCount() {
        return count;
    }
}
```

CoutingConnectionMaker 의존관계가 추가된 DI 설정용 클래스
```JAVA
package springbook.project.user.dao;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class CountingDaoFactory {
    @Bean
    public UserDao userDao() {
        return new UserDao(connectionMaker());
    }

    @Bean
    public ConnectionMaker connectionMaker() {
        return new CountingConnectionMaker(realConnectionMaker());
    }

    @Bean
    public ConnectionMaker realConnectionMaker() {
        return new DConnectionMaker();
    }
}
```

CountingConnectionMaker에 대한 테스트 클래스
```JAVA
package springbook.project.user.dao;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import springbook.project.user.domain.User;

import java.sql.SQLException;

public class UserDaoCoinnectionCountingTest {
    public static void main(String[] args) throws SQLException, ClassNotFoundException {
        ApplicationContext context = new AnnotationConfigApplicationContext(CountingDaoFactory.class);
        UserDao userDao = context.getBean("userDao", UserDao.class);

        User user = new User();
        user.setId("whiteship");
        user.setName("백기선");
        user.setPassword("married");

        userDao.delete();
        userDao.add(user);
        userDao.get(user.getId());

        CountingConnectionMaker ccm = context.getBean("connectionMaker", CountingConnectionMaker.class);
        System.out.println("Connetion counter : " + ccm.getCount());
    }
}
```

1.7.5 메소드를 이용한 의존관계 주입

스프링은 전통적으로 메소드를 이용한 DI 방법 중에서 수정자 메소드를 가장 많이 사용해왔다. 뒤에서 보겠지만 DaoFactory 같은 자바 코드 대신 XML을 사용하는 경우에는 자바빈 규약을 따르는 수정자 메소드가 가장 사용하기 편리하다.

수정자 메소드 DI 방식을 사용한 UserDao (생성자 제거)
```JAVA
package springbook.project.user.dao;

import springbook.project.user.domain.User;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class UserDao {

    private ConnectionMaker connectionMaker;

    public void setConnectionMaker(ConnectionMaker connectionMaker) {
        this.connectionMaker = connectionMaker;
    }

    public void add(User user) throws ClassNotFoundException, SQLException {
        Connection c = connectionMaker.makeConnection();

        PreparedStatement ps = c.prepareStatement("insert into users(id,name,password) values(?,?,?)");
        ps.setString(1, user.getId());
        ps.setString(2, user.getName());
        ps.setString(3, user.getPassword());

        ps.executeUpdate();
        c.close();
    }

    public User get(String id) throws ClassNotFoundException, SQLException {
        Connection c = connectionMaker.makeConnection();

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

    public void delete() throws ClassNotFoundException, SQLException {
        Connection c = connectionMaker.makeConnection();

        PreparedStatement ps = c.prepareStatement("DELETE FROM users");
        ps.executeUpdate();

        ps.close();
        c.close();
    }

}
```

수정자 메소드 DI를 사용하는 팩토리 메소드
```JAVA
package springbook.project.user.dao;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class DaoFactory {
    @Bean
    public UserDao userDao() {
        UserDao userDao = new UserDao();
        ConnectionMaker cm = connectionMaker();
        userDao.setConnectionMaker(cm);
        return userDao;
    }

    @Bean
    public ConnectionMaker connectionMaker() {
        return new DConnectionMaker();
    }
}
```

## 1.8 XML을 이용한 설정

스프링은 DaoFactory와 같은 자바 클래스를 이용하는 것 외에도 다양한 방법을 통해 DI 의존관계 설정정보를 만들 수 있다. 가장 대표적인 것이 바로 XML이다.

XML은 단순 테스트 파일이기 때문에 다루기 쉽다 . 또 명시적이고 컴파일과 같은 별도의 빌드 작업 작업이 없다는 것도 장점이다.

### 1.8.1 XML 설정

    빈의 이름 : @Bean 메소드 이름이 빈의 이름이다. 
    빈의 클래스 : 빈 오브젝트를 어떤 클래스를 이용해서 만들지를 정의한다.
    빈의 의존 오브젝트 : 빈의 생성자나 수정자 메소드를 통해 의존 오브젝트를 넣어준다.

#### connectionMaker() 전환

클래스 설정과 XML 설정의 대응항목
|---|자바 코드 설정정보|XML 설정정보|
|---|---|---|
|빈 설정파일|@Configuration|<beans>|
|빈의 이름|@Bean methodName()|<bean id="methodName">|
|빈의 클래스|return new BeanClass();|class="a.b.c...BeanClass">|

#### UserDao의 전환

UserDao() 에는 DI정보의 세 가지 요소가 모두 들어있다. 여기서 관심을 가질 것은 수정자 메소드를 사용해 의존관계를 주입해주는 부분이다. 스프링 개발자가 수정자 메소드를 선호하는 이유 중에는 XML로 의존관계 정보를 만들 때 편리하다는 점도 있다. 자바빈의 관례를 따라서 수정자 메소드는 프로퍼티가 된다.

XML에서는 <property> 태그를 사용해 의존 오브젝트와의 관계를 정의한다. <property> 태그는 name과 ref라는 두 개의 애트리뷰트를 갖는다. name은 프로퍼티의 이름이다. 이 프로퍼티 이름으로 수정자 메소드를 알 수 있다. ref는 수정자 메소드를 통해 주입해줄 오브젝트의 빈 이름이다. DI할 오브젝트도 역시 빈이다.

### 1.8.2 XML을 이용하는 애플리케이션 컨텍스트

XML 설정 정보를 담은 applicationContext.xml
```XML
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
        <bean id="connectionMaker" class="springbook.project.user.dao.DConnectionMaker"/>
        <bean id="userDao" class="springbook.project.user.dao.UserDao">
            <property name="connectionMaker" ref="connectionMaker"/>
        </bean>
</beans>
```

XML에서 빈의 의존관계 정보를 이용하는 IoC/DI 작업에는 GenericXmlApplicationContext를 사용한다.

애플리케이션 컨텍스트가 사용하는 XML설정파일의 이름은 관례를 따라 applicationContext.xml이라고 만든다.

GenericXmlApplicationContext의 생성자에는 applicationContext.xml의 클래스패스를 넣는다. 클래스패스를 시작하는 /는 넣을 수도 있고 생략할 수도 있다. /가 없는 경우에도 항상 루트에서부터 시작하는 클래스 패스라는 점을 기억해두자.

```JAVa
ApplicationContext context = new GenericXmlApplicationContext("applicationContext.xml");
```

GenericXmlApplicationContext 외에도 ClassPathXmlApplicationContext를 이용해 XML로부터 설정정보를 가져올 수 있다.

GenericXmlApplicationContext는 클래스패스뿐 아니라 다양한 소스로부터 설정파일을 읽어올 수 있다. 

ClassPathXmlApplicationContext는 XMl 파일을 클래스패스에서 읽어올 때, XML 파일과 같은 클래스패스에 있는 클래스 오브젝트를 넘겨서 클래스패스에 대한 힌트를 제공할 수 있다.

1.8.3 DataSource 인터페이스로 변환

* DataSource 인터페이스

자바에서 DB커넥션을 가져오는 오브젝트의 기능을 추상화해서 비슷한 용도로 사용할 수 있게 만들어진 DataSource라는 인터페이스가 이미 존재한다. DataSource를 구현해서 DB커넥션을 제공하는 클래스를 만들 일은 거의 없다. 이미 다양한 DB연결과 풀링(Pooling) 기능을 갖춘 많은 DataSource 구현 클래스가 존재한다. 대부분의 DataSource 구현 클래스는 DB의 종류나 아이디, 비밀번호 정도는 DataSource 구현 클래스를 다시 만들지 않고도 지정할 수 있는 방법을 제공한다. DataSource 인터페이스 실제로 관심을 가질 것은 getConnection() 메소드 하나뿐이다. DataSource 구현 클래스가 필요하다. SimpleDriverDataSource라는 것이 있다. 

DataSource 타입의 dataSource 빈 정의 메소드
```JAVA
    @Bean
    public DataSource dataSource() {
        SimpleDriverDataSource dataSource = new SimpleDriverDataSource();
        dataSource.setDriverClass(org.mariadb.jdbc.Driver.class);
        dataSource.setUrl("jdbc:mariadb://localhost/tobechap01");
        dataSource.setUsername("bootuser");
        dataSource.setPassword("bootuser");
    }
```

DataSource로 사용하는 UserDao
```JAVA
package springbook.project.user.dao;

import springbook.project.user.domain.User;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class UserDao {
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

    public void delete() throws SQLException {
        Connection c = dataSource.getConnection();

        PreparedStatement ps = c.prepareStatement("DELETE FROM users");
        ps.executeUpdate();

        ps.close();
        c.close();
    }
}
```

Datasource를 적용 완료한 applicationContext.xml
```XML
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
<!--        <bean id="connectionMaker" class="springbook.project.user.dao.DConnectionMaker"/>-->
        <bean id="userDao" class="springbook.project.user.dao.UserDao">
            <property name="dataSource" ref="dataSource"/>
        </bean>
        <bean id="dataSource" class="org.springframework.jdbc.datasource.SimpleDriverDataSource">
            <property name="driverClass" value="org.mariadb.jdbc.Driver"/>
            <property name="url" value="jdbc:mariadb://localhost/tobechap01"/>
            <property name="username" value="bootuser"/>
            <property name="password" value="bootuser"/>
        </bean>
</beans>
```

UserDaoTest
```Java
package springbook.project.user.dao;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;
import springbook.project.user.domain.User;

import java.sql.SQLException;

public class UserDaoTest {
    public static void main(String[] args) throws SQLException, ClassNotFoundException {
//        ApplicationContext applicationCointext = new AnnotationConfigApplicationContext(DaoFactory.class);
//        UserDao dao = applicationCointext.getBean("userDao", UserDao.class);
        ApplicationContext applicationContext = new GenericXmlApplicationContext("applicationContext.xml");
        UserDao dao = (UserDao) applicationContext.getBean("userDao");

        User user = new User();
        user.setId("whiteship");
        user.setName("백기선");
        user.setPassword("married");

        dao.delete();

        dao.add(user);

        System.out.println(user.getId() + " 등록 성공");

        User user2 = dao.get(user.getId());
        System.out.println(user2.getName());
        System.out.println(user2.getPassword());

        System.out.println(user.getId() + " 조회 성공");
    }
}
```

#### Value 값의 자동변환

한가지 이상한 것이 있다. driverClass는 스트링 타입이 아니라 java.lang.Class 타입이다. 자바코드에서는 Driver.class라는 Class 타입 오브젝트를 전달한다. 그런데 XML에서는 별다른 타입정보 없이 클래스의 이름이 텍스트 형태로 value에 들어가 있다. 이런 설정이 가능한 이유는 스프링이 프로퍼티 값을 수정자 메소드의 파라미터 타입을 참고 해서 적절한 형태로 변환해주기 때문이다.


  













