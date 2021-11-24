
스프링의 3대 기술 ?

IoC/DI, 서비스 추상화, AOP


## 6.1 트랜잭션 코드의 분리

### 6.1.1 메소드 분리

트랜잭션 경계설정과 비즈니스 로직이 공존하는 메소드

```JAVA
public void upgradeLevels() throws Exception {
    TransactionStatus status = 
          this.transactionManager.getTransaction(new DefaultTransactionDefinition());
    try {
 
        List<User> users = userDao.getAll();
        for(User user : users) {
            if (canUpgradeLevel(user)) {
                upgradeLevel(user);
            }
        }

        this.transactionManager.commit(status);
    } catch(Exception e) {
        this.transactionManager.rollback(status);
        throw e;
    }
}
```

비즈니스 로직 코드를 사이에 두고 트랜잭션 시작과 종료를 담당하는 코드가 앞뒤에 위치하고 있다. <br>
이 메소드에서 시작된 트랜잭션 정보는 트랜잭션 동기화 방법을 통해 DAO가 알아서 활용한다.<br>
이 두 코드는 성격이 다를 뿐 아니라 서로 주고받는 것도 없는 완벽하게 독립적인 코드이다.<br>

비즈니스 로직과 트랜잭션 경계설정의 분리
```JAVA
    public void upgradeLevels() throws Exception {
        TransactionStatus status = this.transactionManager.getTransaction(new DefaultTransactionDefinition());

        try {

            upgradeLevelsInternal();

            this.transactionManager.commit(status); //정상작업
        } catch (Exception e) {
            this.transactionManager.rollback(status); //예외발생
            throw e;
        }
    }

    private void upgradeLevelsInternal() {
        List<User> users = userDao.getAll();
        for (User user : users) {
            if(canUpgradeLevel(user)) {
                upgradeLevel(user);
            }
        }
    }
```

### DI 적용을 이용한 트랜잭션 분리

UserService를 인터페이스를 두고 UserService의 구현체 UserServiceImpl, UserServiceTx클래스를 구현하는 것이다. UserServiceImpl와 UserServiceTx는 각각 비즈니스 로직과 트랜잭션 경계설정의 책임을 맡고 있다.

### UserService 인터페이스 도입

UserService 인터페이스
```JAVA
interface UserService {
    void add(User user);
    void upgradeLevels();
}
```

트랜잭션 코드를 제거한 UserService 구현 클래스
```JAVA
public class UserServiceImpl implements UserService {
    private UserDao userDao;
    private MailSender mailSender;

    public void upgradeLevels() {
        List<User> users = userDao.getAll();
        for (User user : users) {
            if(canUpgradeLevel(user)) {
                upgradeLevel(user);
            }
        }
    }
...
}
```

### 분리된 트랜잭션 기능

위임 기능을 가진 UserSErviceTx클래스

```JAVA
public class UserServiceTx implements UserService {
    UserService userService;

    public void setUserService(UserService userService) {
        this.userService = userService;
    }

    public void add(User user) {
        if (user.getLevel() == null) user.setLevel(Level.BASIC);
        userService.add(user);
    }

    public void upgradeLevels() {
        userService.upgradeLevels();
    }
}
```

UserServiceTx는 사용자관리라는 비즈니스 로직을 전혀 갖기 않고 고스란히 다른 UserSErvice 구현 오브젝트에 기능을 위임한다.

UserServiceTx에 트랜잭션 경계설정이라는 부가적인 작업을 부여해보자. 

트랜잭션이 적용된 UserServiceTx

```JAVA
public class UserServiceTx implements UserService {
    UserService userService;
    PlatformTransactionManager transactionManager;

    public void setUserService(UserService userService) {
        this.userService = userService;
    }
    public void setTransactionManager(PlatformTransactionManager transactionManager) {
        this.transactionManager = transactionManager;
    }

    public void add(User user) {
        if (user.getLevel() == null) user.setLevel(Level.BASIC);
        userService.add(user);
    }

    public void upgradeLevels() {
        TransactionStatus status = this.transactionManager.getTransaction(new DefaultTransactionDefinition());
        try {
            userService.upgradeLevels();

            this.transactionManager.commit(status);
        } catch(RuntimeException ex) {
            this.transactionManager.rollback(status);
            throw ex;
        }
    }
}

```

