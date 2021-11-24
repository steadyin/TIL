4장에서는 JdbcTemplate을 대표로 하는 스프링의 데이터 액세스 기능에 담겨 있는 예외 처리와 관련된 접근방법에 대해 알아본다.

# 4.1 사라진 SQLException

JdbcTemplate을 사용 후 메소드 선언 문에 throws SQLException이 사라졌음을 알 수 있다. 이 SQLException은 어디로 갔을까?

```JAVA
  public void deleteAll() throws SQLException {
    this.jdbcTemplate.update("delete from users");
  }
```

## 4.1.1 초난감 예외처리

### 예외 블랙홀

```JAVA
  try {
    ...
  }
  catch(SQLException e) {}
```

예외를 잡고는 아무것도 하지 않는다. 

예외 발생을 무시하고 정상적인 상황인 것처럼 다음 라인으로 넘어가겠다는 분명한 의도가 있는게 아니라면 절대 만들어서는 안되는 코드이다.

catch문에 System.out.println(e) 나 e.printStackTrace() 같은 출력만 해주는것도 마찬가지이다.

예외는 처리되어야 한다. 예외를 처리할 때 핵심원칙은 모든 예외는 적절하게 복구되든지 아니면 작업을 중단시키고 운영자 또는 개발자에 분명하게 통보돼야 한다.

취할 방법이 없다면 메소드 선언문에 throws SQLException을 선언해서 메소드 밖으로 예외처리를 책임을 전가하는 것이 나은 선택이다.

### 무의미하고 무책임한 throws

예외 관련 컴파일 에러 때문에 메소드 선언에 throws Exception을 기계적으로 붙이는 무책임한 개발자도 있다. 

메소드 선언에서는 의미 있는 정보를 얻을 수 없다. 예외적인 상황이 발생한 것인지, 습관적으로 복사 붙여놓은 것인지 알수 없다.

결과적으로 적절한 처리를 통해 복구될 수 있는 예외 상황도 제대로 다룰 수 있는 기회를 박탈당한다.

## 4.1.2 예외의 종류와 특징

그렇다면 예외를 어떻게 다뤄야 할까? 예외를 다루는게 가장 큰 이슈는 체크 에외(checked exception)라고 불리는 명시적인 처리가 필요한 예외를 사용하고 다루는 방법이다.

자바에서 throws를 통해 발생시킬 수 있는 예외는 크게 3가지가 있다.

* Error

java.lang.Error 클래스의 서브 클래스들이다.

시스템에 비정상적인 상황이 발생했을 경우 사용된다. 주로 자바 VM에서 발생하는 경우가 많다.

시스템 레벨에서 발생하는 에러로 애플리케이션 코드에서 신경쓰지 않아도 된다. 

대표적으로 OutOfMemoryError 나 ThreadDeath가 있다.

