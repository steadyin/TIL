자바에서 표준 스펙, 상용 제품, 오픈 소스를 통틀어서 사용 방법과 형식은 다르지만 기능과 목적이 유사한 기술이 존재한다.
5장에서는 DAO에 트랜잭션을 적용해보면서 스프링이 어떻게 성격이 비슷한 여러 종류의 기술을 추상화하고 이를 일관된 방법으로 사용할 수 있도록 지원하는 지를 살펴볼 것 이다.

# 5.1 사용자 레벨의 관리 기능 추가

## 5.1.1 필드 추가

### Level 이늄

User클래스에 사용자의 레벨을 저장할 필드를 추가하자. DB에서 'SILVER' 문자열을 직접 넣는 방법 보다는 레벨을 코드화해서 숫자로 넣는 것이 좋아보인다. 그럼 자바에서 User에 추가할 프로퍼티 타입도 숫자로 하면 될까 ? 의미 없는 숫자를 프로퍼티에 사용하면 타입이 안전하지 않아서 위험할 수 있다. 

```JAVA
class User {
  private static final int BASIC = 1;
  private static final int SILVER = 2;
  private static final int GOLD = 3;

  public void setLevel(int level) {
    this.level = level;
  }
}
```

사용자 레벨을 정수형 상수 값으로 정의하면 아래처러 깔끔하게 소스코드를 작성할 수 있긴 하다. 

```JAVA
if (user1.getLevel() == User.BASIC) {
  user1.setLevel(user>SILVER);
}
```

문제는 level의 타입이 int이기 때문에 임의의 숫자를 실수를 컴파일러가 체크해주지 못한다는 점이다. 

그래서 숫자 타입을 직접 사용하는 것보다는 자바 5 이상에서 제공하는 `이늄`(enum)을 이용하는게 안전하고 편리하다.

```JAVA
public enum Level {
  BASIC(1), SILVER(2), GOLD(3); // 3개의 이눔 오브젝트 정의

  private final int value;

  Level(int value) { // DB에 저장할 값을 넣어줄 생성자를 만들어둔다.
    this.value = value;
  }

  public int intValue() { //값을 가져오는 메소드
    return value;
  }

  public static Level valueOf(int value) { // 값으로부터 Level 타입 오브젝트를 가져오도록 만든 스태틱 메소드
    switch(value) {
      case 1 : return BASIC;
      case 2 : return SILVER;
      case 3 : return GOLD;
      default : throw new AssertionError("Unknown value : " + value);
    }
  }
}
```

Level 이늄은 내부에는 DB에 저장할 int타입의 값을 갖고 있지만 겉으로는 Level타입의 오브젝트 이기 때문에 안전하게 사용할 수 있다.


### User 필드 추가

Level 타입의 변수를 User 클래스에 추가하고 앞에서 언급한 로그인 횟수, 추천수도 추가하자.

```JAVA
public class User {
    String id;
    String name;
    String password;

    Level level;
    int login;
    int recommend;

    public User() {}

    public User(String id, String name, String password, Level level, int login, int recommend) {
        this.id = id;
        this.name = name;
        this.password = password;
        this.level = level;
        this.login = login;
        this.recommend = recommend;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() { return name; }

    public void setName(String name) {
        this.name = name;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public Level getLevel() {
        return level;
    }

    public void setLevel(Level level) {
        this.level = level;
    }

    public int getLogin() {
        return login;
    }

    public void setLogin(int login) {
        this.login = login;
    }

    public int getRecommend() {
        return recommend;
    }

    public void setRecommend(int recommend) {
        this.recommend = recommend;
    }
}
```

DB의 USER테이블에도 필드를 추가한다.

```Mysql
alter table users add column level tinyint Not Null
alter table users add column login int Not Null
alter table users add column recommend int Not Null
```

### UserDaoTest 테스트 수정

UserDaoJDBC와 테스트에도 필드를 추가해야한다. 

```JAVA
public class UserDaoTest {
...생략...
    @Before
    public void setUp() {
        user1 = new User("gyumee", "박성철", "springno1", Level.BASIC, 1, 0);
        user2 = new User("leegw700", "이길원", "springno2", Level.SILVER, 55, 10);
        user3 = new User("bumjin", "박범진", "springno3", Level.GOLD, 100, 40);
    }

    private void checkSameUser(User user1, User user2) {
        assertThat(user1.getId(), is(user2.getId()));
        assertThat(user1.getName(), is(user2.getName()));
        assertThat(user1.getPassword(), is(user2.getPassword()));
        assertThat(user1.getLevel(), is(user2.getLevel()));
        assertThat(user1.getLogin(), is(user2.getLogin()));
        assertThat(user1.getRecommend(), is(user2.getRecommend()));
    }
...생략...
}
```
### UserDaoJdbc 수정

UserDaoJdbc에 포함 된 SQL에 추가된 컬럼을 반영하고 userMapper에 추가된 필드를 넣는다.
```JAVA
public class UserDao {
...

    private RowMapper<User> userRowMapper =
            new RowMapper<User>() {
                @Override
                public User mapRow(ResultSet resultSet, int i) throws SQLException {
                    User user = new User();
                    user.setId(resultSet.getString("id"));
                    user.setName(resultSet.getString("name"));
                    user.setPassword(resultSet.getString("password"));
                    user.setLevel(Level.valueOf(resultSet.getInt(("level"))));
                    user.setLogin(resultSet.getInt("login"));
                    user.setRecommend(resultSet.getInt("recommend"));
                    return user;
                }
            };
...
    public void add(final User user) {
        jdbcTemplate.update("insert into users(id, name, password, level, login, recommend) value(?,?,?)", user.getId(), user.getName(), user.getPassword(), user.getLevel().intValue(), user.getLogin(), user.getRecommend(), user.getId());
    }
}
```

눈 여겨볼 것은 Level 타입의 level 필드를 사용하는 부분이다. Level 이늄은 오브젝트이며 DB에 저장할 수 있는 SQL타입이 아니다.
따라서 DB에 저장할 때는 intValue() 메소드를 사용한다. 

반대로 조회를 했을 경우 ResultSet에서는 DB에서 int타입으로 가져와서 Level의 Static 메소드인 valueOf를 이용해서 setLevel에 넣어주어야 한다.

JDBC가 사용하는 SQL은 컴파일 과정에서 자동으로 검증하지 못한다.  따라서 빠르게 실행 가능한 포괄적인 테스트를 만들어두면 이렇게 기능의 추가나 수정이 일어날 때 그 위력이 발휘한다. 

## 5.1.2 사용자 수정 기능 추가

수정할 정보가 담긴 User 오브젝트를 전달하면 id를 참고해서 사용자를 찾아 필드 정보를 UPDATE 문을 이용해 모두 변경해주는 메소드를 하나 만들겠다.

### 수정 기능 테스트 추가

먼저 테스트를 작성하자. 
```JAVA
    @Test
    public void update() {
        dao.deleteAll();

        dao.add(user1);

        user1.setName("오민규");
        user1.setPassword("springno6");
        user1.setLevel(Level.GOLD);
        user1.setLogin(1000);
        user1.setRecommend(999);
        dao.update(user1);

        User user1update = dao.get(user1.getId());
    }
```

그런데 user1이라는 텍스트 픽스처는 인스턴스 변수로 만들어놓은 것인데 이를 직접 변경해도 될까? 상관없다. 테스트 메소드가 실행될 때마다 UserDaoTest 오브젝트는 새로 만들어지고 setUp() 메소드도 다시 불려서 초기화될 테니 내용을 변경해도 상관없다.

### UserDao와 UserDaoJdbc 수정

UserDao.update() 메소드를 추가해보자.
```JAVA
    @Override
    public void update(final User user) {
        jdbcTemplate.update("update users set id = ?, name = ?, password = ?, level = ?, login = ?, recommend = ? where id = ?", user.getId(), user.getName(), user.getPassword(), user.getLevel().intValue(), user.getLogin(), user.getRecommend(), user.getId());
    }
```
### 수정 테스트 보완

테스트는 정상적으로 동작한다. JDBC 개발에서 리소스 반환과 같은 기본 작업을 제외하면 가장 많은 실수가 일어나는 곳은 바로 SQL 문장이다.

우리가 작성한 테스트로는 UPDATE 문장에 WHERE이 빠진 경우 검증하지 못하고 정상동작 하는 것처럼 보인다. 

이 문제를 해결할 방법을 생각해보자.

