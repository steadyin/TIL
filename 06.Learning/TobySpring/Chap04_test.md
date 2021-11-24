# Chap04. 예외

* 예외를 처리할 때 반드시 지켜야 할 핵심 원칙은 한가지다. 모든 예외는 적절하게 복구되든지 아니면 작업을 중단시키고 운영자 또는 개발자에게 분명하게 통보돼야 한다.
예외를 무시하거나 잡아먹어 버리는 코드는 만들지 말라는 뜻이다. 굳이 예외를 잡아서 뭔가 조치를 취할 방법이 없다면 잡지 말아야 한다. 메소드에 throws SQLException을
선언해서 메소드 밖으로 던지고 자신을 호출한 코드에 예외처리 책임을 전가해버려라.(그러라는 의미가 아니라 차라리)

* Error
    
      java.lang.Error 클래스의 서브클래스들이다. 에러는 시스템 레벨에서 뭔가 비정상 상황이 발생했을 경우 사용된다. 
      JVM에 발생시키는 것이 주다. 대응방법이 없기 때문에 처리는 신경 쓰지 않아도 된다.
    
* Exception
      
      java.lang.Exception 클래스의 서브클래스들이다. 체크 예외와 언체크 예외로 나뉜다.
      
* 체크 예외(Checked Exception)

      Exception의 서브클래스 중 RuntimeException을 상속하지 않은 클래스. 체크 예외가 발생할 수 있는 경우에 예외 처리가 강제된다. 
      예외를 처리하지 않으면 컴파일 에러가 발생한다.

* 언체크 예외(Unchecked Exception) = 런타임예외(RuntimeException)

      Exception의 서브클래스 RuntimeException을 상속한 클래스, 상속하고 RuntimeException 클래스를 상속하지 않은 것이다.
      예외처리가 강제되지 않는 예외. NullPointerException, IllegalArgumentException 등이 있다.
      
