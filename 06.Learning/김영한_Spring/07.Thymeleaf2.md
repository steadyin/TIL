# 2. 타임리프 스프링 통합

타임리프는 크게 2가지 메뉴얼을 제공한다.

기본메뉴얼 : https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html

스프링 통합 메뉴얼 : https://www.thymeleaf.org/doc/tutorials/3.0/thymeleafspring.html

* 스프링 통합으로 추가되는 기능들
  * 스프링의 SpringEL 문법 통합
    * ${@myBean.doSomething()} 처럼 스프링 빈 호출 지원
  *. 편리한 폼 관리를 위한 추가 속성
    * th:object (기능 강화, 폼 커맨드 객체 선택)    * th:field, th:errors, th:errorclass
  * 폼 컴포넌트 기능
    * checkBox, radio button, List 등을 편리하게 사용할 수 있는 기능 지원
  * 스프링의 메시지, 국제화 기능의 편리한 통합
  * 스프링의 검증, 오류 처리 통합
  * 스프링의 변환 서비스 통합(ConversionService)

설정 방법 - 타임리프 템플릿 엔진을 스프링 빈에 등록하고, 타임리프용 뷰 리졸버를 스프링 빈으로 등록하는 방법

http://www.thymeleaf.org/doc/tutorials/3.0/thymeleafspring.html#the-springstandard-dialect
http://www.thymeleaf.org/doc/tutorials/3.0/thymeleafspring.html#views-and-view-resolvers

* 타임리프 템플릿 엔진을 스프링 빈에 등록

  ```JAVA
    @Bean
    public SpringResourceTemplateResolver templateResolver(){
      // SpringResourceTemplateResolver automatically integrates with Spring's own
      // resource resolution infrastructure, which is highly recommended.
      SpringResourceTemplateResolver templateResolver = new SpringResourceTemplateResolver();
      templateResolver.setApplicationContext(this.applicationContext);
      templateResolver.setPrefix("/WEB-INF/templates/");
      templateResolver.setSuffix(".html");
      // HTML is the default value, added here for the sake of clarity.
      templateResolver.setTemplateMode(TemplateMode.HTML);
      // Template cache is true by default. Set to false if you want
      // templates to be automatically updated when modified.
      templateResolver.setCacheable(true);
      return templateResolver;
    }
    
    @Bean
    public SpringTemplateEngine templateEngine(){
      // SpringTemplateEngine automatically applies SpringStandardDialect and
      // enables Spring's own MessageSource message resolution mechanisms.
      SpringTemplateEngine templateEngine = new SpringTemplateEngine();
      templateEngine.setTemplateResolver(templateResolver());
      // Enabling the SpringEL compiler with Spring 4.2.4 or newer can
      // speed up execution in most scenarios, but might be incompatible
      // with specific cases when expressions in one template are reused
      // across different data types, so this flag is "false" by default
      // for safer backwards compatibility.
      templateEngine.setEnableSpringELCompiler(true);
      return templateEngine;
    }
   
  ```

* Thymeleaf 뷰 리졸버 빈 등록

  ```JAVA
  @Bean
  public ThymeleafViewResolver viewResolver(){
      ThymeleafViewResolver viewResolver = new ThymeleafViewResolver();
      viewResolver.setTemplateEngine(templateEngine());
      // NOTE 'order' and 'viewNames' are optional
      viewResolver.setOrder(1);
      viewResolver.setViewNames(new String[] {".html", ".xhtml"});
      return viewResolver;
  }
  ```
  스프링부트는 이런 ()과정을 자동화해준다. 'org.springframework.boot::spring-boot-starter-thymeleaf' 의존성을 추가하면 알아서 스프링 빈을 자동으로 등록해준다.

* 스프링부트가 제공하는 타임리프 설정, thymeleaf 검색 필요
 
  https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#application-properties.templating

  참고로 suffix 의 기본값은 html 이다.

  ![img.png](img.png)

# 2.1 입력 폼 처리

1. th:object : 커맨드 객체를 지정한다.
2. *{...} : 선택변수 식이라고 한다. th:object에서 선택한 객체에 접근한다.
   1. th:field : HTML 태그의 id, name, value 속성을 자동으로 처리해준다.

