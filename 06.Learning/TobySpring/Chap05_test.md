# Chap05. 서비스 추상화

* 스프링이 어떻게 성격이 비슷한 여러 종류의 기술을 추상화하고 이를 일관된 방법으로 사용할 수 있도록 지원하는 지를 살펴볼 것 이다.


* 코드를 정의할 때 이늄(enum)을 이용하는게 안전하고 편리하다. 이렇게 만들어진 Level 이늄은 내부에서 DB에 저장할 int타입의 값을 갖고 있지만 겉으로는(코드상) Level 타입의 오브젝트이기 때문에 안전하게 사용할 수 있다.

* Enum
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

* UserDao RowMapper Interface

    RowMapper 인터페이스를 구현해서 resultSet을 domain객체로 변환할 수 있다.

    ```JAVA
    public class UserDao {    
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
    }            
    ```            

* 사용자 관리 로직은 어디다 두는 것이 좋을까 ?

      UserDaoJdbc는 적당하지 않다. DAO는 데이터를 어떻게 가져오고 조작할지를 다루는 곳이지 비즈니스 로직을 두는 곳이 아니다.
      사용자 관리 비즈니스 로직을 담을 서비스 클래스를 하나 추가하자. 
      
* 처음 가입하면 사용자는 기본적으로 BASIC 레벨이어야 한다. 이 로직은 어디에 담는 것이 좋을까?

      UserDaoJdbc의 Add() 메소드는 적합하지 않아 보인다. UserDaoJdbc는 주어진 User 오브젝트를 DB에 정보를 넣고 읽는 방법에만 관심
      을 가져야지 비즈니스적인 의미를 지닌 정보를 설정하는 책임을 지는 것은 바람직하지 않다.
      
      User 클래스에서 아예 level필드를 Level.BASIC으로 초기화하는 것은 어떨까? 처음 가입할 때를 제외하면 무의미한 정보인데 단지
      이 로직을 담기 위해 클래스에서 직접 초기화하는 것은 좀 문제가 있어 보인다.
      
      그렇다면 사용자 관리에 대한 비즈니스 로직을 담고 있는 UserService에 이 로직을 넣으면 어떨까? UserDao의 add() 메소드는 사용자
      정보를 담은 User 오브젝트를 받아서 DB에 넣어주는 데 충실한 역할을 한다면 UserService에도 add()를 만들어두고 사용자가 등록될 때
      적용할만한 비즈니스 로직을 담당하게 하면 될 것이다.
      