### 트랜잭션 적용을 위한 DI설정

UserServiceTx가 UserSErviceImpl에 기능을 위임하기 때문에 다음과 같은 의존관계가 형성 됩니다.

Client -> UserServiceTx ->UserSErviceImpl

UserService빈이 의존하던 TransactionManager는 UserSErviceTx의 빈으로 <br>
userDao와 mailSender는 USerSErviceImpl 빈이 각각 의존하도록 프로퍼티 정보를 분리합니다.

```XML
    <bean id="userService" class="me.stead95.chap01.service.UserServiceTx">
        <property name="transactionManager" ref="transactionManager"></property>
    </bean>

    <bean id="userServiceImpl" class="me.stead95.chap01.service.UserServiceImpl">
        <property name="userDao" ref="userDao"></property>
        <property name="mailSender" ref="mailSender"></property>
    </bean>
```

### 트랜잭션 분리에 따른 테스트 수정

@Autowired는 기본적으로 타입을 이용해 빈을 찾지만 만약 타입으로 하나의 빈을 결정할 수 없는 경우에는 필드 이름을 이용해 빈을 찾는다.

UserServiceTest에는 수정해야 될 부분이 많다. 

UserServiceImpl 클래스의 비즈니스 기능을 테스트하기 위한 클래스이므로 TestUserService는 UserServiceImpl 를 상속받아야하고
MailSender는 UserServiceImpl에 주입해줘야 한다. 그리고 트랜잭션 기능이 포함된 테스트를 위해서는 UserServiceTx 클래스의 메소드를 호출하면서 테스트를 수행하도록 해야 한다.

```JAVA
public class UserServiceTest {
...
    @Autowired
    UserService userService;

    @Autowired
    UserServiceImpl userServiceImpl;

    static class TestUserService extends UserServiceImpl {
        private String id;

        private TestUserService(String id) {
            this.id = id;
        }

        protected void upgradeLevel(User user) {
            if (user.getId().equals(this.id)) throw new TestUserServiceExcepton();
            super.upgradeLevel(user);
        }
    }
...
    @Test
    @DirtiesContext //컨텍스트의 DI 설정을 변경하는 테스트라는 것을 알려준다.
    public void upgradeLevels() throws Exception {
        userDao.deleteAll();
        for (User user : users) userDao.add(user);

        MockMailSender mockMailSender = new MockMailSender();
        userServiceImpl.setMailSender(mockMailSender);
...


    @Test
    public void upgradeAllOrNothing() throws Exception {
        TestUserService testUserService = new TestUserService(users.get(3).getId());
        testUserService.setUserDao(this.userDao); // userDao를 수동 DI해준다.
        testUserService.setMailSender(this.mailSender);

        UserServiceTx userServiceTx = new UserServiceTx();
        userServiceTx.setUserService(testUserService);
        userServiceTx.setTransactionManager(transactionManager);

        userDao.deleteAll();

        for (User user : users) userDao.add(user);

        try { // TestUserService는 업그레이드 작업 중에 예외가 발생해야 한다. 정상종료라면 문제가 있으니 실패
            userServiceTx.upgradeLevels();
            testUserService.setMailSender(mailSender);
            fail("TestUserServiceException expected");
        } catch (TestUserServiceExcepton e) { //TestUserService가 던져주는 예외를 잡아서 계속 진행되도록 한다. 그 외의 예외라면 실패
        } catch (Exception e) {
            e.printStackTrace();
        }

        checkLevel(users.get(1), false); // 예외가 발생하기 전에 레벨이 변경이 있었던 사용자의 레벨이 처음 상태로 바뀌었나 확인
    }
...
```