첫번째 방법은 JdbcTemplate의 update()가 돌려주는 리턴 값을 확인하는 것이다. JdbcTemplate은 테이블의 내용에 영향을 주는 SQL을 실행하면 영향받은 로우의 개수를 돌려준다.

두번째 방법은 테스트를 보강해서 원하는 사용자 외의 정보는 변경되지 않았음을 직접 확인하는 것이다.

여기서는 두번째 방법을 적용해보자.

``` 
    @Test
    public void update() throws SQLException {
        dao.deleteAll();

        dao.add(user1);  //수정할 사용자
        dao.add(user2);  //수정하지 않을 사용자

        user1.setName("오민규");
        user1.setPassword("springno6");
        user1.setLevel(Level.GOLD);
        user1.setLogin(1000);
        user1.setRecommend(999);

        dao.update(user1);

        User user1update = dao.get(user1.getId());
        checkSameUser(user1, user1update);

        User user2same = dao.get(user2.getId());
        checkSameUser(user2, user2same);
    }
```

update() 메소드의 SQL에 WHERE이 없다면 이 테스트는 실패로 끝날 것이다. 

5.1.3 UserService.upgradeLevels()

사용자 관리 로직을 어디다 두는 것이 좋을까? 비즈니스 로직 서비스를 제공한다는 의미에서 클래스 이름은 UserService로 한다. UserService는 UserDao 인터페이스 타입으로 UserDao 빈을 DI 받아 사용하게 만든다. 

### UserService 클래스와 빈 등록

```JAVA
public class UserService {

    UserDao userDao;

    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }
...
```
userService 빈 설정
```XML
    <bean id="userService" class="me.stead95.chap01.service.UserService">
        <property name="userDao" ref="userDao"></property>
    </bean>
```

### UserServiceTest 테스트 클래스

UserServiceTest 클래스
```JAVA
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "/applicationContext.xml")
public class UserServiceTest {
    @Autowired
    UserService userService;
    
    @Autowired
    UserDao userDao;
...
```

### upgradeLevels() 메소드

사용자 레벨 업그레이드 메소드
```JAVA
   public void upgradeLevels() {
        List<User> users = userDao.getAll();

        for(User user : users) {
            Boolean changed = null;

            if(user.getLevel() == Level.BASIC && user.getLogin() >= 50) {
                user.setLevel(Level.SILVER);
                changed = true;
            }

            else if (user.getLevel() == Level.SILVER && user.getRecommend() >= 30) {
                user.setLevel(Level.GOLD);
                changed = true;
            }
            else if(user.getLevel() == Level.GOLD) { changed = false; }
            else { changed = false; }

            if(changed) { userDao.update(user); }
        }
    }
```
이 정도 코드라면 오류가 있는지 없는지 쉽게 찾아낼 자신이 있는 개발자도 있겠지만 뛰어난 개발자라면 아무리 간단해 보여도 실수할 수 있음을 알고 있기 때문에 테스트를 만들어서 직접 동작하는 모습을 확인해보려고 할 것이다.

### upgradeLevels() 테스트

테스트 방법을 생각해보자. 적어도 가능한 모든 조건을 하나씩은 확인해봐야 한다.

```JAVA
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "/applicationContext.xml")
public class UserServiceTest {

    List<User> users;

    @Before
    public void setUp() {
        users = Arrays.asList(
                new User("bumjin", "박범진", "p1", Level.BASIC, 49, 0),
                new User("joytouch", "강명성", "p2", Level.BASIC, 50, 0),
                new User("erwins", "신승한", "p3", Level.SILVER, 60, 29),
                new User("madnite1", "이승호", "p4", Level.SILVER, 60, 30),
                new User("green", "오민규", "p5", Level.GOLD, 100, 100)
        );
    }
...
```

사용자 레벨 업그레이드 테스트
```JAVA
    @Test
    public void upgradeLevels() {
        userDao.deleteAll();
        for(User user : users) userDao.add(user);

        userService.upgradeLevels();

        checkLevel(users.get(0), Level.BASIC);
        checkLevel(users.get(1), Level.SILVER);
        checkLevel(users.get(2), Level.SILVER);
        checkLevel(users.get(3), Level.GOLD);
        checkLevel(users.get(4), Level.GOLD);
    }

    private void checkLevel(User user, Level expectedLevel) {
        User userUpdate = userDao.get(user.getId());
        assertThat(userUpdate.getLevel(), is(expectedLevel));
    }
...
```

## 5.1.4 UserService.add()

사용자는 기본적으로 BASIC 레벨이어야 한다는 부분이다. 이 로직은 어디에 담는 것이 좋을까 ?

User클래스에서 level 필드로 초기화 하는 것은 처음 가입할 때를 제외하면 무의미한 정보인데 클래스에서 직접 초기화하는 것은 그리 좋은 선택은 아니다.

차라리 UserService에 add()를 만들고 사용자가 등록될 때 적용할만한 비즈니스 로직을 담당하게 하면 될 것이다.

테스트 케이스는 두 종류를 만들면 된다. 레벨이 미리 정해진 경우와 레벨이 비어 있는 두 가지 경우를 고려해야 한다.

add() 메소드 테스트
```JAVA
    @Test
    public void add() {
        userDao.deleteAll();

        User userWithLevel = users.get(4);
        User userWithoutLevel = users.get(0);
        userWithoutLevel.setLevel(null);

        userService.add(userWithLevel);
        userService.add(userWithoutLevel);

        User userWithLevelRead = userDao.get(userWithLevel.getId());
        User userWithoutLevelRead = userDao.get(userWithoutLevel.getId());

        assertThat(userWithLevelRead.getLevel(), is(userWithLevel.getLevel()));
        assertThat(userWithoutLevelRead.getLevel(), is(userWithoutLevelRead.getLevel()));

    }
```

사용자 신규 등록 로직을 담은 add()메소드
```JAVA
    public void add(User user) {
        if(user.getLevel()==null) user.setLevel(Level.BASIC);
        userDao.add(user);
    }
```

## 5.1.5 코드 개선

비즈니스 로직의 구현을 마쳤다. 개발자라면 다음과 같은 질문을 해볼 필요가 있다.

코드에 중복된 부분은 없는가 ?
코드가 무엇을 하는 것인지 이해하기 불편하지 않은가 ?
코드가 자신이 있어야 할 자리에 있는가 ?
앞으로의 변경이 일어난다면 어떤 것이 있을 수 있고, 변환에 쉽게 대응할 수 있게 작성되어있는가 ?

### upgradeLevels() 메소드 코드의 문제점

UserService의 upgradeLevel() 메소드를 살펴보면 문제점이 보인다. 

for 루프속에 if/elseif/else 블록들이 읽기 불편하고, 조건이 충족되었을 때 할 작업이 한데 섞여 있어서 로직을 이해하기가 쉽지 않다.
또한 플래그를 두고 이를 변경하고 마지막에 이를 확인해서 업데이트를 진행하는 방법도 그리 깔끔해보이지 않는다.

코드가 깔끔해 보이지 않는 이유는 이렇게 성격이 다른 여러가지 로직이 한데 섞여 있기 때문이다. 
만약 새로운 레벨이 추가된다면 갈수록 이해하고 관리하기가 힘들어진다.

주어진 비즈니스 로직을 잘 처리하는 코드인 듯 보이지만 사실 곰곰히 따져보면 상당히 변화에 취약하고 다루기 힘든 코드임을 알 수 있다.

### upgradeLevels() 리팩토링

먼저 추상적인 레벨에서 로직을 작성해보자. 기존의 upgradeLevels() 메소드는 자주 변경 될 가능성이 있는 구체적인 내용이 추상적인 로직의 흐름과 함께 섞여 있다. 레벨을 업그레이드하는 작업의 기본 흐름만 먼저 만들어보자.

기본 작업 흐름만 남겨둔 upgradeLevels()

```JAVA 
    public void upgradeLevels() {
        List<User> users = userDao.getAll();

        for (user user : users) {
            if (canUpgradeLevel(user)) {
                upgradeLevel(user);
            }
        }
    }
```

업그레이드 가능 확인 메소드
```JAVA
    private boolean canUpgradeLevel(User user) {
        Level currentLevel = user.getLevel();

        switch (currentLevel){
            case BASIC :return (user.getLogin() >= 50);
            case SILVER : return (user.getRecommend() >= 30);
            case GOLD : return false;
            default :
                throw new IllegalArgumentException("Unknow Level : " + currentLevel); // 현재 로직에서 다룰 수 없는 예외,
        }
    }
```