![](https://github.com/steadyin/steadyin/blob/main/wiki/img/exception.png?raw=true)

* Exception, 체크 예외

애플리케이션 코드 작업 중에 예외 상황이 발생했을 경우 대응하기 위한 예외이다. 

체크 예외는 Excpetion 클래스의 서브클래스 이면서 RuntimeException 클래스를 상속하지 않은 것이다. 

체크 예외는 반드시 예외를 처리하는 코드(catch, throws)를 함께 작성해야 되고 처리하지 않으면 컴파일 에러가 발생한다. 

* RuntimeException, 언체크/런타임 예외

java.lang.RuntimeException 클래스를 상속한 예외들은 명시적인 예외처리(catch, throws)를 강제하지 않기 때문에 언체크 예외라고 불린다.  

또는 상위 클래스 이름을 따서 런타임 예외라고도 한다.

코드에서 피할 수 있지만 개발자가 부주의 해서 발생할 수 있는 경우에 발생하도록 만든 것이 런타임 예외이다.

예를 들면 NullPointerException과 IllegalArgumentException 이 있다.

NullPointerException - 할당하지 않은 레퍼런스 변수를 사용하려고 시도했을 때 발생

IllegalArgumentException - 허용 되지 않은 값을 사용해서 메소드 호출 할 때 발생

## 4.1.3 예외처리 방법

체크 예외가 예외처리를 강제하는 것 때문에 예외 블랙홀이나 무책임한 throws 같은 코드가 남발됐다. 

일단 예외를 처리하는 일반적인 방법을 살펴보고 효과적인 예외처리 전략을 생각해보겠다.

### 예외 복구

첫번째 예외처리 방법은 예외상황을 파악하고 문제를 해결해서 정상 상태로 돌려 놓는 것이다. 

예외처리 코드를 강제하는 체크 예외들은 예외를 어떤 식으로 복구할 가능성이 잇는 경우에 사용된다.

```JAVA
int maxretry = MAX_RETRY;
while(maxretry -- > 0) {
  try {
    ...          //예외가 발생할 가능성이 있는 시도
    return;      //작업 성공
  } catch(SomeException e) {
                 //로그출력, 정해진 시간만큼 대기
  } finally {
                 //리소스 반납, 정리작업
  }
}
throw new RetryFailedException(); // 최대 재시도 횟수를 넘기면 직접 예외 발생
```

### 예외처리 회피

두번째 방법은 예외처리를 자신이 담당하지 않고 자신을 호출한 쪽으로 던져버리는 것이다. 

throws 문으로 선언해서 호출한쪽으로 전가하거나 catch문으로 일단 예외를 잡은 후 로그에 남기고 다시 예외를 던지는 것이다.

``` JAVA
public void add() throws SQLException {
  //JDBC API
}
```
``` JAVA
public void add() throws SQLException {
  try {
    //JDBC API
  }
  catch(SQLException e) {
    //로그 출력
    throw e;
  }
}
```

JdbcTemplate 콜백 오브젝트의 메소드는 SQLException에 대한 예외를 회피하고 템플릿 레벨에서 처리하도록 던져준다.

예외를 회피하는 것은 다른 오브젝트에게 예외처리 책임을 분명히 지게 하거나, 사용하는 쪽에서 예외를 다루는게 최선의 방법이라는 분명한 확신이 있어야 한다.

### 예외 전환(Exception Translation)

마지막으로 예외를 처리하는 방법은 예외 전환 하는 것이다. 

예외 전환은 2가지 목적으로 사용된다.

첫째, 내부에서 발생한 예외가 예외상황에 대한 적절한 의미를 부여해주지 경우 의미를 분명하게 해줄 수 있는 예외로 바꿔주기 위해서이다.

예를 들어 사용자를 등록하려고 시도했을 때 중복되는 경우 SQLException을 그대로 throw하면 서비스 계층에서는 왜 예외가 발생했는지 알 방법이 없다.

그럴 땐 DAO에서 DuplicateUserIdException 같은 예외로 바꿔서 던져주는것이 더 명시적이다.

특정 기술의 정보를 해석하는 코드를 비즈니스 로직을 담은 서비스 계층에 두는 건 관심사의 분리에도 어긋나기 때문에 내부에서 해석해서 던지는 것이 더 합리적이다.

```JAVA
public void add(User user) throws DuplicateUserIdException, SQLException {
  try {
    //JDBC를 이용해 user정보를 DB에 추가하는 코드 또는
    //그런 기능을 가진 다른 SQLException을 던지는 메소드를 호출하는 코드
  } cach(SQLException e) {
    //ErrorCode가 MySQL의 'Duplicate Entry(1062)'이면 예외 전환
    if(e.getErrorCode()==MysqlErrorNumbers.EP_DUP_ENTRY)
      throw DuplicateUserIdException();
    else 
      throw e; //그 외에 경우는 SQLException 그대로
  }
}
```

예외 전환 시 원래 발생한 예외를 담아서 중첩 예외(nested exception)로 만드는 것이 좋다.

getCause() 메소드를 이용해서 처음 발생한 예외가 무엇인지 확인할 수 있다.

중첩예외1 - 생성자
```JAVA
  catch(SQLException e) {
    throw DuplicateUserIdException(e);
  }
```
중첩예외2 - initCause()
```JAVA
  catch(SQLException e) {
    throw DuplicateUserIdException().initCause(e);
  }
```

둘째, 예외를 처리하기 쉽고 단순하게 만들기 위해 포장하는 것이다. 

의미를 명확하게 하려고 다른 예외로 전환하는 것이 아니라, 비즈니스 로직으로 볼 때 의미 있는 예외이거나 복구 가능한 예외가 아닌 경우,

예외처리(catch, throws)를 하기보다는 런타임 예외로 바로 포장해서 던지는 편이 낫다.

반대로 비즈니스 로직상 의미가 있거나, 복구 가능한 예외라면 체크 예외를 사용한 후 적절한 대응을 하거나 복구작업을 하는 것이 맞다.

대부분 서버 환경에서는 애플리케이션 코드에서 처리하지 않고 전달된 예외들을 처리하는 기능을 제공한다.

어차피 복구하지 못할 예외라면 런타임 예외로 포장해서 던져서 예외처리 서비스를 이용하거나 관리자에게 통보하는 것이 나은 대응이다.

## 4.1.4 예외처리 전략

지금까지 살펴본 예외 종류와 처리 방법 등을 기준으로 일관된 예외처리 전략을 정리해보자.

### 런타임 예외의 보편화

일반적으로 체크 예외가 일반적인 예외를 다루고, 언체크 예외는 시스템 장애, 프로그램상의 오류에 사용한다고 했다.
문제는 체크 예외는 복구할 가능성이 조금이라도 있는 예외적인 상황이기 때문에 자바에서 catch 블록이나 throws 선언을 강제하고 있다는 점이다.

자바 엔터프라이즈 서버환경은 수많은 요청이 동시에 발생하고 각 요청이 독립적인 작업으로 취급된다. 하나의 요청을 처리하는 중에 예외가 발생하면 해당 작업만 중단시키면 그만이다. 그래서 애플리케이션 코드에서 대응이 불가능한 체크 예외라면 런타임 예외로 전환해서 던지는 것이 좋은 선택이다.

자바의 환경이 서버로 이동하면서 체크 예외의 활용도와 가치는 떨어지고 의미 없는 throws Exception이 난무할 가능성이 커졌다.

따라서 표준 스펙 또는 오픈 소스 프레임워크에서 API가 발생시키는 예외를 체크 예외 대신 언체크 예외로 정의하는 것이 일반화되고 있다. 

대게는 복구 불가능한 상황이고 보나마나 RuntimeException 등으로 포장해서 던져야 할 테니 아예 API 차원에서 런타임 예외를 던지도록 만드는 것이다. 

### add() 메소드 예외처리

add() 메소드는 DuplicatedUserIdException과 SQLException 두 가지 체크 예외를 던지게 되어 있다. 

JDBC코드에서는 SQLException이 발생할 수 있는데 그 원인이 ID중복이라면 좀 더 의미있는 DuplicatedUserIdException으로 전환해준다.

SQLException은 복구 불가능한 예외이므로 잡아봤자 처리할 것도 없으므로 런타임 예외로 포장해 던져 그 밖의 메소드들이 신경 쓰지 않게 해주는 편이 낫다.

DuplicatedUserIdException은 어디에서든 잡아서 처리할 수 있다면 체크 예외로 만들지 않고 런타임 예외로 만드는게 낫다.

대신 add() 메소드는 명시적으로 DuplicatedUserIdException을 던진다고 선언해야 한다. 그래야 add() 메소드를 사용하는 개발자에게 의미 있는 정보를 전달할 수 있다.

DuplicatedUserIdException을 작성해보자. 

필요하면 언제든 잡아서 처리할 수 있도록 별도의 예외로 정의하기는 하지만 필요없다면 신경 쓰지 않아도 되도록 런타임 예외로 만든다.

중첩 예외를 만들 수 있도록 생성자를 추가해해주는 것을 잊지 말자.

```JAVA
public class DuplicateUserIdException extends RuntimeException {
  public DuplicateUserIdException(Throwable cause) {
    super(cause);
  }
}
```

이제 SQLException은 런타임예외로 처리하고 예외 처리를 하지 않을 것이므로 메소드 선언부 throws에서 제거한다.
(코드에서 대응방법이 없으므로)

반면에 DuplicateUserIdException은 런타임예외로 처리하지만 외부에서 예외를 처리할 수 있도록 메소드 선언부에 throws 해준다.
(코드에서 대응방법이 있으므로)

```JAVA
public void add() throws DuplicateUserIdException {
  try {
    //JDBC를 이용해 user 정보를 DB에 추가하는 코드 또는
    //그런 기능이 있는 다른 SQLException을 던지는 메소드를 호출하는 코드
  }
  catch(SQLException e) {
    if(e.getErrorCode()==MysqlErrorNumbers.ER_DUP_ENTITY) 
      throw new DuplicateUserIdException(e);  //예외 전환
    else throw new RuntimeException(e);       //예외 포장
  }
}
```

이제 이 add() 메소드를 사용하는 오브젝트는 SQLException을 처리하기 위해 불필요한 throws 선언을 할 필요는 없으면서 필요한 경우 아이디 중복 상황을 처리하기 위해 DuplicatedUserIdException을 이용할 수 있다. 이제 DuplicatedUserIdException이 발생한 경우라면 사용자에게 중복 메시지를 출력 후 추천아이디를 제공하는 코드를 손쉽게 작성할 수 있다.

### 애플리케이션 예외

애플리케이션 자체의 로직에 의해 의도적으로 발생시키고 반드시 catch해서 무엇인가 조치를 취하도록 요구하는 애플리케이션 예외도 있다.
예외상황에서는 비즈니스적인 의미를 띤 예외를 던지도록 만드는 것이다. 이 때 사용하는 예외는 의도적으로 체크 예외로 생성한다.
예를 들면 잔고 부족과 같은 예외 상황에서 비즈니스적인 의미를 띤 InsufficientBalanceExcpetion등을 던진다. 

이런 기능을 담은 메소드를 설계하는 방법은 2가지가 있다.

첫째, 정상적인 출금을 처리햇을 경우와 잔고 부족이 발생했을 경우에 각각 다른 종류의 리턴 값을 돌려주는 것이다.
하지만 이 방법은 리턴 값을 코드화해서 잘 관리해야하고 리턴 값맞다 맞는 처리를 해주기 위해 조건문이 자주 등장할 수 밖에 없다.

둘째, 정상적인 흐름을 따르는 코드는 그대로 두고 예외 상황에서는 비즈니스적인 의미를 띤 예외를 던지도록 만드는 것이다. 
이 때 사용하는 예외는 의도적으로 체크 예외로 만든다. 잊지않고 예외상황에 대한 로직을 구현하도록 강제해주기 위함이다.

예금을 인출해서 처리하는 코드를 정상 흐름으로 만들어두고 잔고 부족을 예외로 만들어 처리하는 코드이다.
애플리케이션 예외인 InsufficientBalanceException을 만들 때는 예외상황에 대한 상세한 정보를 담고 있도록 설계할 필요가 있다.

```JAVA
try {
  BigDecimal balance = account.withdraw(amount);
  ...
  //정상적인 처리 결과를 출력하도록 진행
}
catch(InsufficientBalanceException e) { //체크 예외
  //InsufficientBalanceException에 담긴 인출 가능한 잔고금액 정보를 가져옴
  BigDecimal availFunds = e.getAvailFunds();
  ..
  // 잔고 부족 안내 메시지를 준비하고 이를 출력하도록 진행
}
```
## 4.1.5 SQLException은 어떻게 됐나?

SQLException은 99%가 코드레벨에서는 복구할 방법이 없다. 프로그램 오류, 개발자의 부주의, 통제할 수 없는 외부상황 때문에 발생하는 것이다.

앞서 살펴봤듯이 의미 없는 예외처리 보다는 언체크예외로 전환하는 것은 나은 선택이다.

JdbcTemplate 템플릿과 콜백 안에서 발생하는 모든 SQLException은 런타임 예외인 DataAccessException으로 포장해서 던져준다. 

UserDao에서 꼭 필요한 경우만 DataAccessException을 잡아서 처리하면 되고 그 외의 경우에는 무시하면 된다. 

그래서 DAO메소드에서 SQLException이 모두 사라진 것이다. 

그밖에도 스프링 API 메소드에 정의되어 있는 대부분의 예외는 런타임 예외다. 발생 가능한 예외가 있다고 하더라도 이를 처리하도록 강제하지 않는다. 

# 4.2 예외 전환

예외를 다른 것으로 바꿔서 던지는 예외 전환의 목적은 두 가지라고 설명했다. 

하나는 앞에서 적용해본 것처럼 런타임 예외로 포장해서 필요하지 않은 catch/throws를 줄여주는 것이고 다른 하나는 로우레벨의 예외를 좀 더 의미 잇고 추상화 된 예외로 바꿔서 던져주는 것이다. 

스프링 JdbcTemplate이 던지는 DataAccessException은 런타임 예외로 SQLException을 포장해주는 역할을 한다.

따라서 복구 불가능한 예외인 SQLException에 대해 애플리케이션 레벨에서는 신경 쓰지 않도록 해준다.

또한 DataAccessException은 SQLException에 담긴 다루기 힘든 상세한 예외정보를 의미 있고 일관성 있는 예외로 전환해서 추상화 해주려는 용도로 쓰이기도 한다. 

## 4.2.1 JDBC의 한계

`JDBC`는 `자바를 이용해 DB에 접근하는 방법을 추상화된 API 형태로 정의`해놓고 `각 DB 업체가 JDBC 표준을 따라 만들어진 드라이버를 제공`하게 해준다.

내부 구현은 DB마다 다르겠지만 JDBC의 `Connection`, `Statement`, `ResultSet` 등의 표준 인터페이스를 통해 그 기능을 제공해준다. 

그래서 자바 개발자들은 표준화 된 JDBC의 API만 익숙해지면 DB의 종류에 상관없이 일관된 방법으로 프로그램을 개발할 수 있다.

하지만 현실적으로 DB를 자유롭게 바꾸어 사용할 수 있는 DB 프로그램을 작성하는 데는 두 가지 걸림돌이 있다.

### 비표준 SQL

첫째, JDBC코드에서 사용하는 SQL이다. DB별 비표준 문법과 기능이 존재하기 때문이다.

DB별 최적화 기법이 다르고 DB별 특징이 있기 때문에 비표준 SQL문장이 생성된다.

비표준 SQL은 결국 DAO를 특정 DB에 종속적인 코드가 되도록 만든다.

해결책으로는 호환 가능한 표준 SQL만 사용하는 방법과 DB별 DAO를 만들거나 SQL을 외부에 독립시켜서 관리하는 방법이 있다.

### 호환성 없는 SQLException의 DB 에러 정보

두번째 문제는 SQLException이다. 

문제는 DB마다 에러의 종류와 원인도 제 각각이라는 점이다. 그래서 JDBC는 데이터 처리 중 발생하는 다양한 예외를 그냥 SQLException 하나에 모두 담아버린다.

예외가 발생한 원인은 SQLException 안에 담긴 에러코드와 SQL 상태정보를 참조봐야 한다. 

그런데 SQLException의 getErrorCode()로 가져 올 수 있는 에러코드가 DB별로 다르다. DB 벤더가 정의한 고유한 에러 코드를 사용하기 때문이다. 

예를 들면 다음 소스 코드는 MySQL에 종속된다.

```JAVA
 if(e.getErrorCode()==MysqlErrorNumbers.ER_DUP_ENTRY) { ...
```

그래서 SQLException은 예외가 발생했을 때의 DB상태를 담은 SQL 상태정보를 부가적으로 제공한다. `getSQLState()` 메소드로 예외상황에 대한 상태정보를 가져올 수 있다. 

이 상태정보는 DB별로 달라지는 에러 코드를 대신할 수 있도록 OpenGroup의 XOPEN SQL 스팩에 정의된 SQL 상태 코드를 따르도록 되어 있다. 

그런데 문제는 DB의 JDBC 드라이버에서 SQLException을 담을 상태코드를 정확하게 만들어 주지 않는 다는 점이다. 

결론만 말하면 SQL 상태 코드를 믿고 결과를 파악하도록 코도를 작성하는 것은 신뢰할 수 없다.

결국 호환성 없는 에러코드와 표준을 따르지 않는 상태코드를 가진 SQLException만으로 DB에 독립적인 유연한 코드를 작성하는 건 불가능에 가깝다.

## 4.2.2 DB 에러코드 매핑을 통한 전환

두가지 해결 문제중 SQL과 관련된 문제는 뒤에서 다루기로 하고 SQLException의 비표준 에러코드와 SQL 상태 코드에 대한 해결책을 알아보자.

SQLException에 담긴 SQL 상태 코드는 신뢰할 만한 게 아니므로 더 이상 고려하지 않겠다. 

DB 벤더 별로 만들어 유지해오고 있는 DB 전용 코드가 더 정확한 정보라고 판단된다.

해결 방법은 DB별 에러 코드를 참고해서 발생한 예외의 원인이 무엇인지 해석해주는 기능을 만드는 것이다.

키 값이 중복돼서 중복 오류가 발생하는 경우에 MySQL은 1062, 오라클은 1, DB2는 -803이라는 에러 코드를 받게 된다.

이 에러 코드 값을 DuplicateKeyException이라는 의미가 드러나는 예외로 전환할 수 있다. 

스프링은 DataAccessException이라는 SQLException을 대체할 수 있는 런타임 예외를 정의하고 있을 뿐만 아니라 DataAccessException의 서브 클래스로 세분화 된 예외클래스를 정의하고 있다.

여기에는 BadSqlGrammarException, DataAccessResourceFailureException, DataIntegrityViolationException, DuplicatedKeyExceptione 등이 포함된다.

스프링은 DB별로 에러 코드를 분류해서 스프링이 정의한 예외 클래스와 매핑해놓은 에러 코드 매핑 정보 테이블을 만들어두고 이를 이용한다.

```XML
<bean id= "Oracle" class="org.springframework.jdbc.support.SQLErrorCodes">
  <property name="badSqlGrammarCodes"> <!-- 예외 클래스 종류 -->
    <value>900,903,904,917....</value> <!-- DB 에러 코드 --!>
...
```

JdbcTemplate는 DB의 에러코드를 DataAccessException 계층 구조의 클래스 중 하나로 매핑해준다. 

DB 메타정보를 참고해서 DB별로 매핑정보를 참고해서 적절한 예외클래스를 선택해준다.

중복 키 에러를 따로 분류해서 예외처리를 해줬던 add() 메소드가 JdbcTemplate을 사용하도록 바꾸면 다음과 같이 간단해진다.

```JAVA
public void add(User user) throws DuplicateUserIdException, SQLException {
  try {
    //JDBC를 이용해 user정보를 DB에 추가하는 코드 또는
    //그런 기능을 가진 다른 SQLException을 던지는 메소드를 호출하는 코드
  } cach(SQLException e) {
    //ErrorCode가 MySQL의 'Duplicate Entry(1062)'이면 예외 전환
    if(e.getErrorCode()==MysqlErrorNumbers.EP_DUP_ENTRY)
      throw DuplicateUserIdException();
    else 
      throw e; //그 외에 경우는 SQLException 그대로
  }
}
```
```JAVA
public void add(User user) throws DuplicateKeyException {
  // JdbcTemplate을 이용해 User를 add하는 코드
}
```
JdbcTemplate은 체크 예외인 SQLException을 런타임 예외인 DataAccessException 계층구조의 예외로 포장해주기 때문에 add() 메소드에는 예외 포장을 위한 코드가 따로 필요 없다.

또한 앞 소스와 달리 DB를 변경하더라도 동일한 예외가 던져지는 것이 보장된다. JdbcTemplate 안에서 DB별로 에러코드와 적절한 예외가 준비되어 있기 때문이다.

JdbcTemplate을 이용한다면 JDBC에서 발생하는 DB관련 예외는 거의 신경쓰지 않아도 된다.

그런데 개발 정책상 중복키가 발생했을 때 예외처리를 강제해야 하는 경우가 있을 수 있다. 

그럼 경우 애플리케이션 레벨의 체크 예외인 DuplicateUserIdException으로 전환해주는 코드를 넣으면 된다.

```JAVA 중복 키 예외의 전환
public void add(0 throws DuplicateUserIdException {
  try {
    // jdbcTemplate을 이용해 User를 add하는 코드
  } catch(DuplicateKeyException e) {
    //로그를 남기는 등의 필요한 작업
    throw new DuplicateUserIdException(e);
  }
}
```

## 4.2.3 DAO 인터페이스와 DataAccessException 계층 구조

DataAccessException은 JDBC의 SQLException을 전환하는 용도로만 만들어진 것을 아니다. 에서도 같은 동작을 한다.

DataAccessException은 의미가 같은 예외이라면 데이터 엑세스 기술(하이버네이트, iBatis)의 종류와 상관없이 일관된 예외가 발생하도록 만들어준다. 

데이터 액세스 기술에 독립적인 추상화된 예외를 제공하는 것이다. 

### DAO 인터페이스의 구현의 분리

**DAO를 굳이 따로 만들어서 사용하는 이유는 무엇일가**? 

관심사를 분리하기 위해 데이터 엑세스 로직을 분리한기 위함이다. DAO를 사용하는 쪽에서 DAO에 내부에서 어떤 데이터 엑세스 기술을 사용하는지 신경쓰지 않아도 된다. 

그런면에서 DAO는 인터페이스를 사용해 구체적인 클래스 정보와 구현 방법을 감추고 DI를 통해 제공되도록 만드는 것이 바람직하다. 

```JAVA 기술에 독립적인 이상적인 DAO 인터페이스
public interface UserDao {
    public void add(User user); 
```

위와 같은 메소드 선언을 사용할 수 없다. 인터페이스의 메소드 선언에는 없는 예외를 구현 클래스 메소드의 throws에 넣을 수 없다.

```JAVA
public interface UserDao throws SQLException;
```

따라서 다음과 같이 선언되어야 한다. 하지만 이렇게 정의한 인터페이스는 JDBC가 아닌 데이터 엑세스 기술로 DAO를 구현을 전환하면 사용할 수 없다. 각자 독립적인 예외를 던지기 때문이다.

```JAVA
public void add(User user) throws PersistentException; // JPA
public void add(User user) throws HibernateException; // Hibernate
public void add(User user) throws JdoException; // JDO
```

결국 인터페이스로 메소드의 구현은 추상화했지만 구현 기술마다 던지는 예외가 다르기 때문에 메소드의 선언이 달라진다는 문제가 발생한다. 
그렇다고 throws Excpetion으로 던지는 것은 너무 무책임하다.

다행히도 JDBC 보다는 늦게 등장한 JDO, Hibernatem JPA 등의 기술은 SQLException 같은 체크 예외 대신 런타임 예외를 사용한다. 

따라서 throws에 선언을 해주지 않아도 되고, JDBC API를 직접 사용하는 DAO만 메소드 내에서 런타임 예외로 포장해서 던져줄 수 있다. 

따라서 처음 의도했던 대로 다음과 같인 선언해도 된다.

```JAVA
public void add(User user);
```

### 데이터 액세스 예외 추상화와 DataAccessException 계층 구조

그래서 스프링은 자바의 다양한 데이터 액세스 기술을 사용할 때 발생하는 예외들을 추상화해서 DAtaAccessException 계층 구조를 안에 정리 해놓았다.

예를 들어 JDBC, JDO, JPA, 하이버네이트에 상관없이 데이트 액세스 기술을 부정확하게 사용했을 때는 InvalidaDataAccessResourceUsageEsxception 예외가 던져진다.

그리고 InvalidaDataAccessResourceUsageEsxception의 서브클래스에는 HibernateQueryException, JdbcUpdateAffectedIncoreectNumberOfRowsException과 같은 기술에 종속된 예외클래스들이 존재한다.

JdbcTempate과 같은 스프링이 데이터 액세스 지원 기술을 이용해 DAO를 만들면 사용 기술에 독립적인 일관성 있는 예외를 던질 수 있다.

## 4.2.4 기술에 독립적인 UserDao 만들기

### 인터페이스 적용

인터페이스 구현 클래스의 이름을 정하는 방법은 여러가지가 있다. I라는 접두어를 붙이는 방법도 있고  인터페이스 이름은 간단하게, 구현클래스를 특징을 붙이는 경우도 있다.

UserDao는 인터페이스로 UserDaoJdbc는 JDBC Dao 클래스로 이름을 붙이겠다. 

```JAVA UserDao 인터페이스
public interface UserDao {
  void add(User user);
  User get(String id);
  List<User> getAll();
  void deleteAll();
  int getCount();
}
```

public 접근자를 가진 메소드이긴 하지만 UserDao의 setDataSource() 메소드는 인터페이스에 추가하면 안 된다는 사실에 주의하자. setDataSource() 메소드는 UserDao의 구현방법에 따라 변경될 수도 있고 클라이언트가 알고 있을 필요가 없다.

다음을 반영하자.

```Java UserDaoJdbc
public class UserDaoJdbc implements UserDao {
```

```pom.XML
<bean id="userDao" class="springbook.dao.UserDaoJdbc">
  <property name="dataSource" ref="dataSource"/>
</bean>
```

### 테스트 보완

UserDaoTest에 중복된 키를 가진 정보를 등록했을 때 어떤 예외가 발생하는지를 확인하기 위해 테스트를 하나 추가해보자. 

```JAVA DataAccessException에 대한 테스트
    @Test(expected = DataAccessException.class)
    public void duplicateKey() {
        dao.deleteAll();

        dao.add(user1);
        dao.add(user1);
    }
```
테스트는 성공한다. 다만 DataAccessException 타입의 예외가 던졌음이 분명한데 DataAccessException의 서브클래스 일수도 있으므로 구체적으로 어떤 예외인지 확인해 볼 필요가 있다.

### DataAccessException 활용 시 주의사항

DataAccessException이 기술에 상관없이 어느 정도 추상화된 공통 예외로 변환해주긴 하지만 근본적인 한계 때문에 완벽하다고 기대할 수 는 없다.

DB 기술별로 예외 분류 체계가 달라 스프링이 DataAccessException으로 변환해주어도 정확히 매핑시켜줄 수 없기 때문이다. 

DAO에서 사용하는 기술의 종류와 상관없이 동일한 예외를 얻고 싶다면 직접 예외를 정의해두고 기술별 좀 더 상세한 예외전환을 해줄 필요가 있다.

SQLException을 직접 해석해 DataAccessException으로 변환하는 코드를 사용법을 살펴보자.

사용하는 DataSource에서 SQLException에서 직접 DuplicateKeyException으로 전환하는 기능을 확인하는 테스트이다. 

```JAVA
@Test
    public void sqlExceptionTranslate() {
        dao.deleteAll();

        try {
            dao.add(user1);
            dao.add(user1);
        } catch(DuplicateKeyException ex) {
            SQLException sqlEx = (SQLException) ex.getRootCause();
            SQLExceptionTranslator set =
                    new SQLErrorCodeSQLExceptionTranslator(this.dataSource);

            assertThat(set.translate(null, null, sqlEx), is(DuplicateKeyException.class));
        }
    }
```