### 트랜잭션 경계설정 코드 분리의 장점

첫째, 이제 비느지스 로직을 담당하고 있는 UserSErviceImpl의 코드를 작성할 때는 트랜잭션과 같은 기술적인 내용에는 전혀 신경쓰지 않아도 된다.
둘째, 비즈니스 로직에 대한 테스트를 손쉽게 만들어낼 수 있다는 것이다.

# 6.2 고립된 단위 테스트

## 6.2.1 복잡한 의존관계 속의 테스트

UserServiceTest가 테스트하고자 하는 대상인 UserService는 사용자 정보를 관리하는 비즈니스 로직의 구현 코드다. 따라서 테스트의 단위는 UserService 클래스여야 한다. 하지만 UserSErvice는 UserDao, TransactionManager, MailSender라는 세가지 의존 관계를 갖고 있다. 그래서 UserSErvice라는 테스트 대상이 테스트 단위인 것처럼 보이지만 사실은 그뒤의 의존관계를 따라 등장하는 오브젝트와 서비스, 환경 등이 모두 합쳐져 테스트 대상이 되는 것이다.

## 6.2.2 테스트 대상 오브젝트 고립시키기

테스트 대상이 환경이나 외부 서버 다른 클래스의 코드에 종속되고 영향을 받지 않도록 고립시킬 필요가 있다. 테스트를 의존 대상으로부터 분리해서 고립시키는 방법은 MailSender에 적용해봤던 대로 테스트를 위한 대역을 사용하는 것이다. 

### 테스트를 위한 UserServiceImpl 고립

고립 된 테스트가 가능하도록 UserService를 재구성해보면 두 개의 목오브젝트(MockUserDao, MockMailSender)에만 의존하는 구조가 될 것이다.

UserServiceImpl의 upgradeLevels() 메소드는 리턴 값이 없는 void형이다. 그 코드의 동작이 바르게 됐는지 확인하려면 결과가 남아 있는 DB를 직접 확인할 수 밖에 없다. 이럴 땐 테스트 대상인 UserServiceImpl과 그 협력 오브젝트인 UserDao에게 어떤 요청을 했는지를 확인하는 작업이 필요하다. 

### 고립된 단위 테스트 활용

고립된 단위 테스트 방법을 UserServiceTest의 upgradeLevels() 테스트에 적용해보자.

기존 upgradeLevels() 테스트
```JAVA
    @Test
    @DirtiesContext //컨텍스트의 DI 설정을 변경하는 테스트라는 것을 알려준다.
    public void upgradeLevels() throws Exception {
        userDao.deleteAll();
        for (User user : users) userDao.add(user);

        //메일 발송 결과를 테스트할 수 있도록 목 오브젝트를 만들어
        //userService의 의존 오브젝트로 주입해준다.
        MockMailSender mockMailSender = new MockMailSender();
        userServiceImpl.setMailSender(mockMailSender);

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

테스트 코드에서 두 가지 작업을 확인할 수 있습니다. 하나는 DB에서 결과를 확인하는 작업과 목 오브젝트를 통해 메일 발송이 있었는지를 확인하는 작업입니다.

### UserDao 목 오브젝트

실제 UserDao와 DB까지 직접 의존하고 있는 작업도 목 오브젝트를 만들어서 적용해보겠다. upgradeLevels()에서 UserDao를 사용하는 코드는 2가지 이다.

```JAVA
List<User> users = userDao.getAll();
...
userDao.updeate(user);
```

getAll() 에 대해서는 스텁으로 update()에 대해서는 목 오브젝트로서 동작하는 UserDao 타입의 테스트 대역이 필요하다.

UserServiceTest 전용일테니 스태틱 내부 클래스로 만들면 편리하다.

```JAVA
public class MockUserDao implements UserDao {

    //레벨 업그레이드 후보 User 오브젝트 목록
    private List<User> users;