다음은 업그레이드 조건을 만족했을 경우 구체적으로 무엇을 할 것인가를 담고 있는 upgradeLevel() 메소드를 만들어보자.
관리자에게 통보, 업그레이드 안내 메일 같은 작업을 할 수도 있다.

```JAVA
    private void upgradeLevel(User user) {
        if (user.getLevel() == Level.BASIC) user.setLevel(Level.SILVER);
        else if(user.getLevel() == Level.SILVER) user.setLevel(Level.GOLD);
        userDao.update(user);
    }
```

upgradeLevel() 메소드 코드는 마음에 안든다. 다음 단계까 무엇인가 하는 로직과 그때 사용자 오브젝트의 level 필드를 변경해준다는 로직이 함께 있는데다가 너무 노골적으로 드러나 있다. 게다가 예외상황에 대한 처리가 없다. 만약 업그레이드 조건을 잘못 파악해서 더 이상 다음 단계가 GOLD 레벨인 사용자를 업그레이드하려고 이 메소드를 호출한다면 아무것도 처리하지 않고 그냥 DAO 업데이트 메소드만 실행 될 것이다.

먼저 레벨의 순서와 다음 단계 레벨이 무엇인지를 결정하는 일은 Level에게 맡기자.

```JAVA
public enum Level {
    GOLD(3, null), SILVER(2, GOLD), BASIC(1, SILVER); // 3개의 이눔 오브젝트 정의

    private final int value;
    private final Level next;

    Level(int value, Level next) { // DB에 저장할 값을 넣어줄 생성자를 만들어둔다.
        this.value = value;
        this.next = next;
    }

    public int intValue() { //값을 가져오는 메소드
        return value;
    }

    public Level nextLevel() {
        return this.next;
    }

    public static Level valueOf(int value) { // 값으로부터 Level 타입 오브젝트를 가져오도록 만든 스태틱 메소드
        switch(value) {
            case 1 : return BASIC;
            case 2 : return SILVER;
            case 3 : return GOLD;
            default : throw new AssertionError("Unknown value : " + value);
        }
    }
}
```

Level 이늄에서 레벨의 업그레이드 순서를 관리 할 수 있다.

이번에는 사용자 정보가 바뀌늰 부분을 UserService 메소드에서 User로 옮겨보자. User의 내부 정보가 변경되는 것은 UserService보다는 User가 스스로 다루는게 적절하다.

User.upgradeLevel()
```JAVA
    public void upgradeLevel() {
        Level nextLevel = this.level.nextLevel();
        if (nextLevel == null) {
            throw new IllegalStateException(this.level + "은 업그레이드가 불가능하비다.");
        }
        else {
            this.level = nextLevel;
        }
    }
```

UserService.upgradeLevel(User user)
```JAVA
    private void upgradeLevel(User user) {
        user.upgradeLevel();
        userDao.update(user);
    }
```

개선한 코드를 살펴보면 각 오브젝트와 메소드가 각각 자기 몫의 책임을 맡아 일을 하는 구조로 만들졌음을 알 수 있을 것이다.

오브젝트에게 데이터를 요구하지 말고 작업을 요청하라는 것이 객체지향 프로그래밍의 가장 기본이 되는 원리이기도 하다.

처음 구현했던 UserService의 upgradeLevels() 메소드는 User 오브젝트에서 데이터를 가져와서 그것을 가지고 User 오브젝트나 Level 이늄이 해야 할 작업을 대신 수행하고 직접 User 오브젝트의 데이터를 변경해버린다. 이보다는 UserService는 User에게 "레벨 업그레이드 작업을 해달라"고 요청하고 또 User는 Level에게 "다음 레벨이 무엇인지 알려달라"고 요청하는 방식으로 동작하게 하는 것이 바람직하다.

### User 테스트

방금 User에 간단하지만 로직을 담은 메소드를 추가했다. 이 정도는 테스트 없이 넘어갈 수도 있겠지만 앞으로 새로운 기능과 로직이 추가될 가능성이 있으니 테스트를 만들어두면 도움이 될 것이다.

User 테스트
```JAVA

public class UserTest {

    User user;

    @Before
    public void setUp() {
        user = new User();
    }

    @Test()
    public void upgradeLevel() {
        Level[] levels = Level.values();
        for (Level level : levels) {
            if (level.nextLevel() == null) continue;
            user.setLevel(level);
            user.upgradeLevel();
            assertThat(user.getLevel(), is(level.nextLevel()));
        }
    }

    @Test(expected = IllegalStateException.class)
    public void cannotUpgradeLevel() {
        Level[] levels = Level.values();
        for (Level level : levels) {
            if (level.nextLevel() != null) continue;
            user.setLevel(level);
            user.upgradeLevel();
        }
    }
}
```

User 클래스에 대한 테스트는 굳이 스프링의 테스트 컨텍스트를 사용하지 않아도 된다. User 오브젝트는 스프링이 IoC로 관리해주는 오브젝트가 아니기 때문이다.

### UserServiceTest 개선

UserService 테스트도 checkLevel() 메소드를 호출할 때 일일이 다음 단계의 레벨이 무엇인지 넣어주었다.

레벨이 추가되면 테스트도 수정되어야 하니 번거롭다.

```JAVA
    @Test
    public void upgradeLevels() {
        userDao.deleteAll();
        for(User user : users) userDao.add(user);

        userService.upgradeLevel(user);

        checkLevel(users.get(0), false);
        checkLevel(users.get(1), true);
        checkLevel(users.get(2), false);
        checkLevel(users.get(3), true);
        checkLevel(users.get(4), false);
    }

    private void checkLevel(User user,Boolean upgraded) {
        User userUpdate = userDao.get(user.getId());

        if(upgraded) {
            assertThat(userUpdate.getLevel(), is(user.getLevel().nextLevel()));
        }
        else {
            assertThat(userUpdate.getLevel(), is(user.getLevel()));
        }
    }
```

기존의 테스트 코드는 테스트 로직이 분명하게 드러나지 않는 것이 단점이었다. checkLevel()을 호출하면서 파라미터로 Level 이늄을 하나 전달하는데, 테스트 코드만 봐서는 그것이 업그레이드된 경우를 테스트 하려는 것인지 쉽게 파악이 안된다.

그에 반해 개선한 upgradeLevels() 테스트는 각 사용자에 대해 업그레이드가 일어나는지 아닌지가 좀 더 이해하기 쉽게 true, false로 나타나 있어서 보기 좋다.

다음은 코드에서 중복을 제거해보자, 50이 중복되고 있다.

UserService            case BASIC : return (user.getLogin() >= 50);
UserServiceTest        new User("joytouch", "강명성", "p2", Level.BASIC, 50, 0),

테스트와 애플리케이션 코드에 나타난 이런 숫자의 중복도 제거해주어야 할까? 당연한다. 한가지 변경 이유가 발생했을 때 여러 군데를 고치게 만든다면 중복이기 때문이다. 테스트에서는 어느 정도 애플리케이션 로직이 반복돼서 나타날 수밖에 없긴하지만 그래도 이런 상수 값을 중복하는 건 바람직하지 못하다. 

UserService 상수의 도입

```JAVA
...
    public static final int MIN_LOGOUT_FOR_SILVER = 50;
    public static final int MIN_RECOMEND_FOR_GOLD = 30;
...
            case BASIC : return (user.getLogin() >= MIN_LOGOUT_FOR_SILVER);
            case SILVER : return (user.getRecommend() >= MIN_RECOMEND_FOR_GOLD);
```

UserServiceTest 상수를 사용하도록 만든 테스트
```JAVA
...
    @Before
    public void setUp() {
        users = Arrays.asList(
                new User("bumjin", "박범진", "p1", Level.BASIC, UserService.MIN_LOGOUT_FOR_SILVER -1, 0),
                new User("joytouch", "강명성", "p2", Level.BASIC, UserService.MIN_LOGOUT_FOR_SILVER, 0),
                new User("erwins", "신승한", "p3", Level.SILVER, 60, UserService.MIN_RECOMEND_FOR_GOLD-1),
                new User("madnite1", "이승호", "p4", Level.SILVER, 60, UserService.MIN_RECOMEND_FOR_GOLD),
                new User("green", "오민규", "p5", Level.GOLD, 100, Integer.MAX_VALUE)
...
```

숫자로만 되어 있는 경우에는 비즈니스 로직을 상세히 코메느로 달아놓거나 설계문서를 참조하기 전에는 이해하기 힘들었던 부분이 이제는 무슨 의도로 어떤 값을 넣었는지 이해하기 쉬워졌다.