* 등록 폼 수정 

  원래 소스 addForm.html
  ```html
  ...
  <form action="item.html" th:action method="post">
          <div>
              <label for="itemName">상품명</label>
            <input type="text" id="itemName" name="itemName" class="form-control" placeholder="이름을 입력하세요">
          </div>
          <div>
              <label for="price">가격</label>
              <input type="text" id="price" name="price" class="form-control" placeholder="가격을 입력하세요">
          </div>
          <div>
              <label for="quantity">수량</label>
              <input type="text" id="quantity" name="quantity" class="form-control" placeholder="수량을 입력하세요">
          </div>
  ...생략
  ```
  수정 후
  ```html
  ...
  <form action="item.html" th:action th:object="${item}" method="post">
    <div>
      <label for="itemName">상품명</label>
      <input type="text" th:field="*{itemName}" class="form-control" placeholder="이름을 입력하세요">
    </div>
    <div>
      <label for="price">가격</label>
      <input type="text" th:field="*{price}" class ="form-control" placeholder="가격을 입력하세요">
    </div>
    <div>
      <label for="quantity">수량</label>
      <input type="text" th:field="*{quantity}" class="form-control" placeholder="수량을 입력하세요">
    </div>
  ...생략
  ```
  렌더링 후 
  ```HTML
  ...
      <form action="" method="post">
          <div>
              <label for="itemName">상품명</label>
              <input type="text" class="form-control" placeholder="이름을 입력하세요" id="itemName" name="itemName" value="">
          </div>
          <div>
              <label for="price">가격</label>
              <input type="text" class ="form-control" placeholder="가격을 입력하세요" id="price" name="price" value="">
          </div>
          <div>
              <label for="quantity">수량</label>
              <input type="text" class="form-control" placeholder="수량을 입력하세요" id="quantity" name="quantity" value="">
          </div>
  ...생략
  ```
  
  \<from\> 태그에 th:object 속성을 적용하고 \<input\>에 th:field 속성을 적용하면 렌더링 후 id, name, value 속성이 적용되는 것을 확인할 수 있다.

  핸들러에서 빈 객체를 생성해주는 것은 사실 거의 비용이 발생하지 않으니 신경쓰지 않아도된다. 빈 객체를 사용하므로서 addForm의 ath:field 속성의 에러를 잡아줄 수 있고 Validation을 적용할 때 도움이 되므로 얻을 수 있는 장점이 더 많다.   
  ```java
  @GetMapping("/add")
  public String addForm(Model model) {
    model.addAttribute("item", new Item());
    return "form/addForm";
  }
  ```
 
* 수정폼 수정

  원래소스
  ```HTML
  ...
      <form action="item.html" th:action method="post">
          <div>
              <label for="id">상품 ID</label>
              <input type="text" id="id" name="id" class="form-control" value="1" th:value="${item.id}" readonly>
          </div>
          <div>
              <label for="itemName">상품명</label>
              <input type="text" id="itemName" name="itemName" class="form-control" value="상품A" th:value="${item.itemName}">
          </div>
          <div>
              <label for="price">가격</label>
              <input type="text" id="price" name="price" class="form-control" value="10000" th:value="${item.price}">
          </div>
          <div>
              <label for="quantity">수량</label>
              <input type="text" id="quantity" name="quantity" class="form-control" value="10" th:value="${item.quantity}">
          </div>
  ...생략
  ```
  
  수정 후
  ```html
  ...
  <form action="item.html" th:action th:object="${item}" method="post">
    <div>
      <label for="id">상품 ID</label>
      <input type="text" id="id" class="form-control"  th:field="*{id}" readonly>
    </div>
    <div>
      <label for="itemName">상품명</label>
      <input type="text" id="itemName" class="form-control" th:field="*{itemName}"">
    </div>
    <div>
      <label for="price">가격</label>
      <input type="text" id="price" class="form-control" th:field="*{price}"">
    </div>
    <div>
      <label for="quantity">수량</label>
      <input type="text" id="quantity" class="form-control" th:field="*{quantity}"">
    </div>
  ...
  ```
  
  렌더링
  
  ```html
      <!-- 추가 -->
      <form action="" method="post">
      <div>
          <label for="itemId">상품 ID</label>
          <input type="text" id="itemId" name="itemId" class="form-control" value="1" readonly>
      </div>
      <div>
          <label for="itemName">상품명</label>
          <input type="text" id="itemName" name="itemName" class="form-control" value="itemA" readonly>
      </div>
      <div>
          <label for="price">가격</label>
          <input type="text" id="price" name="price" class="form-control" value="10000" readonly>
      </div>
      <div>
          <label for="quantity">수량</label>
          <input type="text" id="quantity" name="quantity" class="form-control" value="10" readonly>
      </div>
  ```