    //업그레이드 대상 오브젝트를 저장해둘 목록
    private List<User> updated = new ArrayList();

    private MockUserDao(List<User> users) {this.users = users;}

    public List<User> getUpdated() {
        return this.updated;
    }

    @Override // 스텁기능 제공
    public List<User> getAll() {return this.users;}

    @Override // 목 오브젝트 기능 제공
    public void update(User user) { updated.add(user); }
    
    @Override
    public void add(User user) { throw new UnsupportedOperationException();}

    @Override
    public void deleteAll() { throw new UnsupportedOperationException();}

    @Override
    public User get(String id) { throw new UnsupportedOperationException();}
    
    @Override
    public int getCount() { throw new UnsupportedOperationException();}
}
```

MockUserDao는 밀 ㅣ준비된 테스트용 리스트를 메모리에 갖고 있다가 돌려주기만 하면 된다.

MockUserDao를 사용해서 만든 고립된 테스트
```JAVA
    @Test
    public void upgradeLevels() throws Exception {
        //고립된 테스트에서는 테스트 대상 오브젝트를 직접 생산 하면 된다.
        UserServiceImpl userServiceImpl = new UserServiceImpl();

        //목 오브젝트로 만든 UserDao를 직접 DI해준다.
        MockUserDao mockUserDao = new MockUserDao(this.users);
        userServiceImpl.setUserDao(mockUserDao);

        MockMailSender mockMailSender = new MockMailSender();
        userServiceImpl.setMailSender(mockMailSender);

        userServiceImpl.upgradeLevels();

        List<User> updated = mockUserDao.getUpdated();
        assertThat(updated.size(), is(2));
        checkUserAndLevel(updated.get(0), "joytouch", Level.SILVER);
        checkUserAndLevel(updated.get(1), "madnite1", Level.GOLD);

        List<String> request = mockMailSender.getRequests();
        assertThat(request.size(), is(2));
        assertThat(request.get(0), is(users.get(1).getEmail()));
        assertThat(request.get(0), is(users.get(3).getEmail()));
    }