좀 더 욕심을 내자면 레벨을 업그레이드 하는 정책이 유연하게 변경할 수 있도록 개선하는 것도 생각해볼 수 있다.

이런 경우 사용자 업그레이드 정책을 UserService에서 분리하고 업그레이드 정책을 담은 오브젝트는 DI를 통해 UserServie에 주입한다.

# 5.2 트랜잭션 서비스 추상화

이제 사용자 레벨 관리 기능에 대한 구현을 마쳤고 테스트를 통해 검증도 끝났다.

이 시스템을 사용할 고객과 개발팀의 열띤 토론 끝에 사용자 레벨 조정 작업은 중간에 문제가 발생해서 작업이 중단된다면 모두 취소시키도록 결정했다.

## 5.2.1 모 아니면 도

모든 사용자에 대해 업그레이드 작업을 진행하다가 중간에 예외가 발생해서 작업이 중단된다면 어떻게 될까?

테스트를 만들어서 확인하자. 시스템 예외상황을 만들면 더 좋겠지만 강제적인 장애상황을 연출하는건 불가능하다.

게다가 테스트의 기본은 자동화된 테스트여야 한다. 일어나는 현상 중의 하나인 예외가 던져지는 상황을 의도적으로 만드는게 나을 것이다.

### 테스트용 UserService 대역

쉬운 방법은 예외를 강제로 발생시키도록 애플리케이션 코드를 수정하는 것이다. 하지만 테스트를 위해서 코드를 함부러 건드는 것은 좋은 생각이 아니다. 그래서 이런 경우엔 테스트용으로 특별히 USerService의 대역을 사용하는 방법이 좋다.

테스트용 UserService 확장 클래스는 어떻게 만들 수 있을까? UserService를 상속해서 테스트에 필요한 기능을 추가하도록 일부 메소드를 오버라이딩 하는 방법이 나을 것 같다.

테스트에서만 사용할 클래스라면 번거롭게 파일을 만들지 말고 테스트 클래스 내부에 스태틱 클래스로 만드는 것이 간편하다.

그런데 UserService의 메소드 대부분은 현재 private 접근제한이 걸려 있어서 오버라이딩이 불가능하다.

UserService의 접근권한을 protected로 수정해서 상속을 통해 오버라이딩이 가능하게 하자.

UserService의 테스트용 대역 클래스

```JAVA
    static class TestUserService extends UserService {
        private String id;

        private TestUserService(String id) {
            this.id = id;
        }

        protected void upgradeLevel(User user) {
            if (user.getId().equals(this.id)) throw new TestUserServiceExcepton();
            super.upgradeLevel(user);
        }
    }

    static class TestUserServiceExcepton extends RuntimeException {
    }
```

오버라이드된 upgradeLevel() 메소드는 UserService 메소드의 기능을 그대로 수행하지만 미리 지정된 id를 가진 사용자가 발견되면 강제로 예외를 던지도록 만들었다. 다른 예외가 발생했을 경우와 구분하기 위해 테스트 목적을 띤 예외를 테스트 클래스 내에 스태틱 멤버 클래스로 만들어도 된다.

### 강제 예외 발생을 통한 테스트

예외 발생 시 작업 취소 여부 테스트
```JAVA
    @Test
    public void upgradeAllOrNothing() {
        UserService testUSerService = new TestUserService(users.get(3).getId());
        testUSerService.setUserDao(this.userDao); // userDao를 수동 DI해준다.

        userDao.deleteAll();
        for (User user : users) userDao.add(user);

        try { // TestUserService는 업그레이드 작업 중에 예외가 발생해야 한다. 정상종료라면 문제가 있으니 실패
            testUSerService.upgradeLevels();
            fail("TestUserServiceException expected");
        } catch (TestUserServiceExcepton e) { //TestUserService가 던져주는 예외를 잡아서 계속 진행되도록 한다. 그 외의 예외라면 실패
        }

        checkLevel(users.get(1), false); // 예외가 발생하기 전에 레벨이 변경이 있었던 사용자의 레벨이 처음 상태로 바뀌었나 확인
    }
```

테스트 용으로 개조한 upgradeLevels()에서는 DB에서 5개의 User를 가져와 차례로 ㅇ버그레이드를 하다가 지정해둔 4번째 사용자 오브젝트 차례가 되면 TestUserServiceException을 발생시킬 것이다. 정상 종료되면 fail() 메소드 때문에 테스트가 실패할 것이다. fail()은 테스트가 의도한 대로 동작하는지를 확인하기 위해 넣은 것이다. 

그리고 두번째 사용자 레벨이 변경됐는지 확인한다. 우리가 기대하는 건 네번째 사용자를 처리하다가 예외ㅓ가 발생해서 작업이 중단됐으니 이미 레벨을 수정했던 두 번째 사용자도 원래 상태로 돌아가는 것이다. 

테스트는 실패한다.
```CONSOLE
java.lang.AssertionError: 
Expected: is <BASIC>
     but: was <SILVER>
<Click to see difference>
```

### 테스트 실패의 원인

`트랜잭션`이란 더 이상 나눌 수 없는 `단위 작업`을 말한다. 작업을 쪼개서 작은 단위로 만들 수 없다는 것은 `트랜잭션`의 핵심 속성인 `원자성`을 의미한다.

이 작업도 더 이상 쪼개서 이뤄질 수 없는 원자와 같은 성질을 띤다. 따라서 중간에 예외가 발생해서 작업을 완료할 수 없다면 아예 작업이 시작되지 않은 것처럼 초기 상태로 돌려놔야 한다. 이것이 바로 트랜잭션이다.

## 5.2.2 트랜잭션 경계 설정

DB는 그 자체로 완벽한 트랜잭션을 지원한다. 하나의 SQL 명령을 처리하는 경우는 DB가 트랜잭션을 보장해준다고 믿을 수 있다. 하지만 여러개의 SQL이 사용되는 작업을 하나의 트랜잭션으로 취급해야 하는 경우도 있다.

여러 개의 SQL을 하나의 트랜잭션으로 처리하는 경우에 하나의 작업이라도 문제가 발생할 경우에는 앞에서 처리한 작업도 취소시켜야 한다. 이를 `트랜잭션 롤백(Transaction Rollback)`이라고 한다.

여러 개의 SQL을 하나의 트랜잭션으로 처리하는 경우에 모든 SQL 수행 작업이 성공적으로 마무리 됐다고 DB에 작업을 확정시키는 것을 `트랜잭션 커밋(Transaction Commit)`이라고 한다.

### JDBC 트랜잭션의 트랜잭션 경계 설정

모든 트랜잭션은 시작하는 지점과 끝나는 지점이 있다. 끝나는 방법은 모든 작업을 무효화 하는 롤백과 모든 작업을 다 확정시키는 커밋이다.
애플리케이션 내에서 트랜잭션이 시작되고 끝나는 위치를 트랜잭션의 경걔라고 부른다. 정확하게 트랜잭션 경계를 설정하는 일은 매우 중요한 작업이다.

트랜잭션을 사용한 JDBC 코드
```JAVA
Connection c = dataSource.getConnection();

c.setAutoCommit(false);
try {
  PreparedStatement st1 = c.prepareStatement("update users ...");
  str1.executeUpdate();

  PreparedStatement st2 = c.prepareStatement("delete users ...");
  str2.executeUpdate();
 
  c.commit(); 
}
catch(Exception e) {
  c.rollback();
}

c.close
```

JDBC의 트랜잭션은 하나의 Connection을 가져와 사용하다가 닫는 사이에서 일어난다. 트랜잭션을 시작하려면 자동커밋 옵션을 false로 만들어주면 된다. JDBC의 기본 설정은 DB 작업을 수행한 직후 자동으로 커밋이 되도록 되어 있다. 작업마다 커밋해서 트랜잭션을 끝내버리므로 JDBC에서는 이 기능을 false로 설정해주면 새로운 트랜잭션이 시작되게 만들 수 있다. 트랜잭션이 한 번 시작되면 commit() 또는 rollback() 메소드가 호출될 때까지의 작업이 하나의 트랜잭션으로 묶인다. 작업 중 예외가 발생하면 트랜잭션을 롤백한다.