* 예외처리 방법

    * 예외 복구

        예외처리 방법은 예외 상황을 파악하고 문제를 해결해서 정상 상태로 돌려놓는 것이다. 기본 작업 흐름이 불가능하면 다른 작업 흐름으로 자연스럽게 유도해주는 것이다.
        
        재시도를 통해 예외를 복구하는 코드
        ```JAVA
        int maxretry = MAX_RETRY;
        while(maxretry -- > 0) {
            try {
                ...          //예외가 발생할 가능성이 잇는 시도
                return;      //작업 성공
            } catch(SomeException e) {
                //로그 출력, 정해진 시간만큼 대기
            } finally {
                //리소스 반납 정리 작업
            }
        }
        throw new RetryFailedException(); //최대 재시도 횟수를 넘기면 직접 예외 발생
        
 
    * 예외처리 회피
    
        예외 처리를 자신이 담당하지 않고 자신을 호출한 쪽으로 던져버리는 것이다. 예외를 잡은 후에 로그를 남기고 다시 예외를 던지는(throw) 것이다.
        예외를 회피하는 것은 예외를 복구하는 것처럼 의도가 분명해야 한다. 
        다른 오브젝트에게 책임을 분명히 지게하거나, 자신을 사용하는 쪽에서 예외를 다루는 게 최선의 방법이라는 분명한 확신이 있어야 한다.
        
        예외처리 회피 1
        ```JAVA
        public void add() throws SQLException {}
        ```
        
        예외처리 회피 2
        ```JAVA
        public void add() throws SQLException {
            try {
                // JDBC API
            } catch(SQLException e) {
                // 로그 출력
                throw e;
            }
        }
        ```
        
    * 예외 전환(exception translation)

        예외 회피와 비슷하게 예외를 복구해서 정상적인 상태로 만들 수 없기 때문에 예외를 메소드 밖으로 던지는 것이다. 하지만 예외 회피와 달리 그대로 넘기는게 아니라 적절한
        예외로 전환해서 던진다는 특징이 있다. 보통 두자기 목적으로 사용된다.
        
        첫째, 예외의 의미를 분명하게 해줄 수 있는 있는 예외로 바꿔준다.
        
        예외 전환 기능을 가진 DAO 메소드
        ```JAVA
        public void add(User user) throws DuplicateUserIdException, SQLException {
            try {
                //JDBC를 이용해 user정보를 DB에 추가하는 코드 또는
                //그런 기능을 가진 다른 SQLException을 던지는 메소드를 호출하는 코드
            } cach(SQLException e) {
                //ErrorCode가 MySQL의 'Duplicate Entry(1062)'이면 예외 전환
                if(e.getErrorCode()==MysqlErrorNumbers.EP_DUP_ENTRY)
                    throw DuplicateUserIdException(); // 중첩 예외 사용방법 1
                else 
                    throw e; //그 외에 경우는 SQLException 그대로
            }
        }
        ```
        
        보통 전환하는 예외에 발생한 예외를 담아서 중첩 예외(nested exception)로 만드는 것이 좋다.
        
        중첩 예외 1,2
        ```JAVA
        catch(SQLException e) {
            throw DuplicateUserIdException(e);
            or
            throw DuplicateUserIdException().initCause(e);
        }
        ```
        
        두번째, 예외처리를 강제하는 체크 예외를 언체크 예외인 런타임 예외로 바꾸는 경우에 사용한다. 복구 처리가 불가능한 예외라면 그자리에서 빨리 런타임 예외로 포장해 던지게 해서 다른 계층의 메소드를 작성할 때 불필요한 throws 선언이 들어가지 않도록 해줘야 한다.
        
        예외 포장
        ```JAVA
        try {
            OrderHome orderHome = EJBHomeFactory.getInstance().getOrderHome();
            Order order = orderHome.findByPrimaryKey(integer id);
        } catch(NamingException ne) {
            throw new EJBException ne) {
        } catch (SQLException se) {
            throw new EJBException(se);
        } catch(RemoteException re) {
            throw new EJBException(re);
        }
        ```

*  예외 처리 전략

    일반적으로 체크예외가 일반적인 예외를 다루고 언체크 예외는 시스템 장애나 프로그램상의 오류에 사용한다고 했다.
    
    자바 엔터프라이즈 서버 환경은 수많은 사용자가 동시에 요청을 보내고 각 요청이 독립적인 작업으로 취급된다. 하나의 요청을 처리하는 중에 예외가 발생하면 해당 작업만 중단시키면 그만이다.
    
    서버 환경에서는 예외상황을 미리 파악하고 사전에 차단하고, 다른 방법으로 유도하고, 대응이 불가능한 체크 예외라면 빨리 런타임 예외로 전환해서 던지는게 낫다.
    
    예를 들어 SQLException 예외 발생 시 SQLException Error코드를 분석해 대응할 수 있는 ID중복같은 Exception이면이면 (사용자에게 ID추천) 체크예외로 예외를 처리하고 SQL문법 Exception이면 런타임예외로 전환하는 것이 더 나은 전략이다.
 
    예외처리 전략을 적용한 add()
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
    
    * 애플리케이션 예외

          애플리케이션 자체의 로직에 의해 의도적으로, 개발자가 발생시키고 반드시 catch 해서 무엇인가 조치를 취하도록 요구하는 예외
          
    * SQLException 예외 처리

          99% SQLException은 코드 레벨에서는 복구할 방법이 없다. 따라서 언체크/런타임 예외로 전환해줘야 한다. 스프링의
          JdbcTemplate은 템플릿과 콜백 안에서 발생하는 모든 SQLException을 런타임 예외인 DataAccessExcetpion으로 포장해서
          던져준다. 따라서 JdbcTemplate의 update(), queryForInt(), query() 메소드 선언을 잘 살펴보면 선언부에 throws
          DataAccessException라고 선언되어 있고, 런타임 예외이므로 예외 처리는 강요하지 않는다.
      
* 예외 전환 ( DataAccessException )

    스프링은 DataAccessException 이라는 SQLException을 대체할 수 있는 런타임 예외를 정의하고 있을 뿐만 아니라 DataAccessExcetpion의 서브 클래스로 세분화된 예외 클래스들을 정의하고 있다. 스프링은 DB별 에러코드를 분류해서 스프링이 정의한 예외 클래스와 매핑 해놓은 에러 코드 매핑정보 테이블을 만들어두고 관리한다.
    
    * JDBC

          JDBC는 자바를 이용해 DB에 접근하는 방법을 추상화된 API 형태로 정의해놓고 DB 업체가 JDBC 표준을 따라 만들어진 드라이버를
          제공하게 해준다. 내부 구현은 DB마다 다르겠지만 JDBC의 Connection, Statement, ResultSet 등의 표준 인터페이스를 통해
          기능을 제공해주기 때문에 자바 개발자들은 표준화된 JDBC의 API에만 익숙해지면 DB의 종류에 상관없이 일관된 방법으로
          프로그램 개발을 할 수 있다.

    * DataAccessException
    
          JdbcTemplate은 SQLException을 단지 런타임 예외인 DataAccessExcetpion으로 포장하는 것이 아니라 각 DB의 에러 코드를
          DataAccessException 계층구조의 클래스 중 하나로 매핑해준다. 따라서 DB를 변경하더라도 동일한 예외가 던져지는 것이 보장된다.
          
          DataAccessException은 의미가 같은 예외라면 데이터 액세스 기술의 종류와 상관없이 일관된 예외가 발생하도록 만들어준다.
          데이터 액세스 기술에 독립적인 추상화된 예외를 제공하는 것이다.
    
        ```JAVA
        @Test(expected = DataAccessException.class)
        public void duplicateKEy() {
            dao.deleteAll();
    
            dao.add(user1);
            dao.add(user1);
        }
        ```
    
    * SQLExceptionTranslator의 구현체 SQLErrorCodeSQLExceptionTranslator

          DB에서 발생되는 SQLException을 직접 해석해 DataAccessException으로 변환할 떄 사용되는 Translator
          
      ```JAVA
      @Test
      public void sqlExceptionTranslate() {
          dao.deleteAll();

          try {
              dao.add(user1);
              dao.add(user1);
          } catch(DuplicateKeyException ex) {
              SQLException sqlEx = (SQLException) ex.getRootCause();
              SQLExceptionTranslator set = new SQLErrorCodeSQLExceptionTranslator(this.dataSource);
              assertThat(set.translate(null,null,sqlEx),is(DuplicateKeyException.class));
          }
      }
      ```
    
      
     
    
    
    
    
    
    
    
    
    
    


      
    

            
    
    
    
    
    
    