```

원래 테스트 대상은 스프링 컨테이너에서 @Autowired를 통해 가져온 UserService 타입의 빈이었다. 컨테이너에서 가져온 UserService오브젝트는 DI를 통해서 많은 의존 오브젝트와 서비스 외부환경에 의존하고 있었다.

테스트하고 싶은 로직을 담은 클래스인 UserServiceImpl의 오브젝트를 직접 생성한다.

준비해둔 MockUserDao, MockMailSender 오브젝트를 사용하도록 수동 DI해주기만 하면 된다.

### 테스트 수행 성능의 향상

UserServiceImpl와 테스트를 도와주는 두 개의 목 오브젝트의 외에는 사용자 관리 로직을 검증하는 데 직접적으로 필요하지 않은 의존 오브젝트와 서비스를 모두 제거한 덕분이다.

고립된 테스트를 하면 테스트가 다른 의존 대상에 영향을 받을 경우를 대비해 복잡하게 준비할 필요가 없을 뿐만 아니라 테스트 수행 성능도 크게 향상 된다.

## 6.2.3 단위 테스트와 통합테스트

단위 테스트와 통합 테스트 중에서 어떤 방법을 쓸지는 어떻게 결정할 것인가 ? 

1. 항상 단위테스트를 먼저 고려한다.

2. 외부와의 의존관계를 모두 차단하고 필요에 따라 스텁이나 목 오브젝트 등의 테스트 대역을 이용하도록 테스트를 만든다.

3. 외부 리소스를 사용해야만 가능한 테스트는 통합 테스트로 만든다.

4. DAO는 DB까지 연동하는 테스트로 만드는 편이 효과적이다. DB에 테스트 데이터를 준비하고 DB에 직접 확인을 하는 등의 부가적인 작업이 필요하다.

5. DAO 테스트는 DB라는 외부 리소스를 사용하기 때문에 통합 테스트로 분류된다. 하지만 코드에서 보자면 하나의 기능 단위를 테스트하는 것이기도 하다. DAO를 충분히 검증해두고 DAO를 이용하는 코드는 스텁이나 목 오브젝트로 대체해서 테스트 할 수 있다.

6. 여러 개의 단위가 의존 관계를 가지고 동작할 때를 위한 통합테스트는 필요하다.

7. 단위 테스트를 만들기가 너무 복잡하다고 판단되는 코드는 처음부터 통합 테스트를 고려해 본다.

8. 스프링 테스트 컨텍스트 프레임워크를 이용하는 테스트는 통합 테스트다.

6.2.4 목 프레임워크

단위 테스트를 만들기 위해서는 스텁이나 목 오브젝트의 사용이 필수적이다. 대부분 의존 오브젝트를 필요로 하는 코드를 테스트하게 되기 때문이다.

목 오브젝트를 편리하게 작성하도록 도와주는 다양한 목 오브젝트 지원 프레임워크가 있다.

### Mockito 프레임워크

Mockito와 같은 목 프레임워크는 특징은 목 클래스를 일일이 준비해둘 필요가 없다는 점이다. 

UserDao 인터페이스를 구현한 테스트용 목 오브젝트는 다음과 같이 Mockito의 스태틱 메소드를 한 번 호출해주면 만들어진다.

```JAVA
UserDao mockUserDao = mock(UserDao.class);
```

여기에 먼저 getAll() 메소드가 불려올 때 사용자 목록을 리턴하는 스텁 기능을 추가해줘야 한다.
```JAVA
when(mockUserDao.getAll()).thenReturn(this.users);
```

테스트를 진행하는 동안 mockUserDao의 update() 메소드가 두 번 호출됐는지 확인하고 싶다면, 다음과 같인 검증 코드를 넣어주면 된다.

verify(mockUserDao, times(2)).update(any(User.class));

UserDao 인터페이스를 구현한 클래스를 만들 필요도 없고 리턴 값을 생성자를 통해 넣어줬다가 메소드 호출 시 리턴하도록 코드를 만들 필요도 없다.

Mockito 목 오브젝트는 다음의 네 단계를 거쳐서 사용하면 된다.

1. 인터페이스를 이용해 목 오브젝트를 만든다.
2. 목 오브젝트가 리턴 할 값이 있으면 이를 지정해준다. 메소드가 호출되면 예외를 강제로 던질 수도 있다.
3. 테스트 대상 오브젝트에 DI 해서 목 오브젝트가 테스트 중에 사용되도록 만든다.
4. 테스트 대상 오브젝트를 사용한 후에 목 오브젝트의 특정 메소드가 호출됐는지 어떤 값을 가지고 몇 번 호출됐는지를 검증한다.

Mockito를 적용한 테스트 코드

```JAVA
    @Test
    public void mockUpgradeLevels() throws Exception {
        UserServiceImpl userService = new UserServiceImpl();

        //다이내믹한 목 오브젝트 생성과 메소드의 리턴 값 설정
        //그리고 DI까지 세 줄이면 충분하다.
        UserDao mockUserDao = mock(UserDao.class);
        userServiceImpl.setUserDao(mockUserDao);

        //리턴 값이 없는 메소드를 가진 목 오브젝트는
        //더욱 간단하게 만들 수 있다.
        MailSender mockMailSender = mock(MailSender.class);
        userServiceImpl.setMailSender(mockMailSender);

        userServiceImpl.upgradeLevels();

        //목 오브젝트가 제공하는 검증 기능을 통해서 어떤 메소드가
        //몇 번 호출 됐는지 파라미터는 무엇인지 확인할 수 있다.
        verify(mockUserDao, times(2)).update(any(User.class));
        verify(mockUserDao, times(2)).update(any(User.class));
        verify(mockUserDao).update(users.get(1));
        assertThat(users.get(1).getLevel(), is(Level.SILVER));
        verify(mockUserDao).update(users.get(3));
        assertThat(users.get(3).getLevel(), is(Level.GOLD));

        ArgumentCaptor<SimpleMailMessage> mailMessageArg =
                ArgumentCaptor.forClass(SimpleMailMessage.class);
        verify(mockMailSender, times(2)).send(mailMessageArg.capture());
        List<SimpleMailMessage> mailMessages = mailMessageArg.getAllValues();
        assertThat(mailMessages.get(0).getTo()[0], is(users.get(1).getEmail()));
        assertThat(mailMessages.get(1).getTo()[0], is(users.get(3).getEmail()));
    }