이렇게 setAutoCommit(false)로 트랜잭션을 시작을 선언하고 commit() 또는 rolbback()으로 트랜잭션을 종료하는 작업을 `트랜잭션의 경계설정(transaction demarcation)`이라고 한다. 트랜잭션의 경계는 하나의 Connection이 만들어지고 닫히는 범위 안에 존재한다는 점도 기억해두자.

### UserService와 UserDao의 트랜잭션 문제

그렇다면 UserService에 트랜잭션이 적용되지 않았는지 생각해보자. 

JDBC의 트랜잭션 경계 설정 메소드는 모두 Connection 오브젝트를 사용하게 되어 있는데, JdbcTemplate을 사용하기 시작한 이우로부터 이 Connection 오브젝트는 구경도 못해봤다. JdbcTemplate은 직접 만들어봤던 JdbcContext와 작업 흐름이 거의 동일하다.

템플릿 메소드 안에서 DataSource의 getConnection() 메소드를 호출해서 Connection 오브젝트를 가져오고 작업을 마치면 Connection을 확실하게 닫아주고 템플릿 메소드를 빠져나온다. 결국 템플릿 메소드 호출 한번에 한개의 DB 커넥션이 만들어지고 닫히는 일까지 일어나는 것이다. 결국 JdbcTemplate의 메소드를 사용하는 UserDao는 각 메소드마다 하나씩의 독립적인 트랜잭션으로 실행될 수밖에 없다.

데이터 액세스 코드를 DAO로 만들어서 분리해놓았을 경우에는 이처럼 DAO메소드를 호출할 때마다 하나의 새로운 트랜잭션이 만들어지는 구조가 될 수밖에 없다. 결국 DAO를 사용하면 비즈니스 로직을 담고 있는 UserService 내에서 진행되는 여러가지 작업을 하나의 트랜잭션으로 묶는 일이 불가능해진다. 

### 비즈니스 로직 내의 트랜잭션 경계 설정

UserService와 UserDao를 그대로 둔 채로 트랜잭션을 적용하려면 결국 트랜잭션 경계설정 작업을 UserService 쪽으로 가져와야 한다. 결국 다음과 같은 구조로 만들어져야 한다.

upgradeLevels의 트랜잭션 경계설정 구조
```JAVA
public void upgradeLevels() throws Exception {
    (1) DB Connection 생성
    (2) 트랜잭션 시작
    try {
        (3) DAO 메소드 호출
        (4) 트랜잭션 커밋
    }
    catch(Exception e) {
        (5) 트랜잭션 롤백
        throw e;
    } finally {
        (6) DB Connection 종료
    }
}
````

트랜잭션을 사용하는 전형적인 JDBC의 구조이다. UserService에서 만든 Coinnection 오브젝트를 UserDao에서 사용하려면 DAO메소드를 호출할 때마다 Connection 오브젝트를 파라미터로 전달해줘야 한다.

Connection 오브젝트를 파라미터로 전달받는 UserDao 메소드
```JAVA
public interface UserDao {
    public void add(Connection c, User user);
    publci void get(Connection c, String id);
    ...
    public void update(Connection c, User user1);
}
````

트랜잭션을 담고 있는 Connection을 공유하려면 더 해줄일이 있다. UserService의 upgradeLevels()는 USerDao의 update()를 직접 호출하지 않는다. UserDao를 사용하는 것은 사용자별로 업그레이드 작업을 진행하는 upgradeLevel()메소드다. 결국 UserService의 메소드 사이에도 같은 Connection 오브젝트를 사용하도록 파라미터로 전달해줘야만 한다.

### UserService의 트랜잭션 경계설정의 문제점

하지만 새로운 문제가 발생한다. 

첫째 DB커넥션을 비롯한 리소스의 깔끔한 처리를 가능하게 했던 JdbcTemplate을 더 이상 활용할 수 없다는 점이다.<br>
두번째 문제점은 DAO의 메소드와 비즈니스 로직을 담고 있는 UserService의 메소드에 Connection 파라미터가 추가돼야 한다는 점이다.<br>
세번째, Connection 파라미터가 UserDao인터페이스 메소드에 추가되면 UserDao는 더이상 데이터 엑세스 기술에 독립적일 일 수 없다는 점이다.<br>
마지막, DAO 메소드에 Connection 파라미터를 받게 하면 테스트코드에도 영향을 미친다.

## 5.2.3 트랜잭션 동기화

스프링은 이를 해결할 수 있는 멋진 방법을 제공해준다.

### Connection 파라미터 제거

트랜잭션 동기화(transaction synchronization)란 Service에서 트랜잭션을 시작하기 위해 만든 Connection 오브젝트를 저장소에 보관해두고 이후에 호출되는 DAO에서 저장된 Connection을 가져다가 사용하게 하는 것이다.

1. UserService는 Connection을 생성 <br>
2. 트랜잭션 동기화 저장소에 저장하고 <br>
3. Connection의 setAutoCommit(false)를 호춣해 트랜잭션을 시작한다. <br>
4. UserDao가 호출하는 JdbcTemplate 메소드에서는 가장 먼저 트랜잭션 동기화 저장소에 현재 시작된 트랜잭션을 가진 Connection 오브젝트가 존재하는지 확인한다. <br>
5. Connection을 이용해 PreparedStatement를 만들어 수정 SQL을 실행한다. <br>
6. 트랜잭션은 동기화 저장소에 저장되어 있다가 다른 작업들을 수행한다. <br>
7. UserService는 이제 Coinnection의 commit을 호출해서 트랜잭션을 완료 시키고 Connection을 반납한다. <br>

트랜잭션 동기화 저장소는 작업 스레드마다 독립적으로 Connection 오브젝트를 저장하고 관리하기 때문에 다중 사용자를 처리하는 서버의 멀티스레드 환경에서도 충돌이 날 염려는 없다. 

### 트랜잭션 동기화 적용

스프링은 JdbcTemplate과 더불어 이런 트랜잭션 동기화 기능을 지원하는 간단한 유틸리티 메소드를 제공하고 있다. 

UserService에서 DB 커넥션을 다룰 때 DataSource가 필요하므로 빈에 대한 DI설정을 해둬야 한다.<br>
스프링이 제공하는 트랜잭션 동기화 관리 클래스는 TransactionSynchronizationManager이다.<br>
DataSourceUtils에서 제공하는 getConnection() 메소드를 통해 DB커넥션을 생성한다.<br>
DataSourceUtils의 getConnection() 메소드는 Connection 오브젝트를 생성해줄 뿐만 아니라 트랜잭션 동기화에 사용하도록 저장소에 바인딩 해준다<br>
트랜잭션 동기화가 되어 있는 채로 JdbcTemplate을 사용하면 JdbcTemplate의 작업에서 동기화시킨 DB커넥션을 사용하게 된다.<br>

### 트랜잭션 테스트 보완

트랜잭션이 적용됐는지 테스트를 해보자. Connection 오브젝트를 가져오기 위해 사용되는 DataSource 빈을 가져와 주입해주는 코드를 추가해야 한다. 테스트 용으로 확장해서 만든 TestUserService는 UserService의 서브클래스 이므로 UserService와 마찬가지로 동기화에 필요한 DataSource를 DI 해줘야 하기 때문이다.

동기화가 적용된 UserService에 따라 수정된 테스트
```JAVA
    @Autowired
    DataSource dataSource;

    @Test
    public void upgradeAllOrNothing() throws Exception {
        UserService testUserService = new TestUserService(users.get(3).getId());
        testUserService.setUserDao(this.userDao); // userDao를 수동 DI해준다.
        testUserService.setDataSource(this.dataSource);
```

이제 모든 사용자의 레벨 업그레이드 작업을 완료하지 못하고 작업이 중단되면 이미 변경된 사용자의 레벨도 모두 원래 상태로 돌아갈 것이다.
나머지 테스트도 문제없이 동작하게 하려면 UserService의 dataSource프로퍼티 설정을 다음과 같이 추가해줘야 한다.

dataSource 프로퍼티를 추가한 userService 빈 설정
```XML
    <bean id="userService" class="me.stead95.chap01.service.UserService">
        <property name="userDao" ref="userDao"></property>
        <property name="dataSource" ref="dataSource"></property>
    </bean>
```

### JdbcTemplate과 트랜잭션 동기화

JdbcTemplate은 미리 생성돼서 트랜잭션 동기화 저장소에 등록된 DB커넥션이나 트랜잭션이 없는 경우에는 JdbcTemplate이 직접 DB 커넥션을 생성하고 트랜잭션을 시작해서 JDBC 작업을 진행한다. 반면에 트랜잭션 동기화를 시작해놓았다면 그때부터 실행되는 JdbcTemplate의 메소드에서는 직접 DB 커넥션을 만드는 대신 트랜잭션 동기화 저장소에 들어 있는 DB 커넥션을 가져와서 사용한다.