# 2.2 요구사항 추가

Thymeleaf를 사용해서 폼에서 체크박스, 라디오박스, 셀렉트 박스를 편리하게 사용하는 방법을 학습해보자. 추가된 요구사항이다.

1. 판매여부
   - 판매 오픈 여부
   - 체크 박스로 선택할 수 있다.
2. 등록 지역
   - 서울,부산,제주
   - 체크 박스로 다중 선택할 수 있다.
3. 상품 종류
   - 도서, 식품, 기타
   - 라디오 버튼으로 하나만 선택할 수 있다.
4. 배송 방식
   - 빠른 배송
   - 일반 배송
   - 느린 배송
   - 셀렉트 박스로 하나만 선택할 수 있다.

요구사항 반영을 위한 클래스를 작성한다. ENUM 클래스, String 같은 다양한 상황을 준비했다. 각각의 상황에 어떻게 폼의 데이터를 받을 수 있는지 알아보자.

```java
/**
 * FAST : 빠른 배송
 * NORMAL : 일반 배송
 * SLOW : 느린 배송
 */
@Data
@AllArgsConstructor
public class DeliveryCode {
    private String code;
    private String displayName;
}


public enum ItemType {
  BOOK("도서"), FOOD("음식"), ETC("기타")
  ;

  private final String description;

  ItemType(String description) {
    this.description = description;
  }
}


@Data
public class Item {

  private Long id;
  private String itemName;
  private Integer price;
  private Integer quantity;

  private Boolean open; //판매 여부
  private List<String> regions; //등록 지역
  private ItemType itemType; //상품 종류
  private DeliveryCode deliveryCode; //배송 방식

  public Item() {
  }

  public Item(String itemName, Integer price, Integer quantity) {
    this.itemName = itemName;
    this.price = price;
    this.quantity = quantity;
  }
}
```
 
# 2.3 체크 박스 - 단일 1

* addForm.html
  ```html
  ...
  <form action="item.html" th:action th:object="${item}" method="post">
    ...
          <div>판매 여부</div>
          <div>
              <div class="form-check">
                  <input type="checkbox" id="open" name="open" class="form-check-input">
                  <label for="open" class="form-check-label">판매 오픈</label>
              </div>
          </div>
  ...
  ```
  
  Item클래스에서 정의한 Boolean 타입의 open 정보가 해당 체크박스에 바인딩 되는 것을 기대한다.
  
  addForm에서 등록을 누르면 브라우저에서 open 필드에 'on'이란 값으로 서버에 전달하는 것을 볼 수 있다.
  
  ![img_1.png](img_1.png)
  
  스프링은 'on'이라는 문자를 true으로 변환해준다. 스프링 타입 컨버터가 이 기능을 수행하는데 뒤에서 설명한다.
  체크박스를 선택하지 않고 폼을 전송하면 open이라는 필드 자체가 서버로 전송되지 않는다.
  
  ![img_2.png](img_2.png)

* HTTP 요청 메시지 로깅

  application.properties에 logging.level.org.apache.coyote.http11 = debug 옵션으로 HTTP 요청 메시지를 로깅할 수 있다.
  
  ![img_3.png](img_3.png)
 
  * 판매 오픈 체크했을 때 HttpBody 
  
    ```itemName=1234&price=12&quantity=12&open=on```

  * 판매 오픈 체크안했을 때 HttpBody

    ```itemName=1234&price=12&quantity=12```
 
* 체크 해제를 인식하기 위한 히든 필드
  
  ```html
  <input type="hidden" name="_open"  value="on"> <!--히든 필드 추가-->
  ```
  
  히든 필드를 추가하면 다음과 같은 Http요청 메시지가 다음과 같이 넘어온다.
  
  ```itemName=1234&price=12&quantity=12&_open=on```
  
  스프링에서는 false로 바인딩한다.   

  ![img_4.png](img_4.png)


  

  

  



