```

UserDao,MailSender의 목 오브젝트를 정의하고 DI해준다.

userServiceImpl 메소드가 실행되는 동안 DI 해준 목 오브젝트의 메소드가 호출되면 자동으로 호출 기록이 남겨진다.<br>

getAll()처럼 미리 설정해둔 리턴 값이 있는 경우에는 그 값을 그대로 리턴해주기도 한다.
any()를 사용하면 파라미터의 내용은 무시하고 호출 횟수만 확인할 수 있다.
그 다음 목 오브젝트가 호출됐을 때의 파라미터를 하나씩 점검한다. 

verify(mockUserDao),update(users.get(1))은 users.get(1)을 파라미터로 update()가 호출된적이 있는지 확인한다.

MailSender의 경우는 ArgumentCaptor라는 것을 사용해서 실제 MailSender 목 오브젝트에 전달된 파라미터를 가져와 내용을 검증하는 방법을 사용했다.
파라미터를 직접 비교하기 보다는 파라미터의 내부 정보를 확인해야 경우 유용하다.

Mockito와 같은 목 오브젝트 지원 프레임워크 하나쯤은 익숙하게 사용할 수 있도록 학습해두자.

# 6.3 다이내믹 프록시와 팩토리 빈

## 6.3.1 프록시와 프록시 패턴 데코레이턴 패턴

### 프록시(Proxy)

클라이언트가 사용하려고 하는 실제 대상인 것처럼 대리해서 요청을 받아주는 오브젝트

##### 타깃(target) - 프록시를 통해 최종적으로 요청을 위임받아 처리하는 실제 오브젝트, 실체(real subject)

프록시의 특징은 타깃과 같은 인터페이스를 구현했다는 것과 프록시가 타깃을 제어할 수 있는 위치에 있다는 것이다.

사용목적에 따라 두가지로 구분할 수 있습니다.

첫째, 클라이언트가 타깃에 접근하는 방법을 제어하기 위해서다.
둘째, 클라이언트가 타깃에 부가적인 기능을 부여해주기 위해서다.

목적에 따라서 디자인 패턴에서는 다른 패턴으로 구분한다.

### 데코레이터 패턴

데코레이터 패턴은 타깃에 부가적인 기능을 런타임시 다이내믹하게 부여해주기 위해 프록시를 사용하는 패턴을 말한다.

다이내믹하게 기능을 부가한다는 의미는 컴파일 시점, 즉 코드상에서는 어떤 방법과 순서로 프록시와 타깃을 연결되어 사용되는지 정해져 있지 않다는 뜻이다.

데코레이터 패턴에서는 프록시가 꼭 한개로 제한되지 않는다. 프록시가 직접 타깃을 사용하도록 고정시킬 필요도 없다.

자바 IO 패키지의 InputStream과 OutputStream 구현 클래스는 데코레이터 패턴이 사용된 대표적인 예다. 

InputStream이라는 인터페이스를 구현한 타깃인 FileInputStream에 버퍼 읽기 기능을 제공해주는 BufferedInputStream이라는 데코레이터 패턴을 적용한 예이다.

InputStream is = new BufferedInputStream(new FileInputStream("a.txt"));

### 프록시 패턴

프록시를 사용하는 방법 중에서 타깃에 대한 접근 방법을 제어하려는 목적을 가진 경우를 가리킨다.

타깃 오브젝트를 필요한 시점까지 생성할 필요가 없을 때 실제 타깃 오브젝트 대신 프록시를 넘겨주는 것이다. 

프록시의 메소드를 통해 타깃을 사용하려고 시도하면 그때 프록시를 넘겨주는 것이다. 

원격 오브젝트를 이용하는 경우에도 프록시를 사용하면 편리하다.

다른 서버에 존재하는 오브젝트를 사용해야 한다면 원격 오브젝트에 대한 프록시를 만들어두고 클라이언트는 마친 로컬에 존재하는 오브젝트를 쓰는 것처럼 프록시를 사용하게 할 수 있다.

프록시는 클라이언트의 요청을 받으면 네트워크를 통해 원격의 오브젝트를 실행하고 결과를 받아서 클라이언트에게 돌려준다. 클라이언트로 하여금 원격 오브젝트에 대한 접근 방법을 제공해주는 프록시 패턴의 예라고 볼 수 있다.

또는 특별한 상황에서 타깃에 대한 접근권한을 제어하기 위해 프록시 패턴을 사용할 수 있다. 프록시의 특정 메소드를 사용하려고 하면 접근이 불가능하다고 예외를 발생시키면 된다.

프록시 패턴은 타깃의 기능 자체에는 관여하지 않으면서 접근하는 방법을 제어해주는 프록시를 이용하는 것이다.

## 6.3.2 다이내믹 프록시

프록시는 기존 코드에 영향을 주지 않으면서 타깃의 기능을 확장하거나 접근 방법을 제어할 수 있는 유용한 방법이다.

자바에는 java.lang.reflect 패키지 안에 프록시를 손쉽게 만들 수 있도록 지원해주는 클래스들이 있다. 

### 프록시의 구성과 프록시 작성의 문제점

프록시는 다음의 두 가지 기능으로 구성된다. 

1. 위임 : 타깃과 같은 메소드를 구현하고 있다가 메소드가 호출되면 타깃 오브젝트로 위임한다.

2. 부가작업 : 지정된 요청에 대해서는 부가기능을 수행한다.

### 리플렉션

`리플렉션` : 리플렉션은 구체적인 클래스 타입을 알지 못해도, 클래스 메타정보 가져와  클래스의 메소드, 타입, 변수들에 접근할 수 있도록 해주는 자바 API

자바의 모든 클래스는 그 클래스 자체의 구성정보를 담은 Class 타입의 오브젝트를 하나씩 갖고 있다. '클래스이름.class'라고 하거나 오브젝트의 getClass() 메소드를 호출하면 클래스 정보를 담은 Class 타입의 오브젝트를 가져올 수 있다. 클래스 코드에 대한 메타정보를 가져오거나 오브젝트를 조작할 수 있다.

리플렉션 API 중에서 메소드에 대한 정의를 담은 Method라는 인터페이스를 이용해 메소드를 호출하는 방법을 살펴보자.

Method lengthMethod = String.class.getMethod("length");

스트링이 가진 메소드 중에서 "length"라는 이름을 갖고 있고 파라미터는 없는 메소드의 정보를 가져오는 것이다.

이를 이용해 특정 오브젝트의 메소드를 실행 할 수 있다. invoke(), 메소드를 실행시킬 대상 오브젝트(obj.)와 파라미터 목록(args)을 받아서 메소드를 호출한 뒤 그 결과를 Object 타입으로 돌려준다.

public Object invoke(Object obj, Object... args)

Method lengthMethod = String.class.getMethod("length");
int length = lengthMethod.invoke(name);

리플렉션 학습 테스트

```JAVA
public class ReflectionTest {
    @Test
    public void invokeMethod() throws Exception {
        String name = "Spring";

        // length()
        assertThat(name.length(), is(6));

        Method lengthMethod = String.class.getMethod("length");
        assertThat((Integer)lengthMethod.invoke(name), is(6));

        // charAt()
        assertThat(name.charAt(0), is('S'));

        Method charAtMethod = String.class.getMethod("charAt", int.class);
        assertThat((Character) charAtMethod.invoke(name, 0), is('S'));
    }
}
```
String 클래스의 length() 메소드와 charAt() 메소드를 코드에서 직접 호출하는 방법과 Method를 이용해 리플렉션 방식으로 호출하는 방법을 비교한 것이다.

### 프록시 클래스

다이내믹 프록시를 이용한 프록시를 만들어 보자.

Hello 인터페이스
```JAVA
public interface Hello {
    String sayHello(String name);
    String sayHi(String name);
    String sayThankYou(String name);
}
```
타깃 클래스
```JAVA
public class HelloTarget implements Hello {
    public String sayHello(String name) {
        return "Hello " + name;
    }