* upgradeLevels() 리팩토링

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

    UserService의 upgradeLevel() 메소드를 살펴보면 문제점이 보인다.
    for 루프속에 if/elseif/else 블록들이 읽기 불편하고, 조건이 충족되었을 때 할 작업이 한데 섞여 있어서 로직을 이해하기가 쉽지 않다.
    또한 플래그를 두고 이를 변경하고 마지막에 이를 확인해서 업데이트를 진행하는 방법도 그리 깔끔해보이지 않는다.

    코드가 깔끔해 보이지 않는 이유는 이렇게 성격이 다른 여러가지 로직이 한데 섞여 있기 때문이다.
     
    * user.getLeve() == Level.BASIC -> 현재 레벨이 무엇인지 파악하는 로직이다.
    * user.getLogin() >= 50 -> 업그레이드 조건을 담은 로직이다.
    * user.setLevel(Level.SILVER); -> 다음 단계의 레벨이 무엇이며 업그레이드를 위한 작업은 어떤 것인지가 함께 담겨있다.
    * changed = true; 
      if (changed) { userDao.update(user); } -> flag를 통한 업그레이드 작업이다.
    * Level 수정되거나 추가할수록 if 조건이 늘어난다.
     
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
                case BASIC : return (user.getLogin() >= 50);
                case SILVER : return (user.getRecommend() >= 30);
                case GOLD : return false;
                default :
                    throw new IllegalArgumentException("Unknow Level : " + currentLevel); // 현재 로직에서 다룰 수 없는 예외,
            }
        }
    ```    

    레벨 업그레이드 작업 메소드
    ```JAVA
        private void upgradeLevel(User user) {
            if (user.getLevel() == Level.BASIC) user.setLevel(Level.SILVER);
            else if(user.getLevel() == Level.SILVER) user.setLevel(Level.GOLD);
            userDao.update(user);
        }
    ```    

    upgradeLevel에서 레벨 순서와 다음 단계 레벨이 무엇인지를 결정하는 일은 Level에게 맡기자
    ```JAVA
    public enum Level {
        GOLD(3, null), SILVER(2, GOLD), BASIC(1, SILVER); // 3개의 이눔 오브젝트 정의

        private final int value;
        private final Level next; // 다음 단계 레벨 정보를 스스로 갖고 있도록 next변수를 추가한다.

        Level(int value, Level next) { 
            this.value = value;
            this.next = next;
        }

        public int intValue() { 
            return value;
        }

        public Level nextLevel() { // 다음단계레벨
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

    사용자 정보가 바뀌는 부분을 UserService 메소드에서 User로 옮겨보자. User의 내부 정보가 변경되는 것은 UserService보다는 User가 스스로 다루는게 적절하다.

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

    간결해진 UserServce.upgradeLevel()
    ```JAVA
        private void upgradeLevel(User user) {
            user.upgradeLevel();
            userDao.update(user);
        }
    ```    

    객체지향적인 코드는 다른 오브젝트의 데이터를 가져와서 작업하는 대신 데이터를 갖고 있는 다른 오브젝트에게 작업을 해달라고 요청을한다.
    그것이 객체지향 프로그래밍의 가장 기본이 되는 원리이기도 하다.

* 트랜잭션

      트랜잭션이란 더 이상 나눌 수 없는 단위 작업을 말한다. 작업을 쪼개서 작은 단위로 만들 수 없다는 것은
      트랜잭션의 핵심 속성인 원자성을 의미한다.
    
* DB는 그 자체로 완벽한 트랜잭션을 지원한다. 다중 로우의 수정이나 삭제를 위한 요청을 했을 때 일부 로우만 수정이나 삭제되고 끝나는 경우는 없다.

* 모든 트랜잭션은 시작하는 지점과 끝나는 지점이 있다. 시작하는 방법은 한가지 이지만 끝나는 방법은 두 가지다. 모든 작업을 무효화하는 `롤백`과 모든 작업을 다 확정하는 `커밋`이다. 애플리케이션 내에서 트랜잭션이 시작되고 끝나는 위치를 `트랜잭션의 경계`라고 부른다.

* JDBC의 트랜잭션의 하나의 Connection을 가져와 사용하다가 닫는 사이에서 일어난다. 트랜잭션의 시작과 종료는 Coinnection 오브젝트를 통해 이뤄지기 때문이다. JDBC에서 트랜잭션을 시작하려면 자동 커밋옵션을 false로 만들어주면 된다. 트랜잭션이 한 번 시작되면 commit() 또는 rollback()이 호출되면 그에 따라 작업결과가 DB에 반영되거나 취소되고 트랜잭션이 종료된다. 그리고 작업 중에 예외가 발생하면 트랜잭션을 롤백한다.

* 트랜잭션의 경계설정(Transaction Demarcation)

      setAutoCommit(false)로 트랜잭션의 시작을 선언하고 commit() 또는 rollback()으로 트랜잭션을 종료하는 작업.
      트랜잭션의 경계는 하나의 Connection이 만들어지고 닫히는 범위 안에 존재한다. 
      
* 트랜잭션 경계설정 구조
    ```JAVA
    public void upgradeLevels() throws Exception {
      (1) DB Connection 생성
      (2) 트랜잭션 시작
      try {
        (3) DAO 메소드 호출
        (4) 트랜잭션 커밋
      } catch(Exception e) {
        (5) 트랜잭션 롤백
        throw e;
      }
      finally {
        (6) DB Connection 종료
      }
    }
    ```

* UserService와 UserDao를 그대로 둔 채로 트랜잭션을 적용하려면 결국 트랜잭션의 경계설정 작업을 UserService 쪽으로 가져와야 한다.      
      
* 트랜잭션 동기화(transaction synchronization)

      트랜잭션 동기화란 트랜잭션을 시작하기 위해 만든 Connection 오브젝트를 특별한 저장소에 보관해두고 이후에
      호출되는 DAO의 메서드에서는 저장된 Connection을 가져다가 사용하게 하는 것이다. 
      정확히는 DAO가 사용하는 JdbcTemplate이 트랜잭션 동기화 방식이 이용하도록 하는 것이다.
      
* 트랜잭션 동기화 작업흐름      

    * UserService는 Connection을 생성한다.
    * 생성된 Connection 트랜잭션 동기화 저장소에 저장해두고 Connection.setAutoCommit(false)를 호출한다.
    * UserDao의 update() 메소드를 호출한다.
    * update() 메소드 내부에서 이용하는 JdbcTemplate 메소드에서 트랜잭션 동기화 저장소에 현재 시작된 트랜잭션을 가진 Connection 오브젝트가 존재하는지 확인한다. 
    * Connection을 이용해 PrepareStatement를 만들어 SQL을 실행한다.
    * 트랜잭션 동기화 저장소에서 DB 커넥션을 가져왔을 때 JdbcTemplate은 Connection을 닫지 않은 채로 작업을 마친다.
    * 여전히 Connection은 열려 있고 트랜잭션은 진행 중인 채로 트랜잭션 동기화 저장소에 저장되어 있다.
    * 두번째 update()가 호출되면 마찬가지로 트랜잭션 동기화 저장소에서 Connection을 가져와 사용한다.
    * 트랜잭션 내의 모든 작업이 정상적으로 끝났으면 UserService는 Connection의 Commit을 호출해서 트랜잭션을 완료시킨다.
    * 트랜잭션 저장소가 더 이상 Connection 오브젝트를 저장해두지 않도록 이를 제거한다.

* <u><b>트랜잭션 동기화 저장소는 작업 스레드마다 독립적으로 Connection 오브젝트를 저장하고 관리하기 때문에 다중 사용자를 처리하는 서버의 멀티스레드 환경에서도 충돌이 날 염려는 없다.</b></u>

* TransactionSynchronizationManager다

      스프링이 제공하는 트랜잭션 동기화 관리 클래스는 TransactionSynchronizationManager다.

* DataSourceUtils.getConnection()

      DataSourceUtils의 getConnection() 메소드를 통해 Connection 오브젝트를 생성하고
      트랜잭션 동기화에 사용하도록 저장소에 Connection을 바인딩한다.

* JdbcTemplate 동작 방식

    * 지금까지의 JdbcTemplate는 update()나 query() 같은 JDBC 작업의 템플릿 메소드를 호출하면 직접 Connection을 생성하고 종료하는 일을 모두 담당했다. 스스로 Connection을 생성해서 사용한다는 사실을 알고 있다.

    * 트랜잭션이 동기화가 되어 있는 채로 JdbcTemplate을 사용하면 JdbcTemplate의 작업에서 동기화 시킨 DB커넥션을 사용하게 된다.
    
    * JdbcTemplate은 미리 생성돼서 트랜잭션 동기화 저장소에 등록된 DB 커넥션이나 트랜잭션이 없는 경우에는 JdbTemplate이 직접 DB 커넥션을 만들고 트랜잭션을 시작해서 JDBC 작업을 진행한다. 반면에 트랜잭션 동기화를 시작해놓았다면 그때부터 실행되서 JdbcTemplate의 메소드에서는 직접 DB 커넥션을 만드는 대신 트랜잭션 동기화 저장소에 들어 있는 DB 커넥션을 가져와서 사용한다.

* JdbcTemplate 이란 ?

      스프링에서 트랜잭션 관리, try/catch/finally 작업 흐름 지원, SQLException 예외 변환 기능을 제공하는 API

* 글로벌 트랜잭션 이란 ?

      각 DB와 독립적으로 만들어지는 Connection을 통해서가 아니라 별도의 트랜잭션 관리자를 통해 트랜잭션을 관리하는 트랜잭션 방식
      
      자바는 JDBC 외에 이런 글로벌 트랜잭션을 지원하는 트랜잭션 매니저를 지원하기 위한 API인 JTA(Java Transaction API)을 제공한다.
      
* 추상화란 ?

      하위 시스템의 공통점을 뽑아내서 분리하는 것을 말한다. 그렇게 해서 하위 시스템이 어떤 것인지 알지 못해도, 바뀌어도
      일관된 방법으로 접근할 수 있다.
      
* JDBC 란 ?

      JDBC(Java Database Connectivity)는 자바에서 데이터베이스에 접속할 수 있도록 하는 자바 API이다. 
      
* 스프링의 트랜잭션 추상화 계층

    <img src = "https://user-images.githubusercontent.com/79847020/138890786-e0dd80aa-1de3-4d06-bb7a-84886d58e457.png" width="60%" height="60%">
    
* PlatformTransactionManager

      스프링이 제공하는 트랜잭션 경계설정을 위한 추상 인터페이스. JDBC의 로컬 트랜잭션을 이용한다면 PlatformTransactionManager
      을 구현한 DataSourceTransactionManager를 사용하면 된다.
      
    ```JAVA
    public class UserService {
        public void upgradeLevels() throws SQLException {
            TransactionStatus status = this.transactionManager.getTransaction(new DefaultTransactionDefinition());

            try {
                List<User> users = userDao.getAll();

                for (User user : users) {
                    if (canUpgradeLevel(user)) {
                        upgradeLevel(user);
                    }
                }

                this.transactionManager.commit(status);
            } catch (Exception e) {
                this.transactionManager.rollback(status);
                throw e;
            }
        }
    }
    ```

* 애플리케이션 로직의 종류에 따른 수펴ㅑㅇ적인 구분이든 로직과 기술이라는 수직적인 구분이든 모두 결합도가 낮으며 서로 영향을 주지 않고 자유롭게 확장될 수 있는 구조를 만들 수 있는 데는 스프링의 DI가 중요한 역할을 하고 있다. DI의 가치는 이렇게 관심 책임 성격이 다른 코드를 깔끔하게 분리하는데 있다.

* 단일 책임 원칙(Single Responsibility Principle)

      단일 책임 원칙은 하나의 모듈은 하나 가지 책임을 가져야 한다는 의미이다. 하나의 모듈이 바뀌는 이유는 한 가지여야 한다고 설명할 수 있다.
      단일 책임 원ㅇ칙을 잘 지키고 있다면 어떤 변경이 필요할 때 수정 대상이 명확해진다. 

* 서비스 추상화

      서비스 추상화라고 하면 트랜잭션과 같은 기능은 유사하나 사용 방법이 다른 로우레벨의 다양한 기술에 대해 추상 인터페이스와 일관성 있는
      접근 방법을 제공해주는 것을 말한다. 반면에 JavaMail의 경우처럼 테스트를 어렵게 만드는 건전하지 않은 방식으로 설계된 API를 사용할 
      때도 유용하게 쓰일 수 있다.
      
* 의존한다는 말은 종속되거나 기능을 사용한다는 의미이다.

* 테스트 대상인 오브젝트가 의존 오브젝트를 갖고 있기 때문에 발생하는 여러가지 테스트상의 문제점이 있다. 간단한 오브젝트의 코드를 테스트하는데 너무 거창한 작업이 뒤따르는 경우이다. 이럴 땐 UserDao의 경우처럼 테스트를 위해 간단한 환경으로 만들어주던가, UserService의 메일 발송 기능의 경우처럼 아예 아무런 일도 하지 않는 빈 오브젝트로 대치해주는 것이 해결책이다.
      
* 테스트 스텁(Test Stub)

      테스트용으로 사용되는 특별한 오브젝트, 대부분 테스트 대상인 오브젝트의 의존 오브젝트가 되는 것들이다. 테스트 환경을 만들기 위해
      테스트 대상이 되는 오브젝트의 기능에만 충실하게 수행하면서 빠르게 자주 테스트를 실행할 수 있도록 사용하는 이런 오브젝트를 통틀어
      테스트 대역이라고 부른다.

      대표적인 테스트 대역은 테스트 스텁이다. 테스트 스텁은 테스트 대상 오브젝트의 의존객체로서 존재하면서 테스트 동안에 코드가 정상적으로
      수행할 수 있도록 돕는 것을 말한다. 
      
* 목 오브젝트(Mock Object)

      테스트 대역 중에서 테스트 대상으로부터 전달받은 정보를 검증할 수 있도록 설계된 것을 목 오브젝트라고 한다.


      
      
      