## 5.2.4 트랜잭션 서비스 추상화

### 기술과 환경에 종속되는 트랜잭션 경계설정 코드

G사는 여러개의 DB를 사용하고 있다. 그래서 하나의 트랜잭션 안에서 여러개의 DB 작업을 해야 할 필요가 있다. 로컬 트랜잭션은 하나의 DB Connection에 종속되기 때문이다. 각 DB와 독립적으로 만들어지는 Coinnection을 통해서가 아니라 별도의 트랜잭션 관리자를 통해 글로벌 트랜잭션(global transaction) 방식을 사용해야 한다.

자바의 JDBC 외에 이런 글로벌 트랜잭션을 지원하는 트랜잭션 매니저를 지원하기 위한 API인 JTA(Java Transaction API)을 제공하고 있다.

하나 이상의 DB가 참여하는 트랜잭션을 만들려면 JTA를 사용해야 한다는 사실만 기억해두자.

아무튼 G사의 요청은 서버가 제공하는 트랜잭션 매니저와 트랜잭션 서비스를 사용할테니 JDBC API가 아닌 JTA를 사용해 트랜잭션을 관리하게 해달라는 것이다.

```JTA를 이용한 트랜잭션 구조
InitialContext ctx = new InitialContext();
tx.begin();
Connection c = dataSource.getConnection();
try {
    // 데이터 액세스 코드
    tx.commit();
} catch (Exception e) {
    tx.rollback();
    throw e;
} finally {
    c.close();
}
````

JDBC를 사용했을 때와 비슷하다. Connection의 메소드 대신에 UserTransaction의 메소드를 사용한다는 점을 제외하면 트랜잭션 처리 방법은 별로 달라진 게 없다.

고객을 위해서는 JDBC를 이용한 트랜잭션 관리 코드를, G사처럼 다중 DB를 위한 글로벌 트랜잭션을 필요로 하는 곳을 위해서는 JTA를 이용한 트랜잭션 관리 코드를 적용해야 한다는 문제가 생긴다. UserService는 자신의 로직이 바뀌지 않았음에도 기술환경에 따라서 코드가 바뀌는 코드가 돼버리고 말았다.

하이버네이트는 Connection을 직접 사용하지 않고 Session이라는 것을 사용하고 독자적인 트랜잭션 관리 API를 사용한다. 

### 트랜잭션 API의 의존관계 문제와 해결책

이런 문제를 어떻게 해결할 것인가 ? UserService에서 트랜잭션의 경계 설정을 해야 할 필요가 생기면서 다시 특정 데이터 액세스 기술에 종속되는 구조가 되고 말았다. 

JDBC에 종속적인 Connection을 이용한 트랜잭션 코드가 UserService에 등장하면서부터 UserService는 USerDaoJDbc에 간접적으로 의존하는 코드가 돼버렸다는 점이다. 

USerService의 메소드 안에서 트랜잭션 경계설정 코드를 제거할 수는 없다. 하지만 특정 기술에 의존적인 Connection, UserTransaction, Session/Transaction API 등에 종속되지 않게 할 수 있는 방법은 있다. 

추상화란 하위 시스템의 공통점을 뽑아내서 분리시키는 것을 말한다. 하위 시스템이 바뀌더라도 일관된 방법으로 접근할 수 있다.

### 스프링의 트랜잭션 서비스 추상화

스프링은 트랜잭션 기술의 공통점을 담은 트랜잭션 추상화 기술을 제공하고 있다. 

스프링의 트랜잭션 추상화 API를 적용한 upgradeLevels() 
```JAVA
    public void upgradeLevels() throws Exception {
        PlatformTransactionManager transactionManager = new DataSourceTransactionManager(dataSource);
        TransactionStatus status = transactionManager.getTransaction(new DefaultTransactionDefinition());

        try {
            List<User> users = userDao.getAll();

            for (User user : users) {
                if (canUpgradeLevel(user)) {
                    upgradeLevel(user);
                }
            }

            transactionManager.commit(status); //정상작업
        } catch(Exception e) {
            transactionManager.rollback(status); //예외발생
            throw e;
        }
    }
```
스프링이 제공하는 트랜잭션 경계설정을 위한 추상 인터페이스는 PlatformTransactionManager다.<br>
JDBC의 로컬 트랜잭션을 이용하면 PlatformTransactionManager를 구현한DataSourceTransactionManager를 사용하면 된다.<br>
사용할 DB의 DataSource를 생성자 파라미터로 넣으면서 DataSourceTransactionManager의 오브젝트를 만든다.<br>

PlatformTrasactionManager.getTransaction()을 실행하면 DB커넥션을 가져오는 작업과 트랜잭션을 시작한다.
파라미터로 넘기는 DefaultTransactionDefinition 오브젝트는 트랜잭션에 대한 속성을 담고 있다.
시작된 트랜잭션은 TransactionStatus 타입의 변수에 저장된다.
TransactionStatus는 트랜잭션에 대한 조작이 필요할 때 PlatformTransactionManager 메소드의 파라미터로 전달해주면 된다.

### 트랜잭션 기술 설정의 분리

UserService 코드를 JTA를 이용하는 글로벌 트랜잭션으로 변경하려면 어떻게 해야 할까 ? 

PlatformTransactionManager 구현 클래스를 DataSourceTransactionManager에서 JTATransactionManager로 바꿔주기만 하면된다.

PlatformTransactionManager txManager = new JTATransactionManager();

하이버네이트는 HibernateTransactionManager, JPA는 JPATransactionMAnager 모두 PlatformTransactionManager 인터페이스로 구현되었으니 getTransaction(), commit(), rollback() 메소드를 사용한 코드는 전혀 손댈 필요가 없다.

그렇다면 DataSourceTransactionManager는 스프링 빈으로 등록하고 UserService가 DI방식으로 사용하게 해야 한다. 


```JAVA
public class UserService {
...
    private PlatformTransactionManager transactionManager;

    public void setTransactionManager(PlatformTransactionManager transactionManager) {
        this.transactionManager = transactionManager;
    }

    public void upgradeLevels() throws Exception {
        TransactionStatus status = this.transactionManager.getTransaction(new DefaultTransactionDefinition());

        try {
            List<User> users = userDao.getAll();

            for (User user : users) {
                if (canUpgradeLevel(user)) {
                    upgradeLevel(user);
                }
            }

            this.transactionManager.commit(status); //정상작업
        } catch(Exception e) {
            this.transactionManager.rollback(status); //예외발생
            throw e;
        }
    }
```

트랜잭션 매니저 빈을 등록한 설정파일
```XML
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>


    <bean id="userService" class="me.stead95.chap01.service.UserService">
        <property name="userDao" ref="userDao"></property>
        <property name="transactionManager" ref="transactionManager"></property>
    </bean>
```

트랜잭션 매니저를 수동 DI 하도록 수정한 테스트
```JAVA
...
public class UserServiceTest {
...
    @Autowired
    PlatformTransactionManager transactionManager;