    public String sayHi(String name) {
        return "Hi " + name;
    }

    public String sayThankYou(String name) {
        return "Thank You " + name;
    }
}

```
프록시 클래스
```JAVA
public class HelloUppercase implements Hello {
    Hello hello;

    public HelloUppercase(Hello hello) {
        this.hello = hello;
    }

    public String sayHello(String name) {
        return hello.sayHello(name).toUpperCase();
    }

    public String sayHi(String name) {
        return hello.sayHi(name).toUpperCase();
    }

    @Override
    public String sayThankYou(String name) {
        return hello.sayThankYou(name).toUpperCase();
    }
}
```
클라이언트 역할
```JAVA
    @Test
    public void simpleProxy() {
        Hello hello = new HelloTarget();
        assertThat(hello.sayHello("Toby"), is("Hello Toby"));
        assertThat(hello.sayHi("Toby"), is("Hi Toby"));
        assertThat(hello.sayThankYou("Toby"), is("Thank You Toby"));

        Hello proxiedHello = new HelloUppercase(new HelloTarget());
        assertThat(proxiedHello.sayHello("Toby"), is("HELLO TOBY"));
        assertThat(proxiedHello.sayHi("Toby"), is("HI TOBY"));
        assertThat(proxiedHello.sayThankYou("Toby"), is("THANK YOU TOBY"));
    }