    public void upgradeAllOrNothing() throws Exception {
        UserService testUserService = new TestUserService(users.get(3).getId());
        testUserService.setUserDao(this.userDao); // userDao를 수동 DI해준다.
        testUserService.setTransactionManager(transactionManager);
...
```

# 5.3 서비스 추상화와 단일 책임 원리

### 수직 수평 계층구조와 의존 관계

애플리케이션 로직의 종류에 따라 수평적인 구분이든 로직과 기술이라는 수직적인 구분이든 모두 결하보가 낮으며 서로 영향을 주지 않고 자유롭게 확장할 수 있는 구조를 만들 수 있는데는 스프링의 DI가 중요한 역할을 하고 있다. DI의 가치는 이렇게 관심 책임 성격이 다른 코드를 깔끔하게 분리하는데 있다.

### 단일 책임 원칙

객체지향 설계의 원리 중의 하나인 단일 책임 원칙(Single Responsibility Principle) 으로 설명할 수 있다. 하나의 모듈은 한 가지 책임을 가져야 한다는 의미다.

### 단일 책임 원칙의 장점

적절하게 책임과 관심이 다른 코드를 분리하고, 서로 영향을 주지 않도록 다양한 추상화 기법을 도입하고 애플리케이션 로직과 기술/환경을 분리하는 등의 작업은 갈수록 복잡해지는 엔터프라이즈 애플리케이션에는 반드시 필요하다. 이를 위한 핵심적인 도구가 바로 스프링이 제공하는  DI다. 스프링의 DI가 없었다면 코드 사이의 결합이 남아있게된다.

5.4 메일 서비스 추상화

안내 메일을 발송하기 위해서는 할 일이 2가지가 있다. 사용자의 이메일 정보가 관리되야하고 upgradeLevel() 메소드에 메일 발송 기능을 추가하는 것이다.

5.4.1 JavaMail을 이용한 메일 발송 기능

### JavaMail 메일 발송

자바에서 메일을 발송할 때는 표준 기술인 JavaMail을 사용하면 된다.

UserService 레벨 업그레이드 작업 메소드 수정
```JAVA
    protected void upgradeLevel(User user) {
        user.upgradeLevel();
        userDao.update(user);
        sendUpgradeEmail(user);
    }
```

Java을 이용한 메일 발송 코드, 기본 샘플

```JAVA
    private void sendUpgradeEmail(User user) {
        Properties props = new Properties();
        props.put("mail.smtp.host", "mail.ksug.org");
        Session s = Session.getInstance(props, null);

        MimeMessage message = new MimeMessage(s);
        try {
            message.setFrom(new InternetAddress("useradmin@ksug.org"));
            message.addRecipient(Message.RecipientType.TO, new InternetAddress(user.getEmail()));
            message.setSubject("upgrade 안내");
            message.setText("사용자님의 등급이 " + user.getLevel().name() + " 로 업그레이드 되었습니다.");

            Transport.send(message);
        } catch (AddressException e) {
            throw new RuntimeException(e);
        } catch (MessagingExceptino e) {
            throw new RuntimeException(e);
        } catch (UnsupportedEncodingException e) {
            throw new RuntimeException(e);
        }
    }
```

SMTP 프로토콜을 지원하는 메일 전송 서버가 준비되어 있다면 이 코드는 정상적으로 동작할 것이고 안내 메일이 발송될 것이다.

5.4.2 JavaMail이 포함된 코드의 테스트

메일 서버가 준비되어 있지 않다면 어떻게 될 것인가 ? 

메일 서버를 사용하지 않고 테스트 메일 서버를 이용해 테스트 하는 방법을 보여준다.

테스트 메일 서버는 외부로 메일을 발송하지 않고 단지 JavaMail과 연동해서 메일 전송 요청을 받는 것까지만 담당한다.

JavaMail은 자바의 표준 기술이고 검증된 안정적인 모듈이다. 

따라서 JavaMail API를 통해 요청이 들어간다는 보장만 있다면 굳이 테스트할 때 마다 JavaMail을 직접 구동시킬 필요가 없다.

5.4.3 테스트를 위한 서비스 추상화

실제 메일 전송을 수행하는 JavaMail 대신에 테스트에서 사용할 JavaMail과 같은 인터페이스를 갖는 오브젝트를 만들어서 사용하면 문제는 모두 해결된다.

### JavaMail을 이용한 테스트의 문제점

그런데 한가지 심각한 문제가 있다. JavaMAil의 핵심 API에는 DataSource처럼 인터페이스로 만들어져서 구현을 바꿀 수 있는게 없다.

javax.mail.Session 클래스의 사용방법을 살펴보자.
```
Session s = Session.getInstance(props, null);
```

JavaMail에서는 Session 오브젝트를 만들어야만 메일 메시지를 생성할 수 있고 메일을 전송할 수 있다. 이 Session은 인터페이스가 아니고 클래스다. 생성자가 모두 private으로 되어 있어서 직접 생성도 불가능하다. 스태틱 팩토리 메소드를 이용해서 오브젝트를 만드는 방법밖에 없다. Session 클래스는 더 이상 상속이 불가능한 final 클래스다. 메일 메시지를 작성하는 MailMessage도 전송 기능을 맡고 있는 Transport도 마찬가지다. 

결국 JavaMail의 구현을 테스트용으로 바꿔치기 하는 건 불가능하다고 볼 수밖에 없다. 

두 번째 방법으로 생각한 JavaMail 대신 테스트용 JavaMail로 대체해서 사용하는 것은 포기해야할까 ?

스프링은 JavaMail을 사용해 만든 코드는 손쉽게 테스트하기 힘들다는 문제를 해결하기 위해서 JavaMail에 대한 추상화 기능을 제공하고 있다.

### 메일 발송 기능 추상화

JavaMail의 서비스 추상화 인터페이스

```Java
package org.springframework.mail;
...
public interface Mailsender {
    void send(SimpleMailMessage simpleMessage) throws MailException;
    void send(SimpleMailMessage[] simpleMessages) throws MailException;
}
```

이 인터페이스는 SimpleMailMessage라는 인터페이스를 구현한 클래스에 담긴 메일 메시지를 전송하는 메소드로만 구성되어 있다. 

기본적으로는 JavaMail을 사용해 메일 발송 기능을 제공하는 JavaMailSenderImple 클래스를 이용하면 된다.

스프링의 MailSender를 이용한 메일 발송 메소드

```JAVA
private void sendUpgradeEMail(User user) {
    //MailMessage 인터페이스의 구현 클래스 오브젝트를 만들어 메일 내용을 작성한다.
    JavaMailSenderImpl mailSender = new JavaMailSEnderImpl();
    mailMessage.setTo(user.getEmail());
    mailMessage.setFrom("useradmin@ksug.org");
    mailMessage.setSubject("Upgrade 안내");
    mailMessage.setText("사용자님의 등급이 " + user.getLevel().name());

    mailSender.send(mailMessage);
}
````

스프링이 제공하는 JavaMailSender 인터페이스와 관련 클래스 등으로 수정하고 나니 일단 지저분한 try/catch 블록이 사라진 것이 가장 먼저 눈에 띈다. 스프링의 예외처리 원칙에 따라서 JavaMail을 처리하는 중에 발생화 각종 예외를 MailException이라는 런타임 예외로 포장해서 던져주기 때문에 귀찮은 try/catch 블록을 않아도 된다. 

하지만 아직은 JavaMail API를 사용하지 않는 테스트용 오브젝트로 대체할 수는 없다. JAvaMail API를 사용하는 JAvaMailSenderImpl 클래스의 오브젝트를 코드에서 직접 사용하기 때문이다.

메일 전송 기능을 가진 오브젝트를 DI 받고록 수정한 UserService

```JAVA
public class UserService {
...
    private MailSender mailSender;

    public void setMailSender(MailSender mailSender) {
        this.mailSender = mailSender;
    }

    private void sendUpgradeEmail(User user) {
        SimpleMailMessage mailMessage = new SimpleMailMessage();
        mailMessage.setTo(user.getEmail());
        mailMessage.setFrom("useradmin@ksug.org");
        mailMessage.setSubject("Upgrade 안내");
        mailMessage.setText("사용자님의 등급이 " + user.getLevel().name());
        this.mailSender.send(mailMessage);
    }
```

설정 파일안에 JavaMailSenderImpl 클래스로 빈을 만들고 UserService에 DI해준다.
스프링 빈으로 등록되는 MailSender의 구현 클래스들은 싱글톤으로 사용 가능해야 한다.

메일 발송 오브젝트의 빈 등록

```XML
    <bean id="mailSender" class="org.springframework.mail.javamail.JavaMailSenderImpl">
        <property name="host" value="mail.server.com"
    </bean>   
    <bean id="userService" class="me.stead95.chap01.service.UserService">
        <property name="userDao" ref="userDao"></property>
        <property name="transactionManager" ref="transactionManager"></property>
        <property name="mailSender" ref="mailSender"></property>
    </bean>
```

### 테스트용 메일 발송 오브젝트 

mailSender 빈의 host 프로퍼티에는 메일 서버를 지정해준다. 이제 실행하면 JavaMail API를 직접 사용했을 때와 동일하게 지정된 메일 서버로 메일이 발송된다.

스프링이 제공한 메일 전송 기능에 대한 인터페이스가 있으니 이를 구현해서 테스트용 메일 전송 클래스를 만들어보자.

테스트가 수행 될 때는 메일을 전송할 필요가 없으므로 없다.

아무런 기능이 없는 MailSender 구현 클래스

```JAVA
public class DummyMailSender implements MailSender {
    @Override
    public void send(SimpleMailMessage simpleMailMessage) throws MailException {
        
    }

    @Override
    public void send(SimpleMailMessage... simpleMailMessages) throws MailException {

    }
}
```

다음은 테스트 설정 파일의 mailSender 빈 클래스를 다음과 같이 JavaMail을 사용하는 JavaMailSenderImpl 대신 DummyMailSender로 변경한다.

```XML
    <bean id="mailSender" class="me.stead95.chap01.service.DummyMailSender">
<!--        <property name="host" value="mail.server.com"/>-->
    </bean>
```

스프링의 DI를 이용해서 테스트가 구동될 때 UserService가 사용할 오브젝트를 변경해줬기 때문에 UserSErvice 코드 자체에는 아무런 수정이 필요하지 않다. 

테스트용 UserService를 위한 메일 전송 오브젝트의 수동 DI
```JAVA
public class UserServiceTest {
...
    @Autowired
    MailSender mailSender;
...
    @Test
    public void upgradeAllOrNothing() throws Exception {
...
            testUserService.setMailSender(mailSender);
...
}
```

### 테스트 서비스 추상화

일반적으로 서비스 추상화라고 하면 트랜잭션과 같이 기능은 유사하나 사용방법이 다른 로우레벨의 다양한 기술에 대해 추상인터페이스와 일관성있는 접근 방법을 제공해주는 것을 말한다. 

다양한 트랜잭션 기술에 대해 추상화 클래스를 제공하는 것과는 분명 대비된다.

반면에 JavaMail경우처럼 테스트를 어렵게 만드는 건전하지 않은 방식으로 설계된 API를 사용할 때도 유용하게 쓰일 수 있다. 스프링이 직접 제공하는 MailSender를 구현한 추상화 클래스는 JavaMailServiceImpl 하나뿐이다. 그럼에도 불구하고 이 추상화된 메일 전송 기능을 사용해 애플리케이션을 작성함으로서 얻을 수 있는 장점은 크다.

JavaMail이 아닌 다른 메시징 서버의 API를 이용해 메일을 전송해야 하는 경우가 생겨도 해당 기술의 API를 이용하는 MailSender 구현 클래스를 만들어서 DI 해주면 된다.

기술이나 환경이 바뀔 가능성이 있음에도, JavaMail처럼 확장이 불가능하게 설계해놓은 API를 사용해야 하는 경우라면 추상화 계층의 도입을 적극 고려해볼 필요가 있다.

5.4.4 xptmxm eodur

### 의존 오브젝트의 변경을 통한 테스트 방법

테스트 대상이 되는 오브젝트가 또 다른 오브젝트를 의존하는 일은 매우 흔하다. 의존한다는 말은 종속되거나 기능을 사용한다는 의미다. 작은 기능이라도 다른 오브젝트의 기능을 사용하면 사용하는 오브젝트의 기능이 바뀌었을 때 자신이 영향을 받을 수 있기 때문에 의존하고 있다고 말하는 것이다. 

테스트 대상인 오브젝트가 의존 오브젝트를 갖고 있기 때문에 발생하는 여러가지 테스트상의 문제점이 있다. 간단한 오브젝트의 코드를 테스트하는데 너무 거창한 작업이 뒤따르는 경우다. 이럴 때는 DI를 활용해서 테스트 오브젝트 대역을 사용하는 방법이 있다. 테스트만을 위해서도 DI는 유용하다. 

### 테스트 대역의 종류와 특징

이렇게 테스트용으로 사용되는 특별한 오브젝트들이 있다. 대부분 테스트 대상인 오브젝트의 의존 오브젝트가 되는 것들이다. 이런 오브젝트를 통틀어서 테트스 대역(test double)이라고 부른다. 대표적인 테스트 대역은 테스트 스텁(test stub)이다. 테스트 스텁은 테스트 대상 오브젝트의 의존객체로서 존재하면서 테스트 동안에코드가 정상적으로 수행할 수 있도록 돕는 것을 말한다. 

테스트 대상 오브젝트의 메소드가 돌려주는 결과뿐 아니라 테스트 오브젝트가 간접적으로 의존 오브젝트에 넘기는 값과 그 행위 자체에 대해서도 검증하고 싶다면 어떻게 해야할까 ? 이 경우에는 단순하게 메소드의 리턴 값을 assertThat()으로 검증하는 것으로는 불가능하다.

테스트 대상의 간접적인 출력 결과를 검증하고 테스트 대상 오브젝트와 의존 오브젝트 사이에서 일어나는 일을 검증할 수 있도록 특별히 설계된 목 오브젝트(mock object)를 사용해야 한다. 목 오브젝트는 스텁처럼 테스트 오브젝트가 정상적으로 실행되도록 도와주면서 테스트 오브젝트와 자신의 사이에서 일어나는 커뮤니케이션 내용을 저장해뒀다가 테스트 결과를 검증하는 데 활용할 수 있게 해준다.

이때는 테스트 대상과 의존 오브젝트 사이에 주고받는 정보를 보존해두는 기능을 가진 테스트용 의존 오브젝트인 목 오브젝트를 만들어서 사용해야 한다. 테스트 대상 오브젝트의 메소드 호출이 끝나고 나면 테스트는 목 오브젝트에게 테스트 대상과 목 오브젝트 사이에서 일어났던 일에 대해 확인을 요청해서 그것을 테스트 검증 자료로 삼을 수 있다.

### 목 오브젝트를 이용한 테스트

트랜잭션 기능을 테스트하려고 만든 upgradeAllOrNothing()의 경우는 테스트가 수행되는 동안에 메일이 전송됐는지 여부는 관심의 대상이 아니다. 따라서 DummyMKailSender가 잘 어울린다.
반면에 정상적인 사용자 레벨 업그레이드 결과를 확인하는 upgradeLevels() 테스트에서는 메일 전송 자체에 대해서도 검증할 필요가 있다. 우리는 스프링의 JavaMail 서비스 추상화를 적용했기 때문에 방금 살펴본 목 오브젝트를 만들어서 메일 발송 여부를 확인할 수 있다.

DummyMailSender 대신에 새로운 MailSender를 대체할 클래스를 하나 만들자.

목 오브젝트로 만든 메일 전송 확인용 클래스
```Java
public class MockMailSender  implements MailSender  {
    // UserService로부터 전송 요청을 받은 메일 주소를 저장해두고 이를 읽을 수 있게 해준다.
    private List<String> requests = new ArrayList<String>();

    public List<String> getRequests() {
        return requests;
    }

    @Override
    public void send(SimpleMailMessage simpleMailMessage) throws MailException {
        //전송 요청을 받은 이메일 주소를 저장해둔다. 간단하게 첫번째 수신자 메일 주소만 저장했다.
        requests.add(simpleMailMessage.getTo()[0]);

    }

    @Override
    public void send(SimpleMailMessage... simpleMailMessages) throws MailException {

    }
}
```

이 클래스는 메일 전송 요청을 보냈을 때 관련 정보를 저장해두는 기능이 있다. 리스트 이므로 순서대로 중복 호출까지 포함해서 다 저장해둘 것이다. 테스트대상 오브젝트가 목 오브젝트에게 전달하는 출력정보를 저장해두는 것이다. 

메일 발송 대상을 확인하는 테스트
```JAVA
    @Test
    @DirtiesContext //컨텍스트의 DI 설정을 변경하는 테스트라는 것을 알려준다.
    public void upgradeLevels() throws Exception {
        userDao.deleteAll();
        for (User user : users) userDao.add(user);

        //메일 발송 결과를 테스트할 수 있도록 목 오브젝트를 만들어
        //userService의 의존 오브젝트로 주입해준다.
        MockMailSender mockMailSender = new MockMailSender();
        userService.setMailSender(mockMailSender);

        userService.upgradeLevels();

        checkLevel(users.get(0), false);
        checkLevel(users.get(1), true);
        checkLevel(users.get(2), false);
        checkLevel(users.get(3), true);
        checkLevel(users.get(4), false);

        List<String> request = mockMailSender.getRequests();
        assertThat(request.size(), is(2));
        assertThat(request.get(0), is(users.get(1).getEmail()));
        assertThat(request.get(1), is(users.get(3).getEmail()));
    }
```

테스트가 수행될 수 있도록 의존 오브젝트에 간접적으로 입력 값을 제공해주는 스텁 오브젝트와 간접적인 출력 값까지 확인이 가능한 목 오브젝트, 이 두가지는 테스트 대역의 가장 대표적인 방법이며 효과적인 테스트 코드를 작성하는데 빠질 수 없는 중요한 도구다. 