```

정상적으로 프록시 클래스가 동작하는 것을 확인할 수 있다. 하지만 이 프록시는 프록시 적용의 일반적인 문제점 두가지를 모두 갖고 있다.

인터페이스의 모든 메소드를 구현해 위임하도록 코드를 만들어야 하며, 부가기능인 리턴 값을 대문자로 바꾸는 기능이 모든 메소드에 중복돼서 나타난다.

### 다이내믹 프록시 적용

다이내믹 프록시는 프록시 팩토리에 의해 런타임 시 다이내믹 하게 만들어지는 오브젝트다. 다이내믹 프록시 오브젝트는 타깃의 인터페이스와 같은 타입으로 만들어진다. 프록시 팩토리에게 인터페이스 정보만 제공해주면 해당 인터페이스를 구현한 클래스의 오브젝트를 자동으로 만들어주기 때문에 인터페이스를 모두 구현해가면서 클래스를 정의하는 수고를 덜 수 있다.

부가기능은 프록시 오브젝트와 독립적으로 InvocationHandler를 구현한 오브젝트에 담는다. InvocationHandler 인터페이스는 다음과 같은 메소드 한 개만 가진 간단한 인터페이스다.

public Object invoke(Object proxy, Method method, Object[] args)

invoke() 메소드는 리플렉션의 Method 인터페이스를 파라미터로 받는다. 메소드를 호출할 때 전달되는 파라미터도 args로 받는다. 다이내믹 프록시 오브젝트는 클라이언트의 모든 요청을 리플렉션 정보로 변환해서 InvocationHandler 구현 오브젝트의 invoke() 메소드로 넘기는 것이다. 타깃 인터페이스의 모든 메소드 요청이 하나의 메소드로 집중되기 때문에 중복되는 기능을 효과적으로 제공할 수 없다.






