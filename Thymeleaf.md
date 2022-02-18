타임리프(Thymeleaf)

* 서버 사이즈 HTML 렌더링(SSR)
  * 백엔드 서버에서 HTML을 동적으로 렌더링하는 용도로 사용된다.
  * 반대 의미로 클라이언트 사이드 렌더링(Javascript + React)가 있다.
  
* 네츄럴 템플릿(Nature Template)
  * 순수 HTML을 그대로 유지하면서 뷰 템플릿도 사용할 수 있는 타임리프의 특징을 네츄럴 템플릿이라고 한다.
  * 타임리프는 백엔드 서버에서 HTML을 동작으로 렌더링 하는 용도로 사용된다.
  * 타임리프로 작성한 파일은 로컬 PC에서 직접 열어도 정상적인 HTML 결과를 확인할 수 있다. 물론, 동적으로 결과가 렌더링 되지는 않지만 HTML 마크업 결과가 어떻게 되는지 확인할 수 있다.
  * 반면에 JSP로 작성한 파일을 로컬 PC에서 직접 열면 JSP 소스코드와 HTML 소스코드가 섞여 웹 브라우저에서 정상적인 HTML 결과를 확인할 수 없다. 오직 서버를 통해서 JSP가 렌더링 되고 HTML 응답  결과를 받아야 화면을 확인할 수 있다.
  
* 스프링 통합 지원
  * 타임리프는 스프링과 자연스럽게 통합되고 스프링의 다양한 기능을편리하게 사용할 수 있게 지원한다.

# 1. 타임리프 사용 선언

```HTML
<html xmlns:th="http://www.thymeleaf.org">
```

# 2. 기타 표현식

```
* 간단한 표현 :
  * 변수 표현식 : ${...}
  * 선택 변수 표현식 : *{...}
  * 메시지 표현식 : #{...}
  * 링크 URL 표현식 : @{...}
  * 조각 표현식 : ~{...}
* 리터럴
  * 텍스트 : 'one text', 'Another one!' ...
  * 숫자 : 0, 34 ...
  * 불린 : treu, false
  * 널 : null
  * 리터럴 토큰 : one, sometext, main ...
* 문자 연산
  * 문자 합치기 : +
  * 리터럴 대체 : |The name is ${name}|
* 산술 연산
  * Binary operators : +, -, *, /, %
  * Minus sign (unary operator) : -
* 불린 연산
  * Binary operators : and, or
  * Boolean negation (unary operator) : !, not
* 비교와 동등
  * 비교 : >, <, >=, <= (gt, lt, ge, le)
  * 동등연산 : ==, !=, (eq, ne)
* 조건 연산
  * IF-then : (if) ? (then)
  * If-then-else: (if) ? (then) : (else)
  * Default: (value) ?: (defaultvalue)
* 특별한 토큰:
  * No-Operation: _
```

## 2.1 텍스트 - text, utext

HTML 컨텐츠(COntent)에 데이털르 출력할 때는 다음과 같이 th:text를 사용하면 된다. HTML 태그의 속성이 아니라 HTMl 콘텐츠 영역안에서 직접 데이터를 출력하고 싶으면 [[...]]를 사용하면 된다.

```HTML
<span th:text="${data}">
[[${data}]]
```

```JAVA
    @RequestMapping(value = "/text-basic")
    public String textBasic(Model model) {
        model.addAttribute("data", "Hello Spring!");
        return "basic/text-basic";
    }
```
```HTML
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>컨테츠에 데이터 출력하기</h1>
<ul>
    <li>th:text 사용 <span th:text="${data}"></span></li>
    <li>컨텐츠 안에서 직접 출력하기 = [[${data}]]</li>
</ul>
</body>

</html>
```
![image](https://user-images.githubusercontent.com/79847020/154701234-a55394c8-7b26-4752-98a2-e5646d8a0149.png)

### HTML 엔티티
 
  HTML 엔티티는 HTML에서 &lt; 같은 특정 캐릭터들이 예약되어있기 때문에 혼란을 막기 위해서 사용하는 것입니다. 흔히 공백을 &nbsp; 로 쓰거나 <,>를 &lt; &gt; 처럼 쓰는 것을 말합니다. 
  
### Escape

출력하려는 텍스트에 <, >가 포함되어 있으면 자동으로 변환해준다.

앞 예제에서 <b>Spring</b> 태그를 사용해서 진하게 나오도록 해보자.

* 웹브라우저 : 
```
Hello <b>Spring!</b>
```
* 소스보기
```HTML 
Hello &lt;b&gt;Spring!&lt;b&gt;
```


개발자가 의도한 것은 &lt;b&gt;를 사용해서 글자를 강조하려는 목적이었다. 하지만 소스보기로 소스를 보면 '& lt ;'과 같은 형식으로 변경되어 있는 것을 확인할 수 있다.

이렇게 HTML에서 사용하는 특수 문자를 HTML 엔티티로 변경하는 것을 이스케이프(Escape)라고 한다.

* Unescape 언이스케이프

  텍스트에서 이스케이프 기능을 사용하지 않기 위해서 타임리프는 다음 두 기능을 제공한다. 
  ```
  th:utext
  [(...)]
  ```

```JAVA
    @RequestMapping(value = "/text-unescaped")
    public String textUnesacaped(Model model) {
        model.addAttribute("data", "Hello <B>Spring!</B>");
        return "basic/text-unescaped";
    }
```
```text-unescape.html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>text vs utext</h1>
<ul>
    <li>th:text = <span th:text="${data}"></span></li>
    <li>th:text = <span th:utext="${data}"></span></li>
</ul>
<h1><span th:inline="none">[[...]] vs [(...)]</span></h1>
<ul>
    <li><span th:inline="none">[[...]] = </span>[[${data}]]</li>
    <li><span th:inline="none">[[...]] = </span>[(${data})]</li>
</ul>
</body>
</html>
```
![image](https://user-images.githubusercontent.com/79847020/154707369-602f5c7e-c3ee-4096-aacc-fa5644694785.png)

실제 서비스를 개발할 때 Escape를 사용하지 않아서 HTML이 정상 렌더링 되지 안흔 수많은 문제가 발생한다. Escape를 기본으로하고 꼭 필요할 때만 Unescape하자.

## 2.2 변수 - SpringEL

타임리프에서 변수를 사용할 때는 변수 표현식을 사용한다.

### 2.2.1 변수 표현식 ${...}

변수 표현식에는 스프링 EL이라는 스프링이 제공하는 표현식을 사용할 수 있다.

* Object 
  * user.username -> user.getUsername() 호출
  * user['username'] -> user.getUsername() 호출
  * user.getUsername() -> user.getUsername() 호출

* List
  * users[0].username -> list.get(0).getUsername() 호출
  * users[0]['username'] -> list.get(0).getUsername() 호출
  * users[0].getUsername() -> list.get(0).getUsername() 호출

* Map
  * userMap['userA'].username -> map.get["userA"l.getUsername() 호출
  * userMap['userA']['username'] -> map.get["userA"l.getUsername() 호출
  * userMap['userA'].getUsername() -> map.get["userA"l.getUsername() 호출

### 2.2.2 지역 변수 선언

th:with를 사용하면 지역 변수를 선언해서 사용할 수 있다. 지역 변수는 선언한 태그 안에서만 사용할 수 있다.

```JAVA
    @RequestMapping(value = "/variable")
    public String variable(Model model) {
        User userA = new User("userA", 10);
        User userB = new User("userB", 20);

        List<User> list = new ArrayList<>();
        list.add(userA);
        list.add(userB);

        Map<String, User> map = new HashMap<>();
        map.put("UserA", userA);
        map.put("UserB", userB);

        model.addAttribute("user", userA);
        model.addAttribute("users", list);
        model.addAttribute("userMap", map);

        return "basic/variable";
    }

    @Data
    class User {
        private String username;
        private int age;

        public User(String username, int age) {
            this.username = username;
            this.age = age;
        }
    }
 ```
 ```HTML
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Title</title>
</head>
<body>
<h1>SprignEL 표현식</h1>
<ul>
    Object
    <li>${user.username} = <span th:text="${user.username}"></span></li>
        <li>${user['username']} =  <span th:text="${user['username']}"></span></li>
    <li>${user.getUsername()} =  <span th:text="${user.getUsername()}"></span></li>
</ul>
<ul>
    List
    <li>${users[0].username} =  <span th:text="${users[0].username}"></span></li>
    <li>${users[0]['username']} =  <span th:text="${users[0]['username']}"></span></li>
    <li>${users[0].getUsername()} =  <span th:text="${users[0].getUsername()}"></span></li>
</ul>
<ul>
    Map
    <li>${userMap['UserA'].username} =  <span th:text="${userMap['UserA'].username}"></span></li>
    <li>${userMap['UserA']['username']} =  <span th:text="${userMap['UserA']['username']}"></span></li>
    <li>${userMap['UserA'].getUsername()} =  <span th:text="${userMap['UserA'].getUsername()}"></span></li>
</ul>


</body>
</html>
```

### 2.2.3 기본 객체들

타임리프는 기본 객체를 제공한다

* ${request}
* ${response}
* ${ssesion}
* ${servletContext}
* ${locale}

그런데 #request는 HttpServletRequest 객체가 그대로 제공되기 때문에 request.getParameter("data") 처럼 불편하게 접근하게 되는데 편의 객체도 제공한다.

* HTTP 요청 파라미터 접근 : param
  ${param.paramData}
* HTTP 세션 접근 : session
  ${session.sessionData}
* 스프링 빈 접근 : @
  ${@helloBean.hello('Spring!')}
  
```JAVA
    @RequestMapping(value = "/basic-objects")
    public String basicObjects(HttpSession session) {
        session.setAttribute("sessionData", "Hello Session");
        return "basic/basic-objects.html";
    }

    @Component("helloBean")
    static class HelloBean {
        int abc;

        public String hello(String data) {
            return "Hello " + data;
        }
    }
```
```HTML
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Title</title>
</head>
<body>
<h1>식 기본 객체 (Expression Basic Objects)</h1>
<ul>
    <li>request = <span th:text="${#request}"></span></li>
    <li>response = <span th:text="${#response}"></span></li>
    <li>session = <span th:text="${#session}"></span></li>
    <li>servletContext = <span th:text="${#servletContext}"></span></li>
    <li>locale = <soan th:Text="${#locale}"></soan></li>
</ul>
<h1>편의 객체</h1>
<ul>
    <li>Request Parameter = <span th:text="${param.paramData}"></span></li>
    <li>session = <span th:text="${session.sessionData}"></span></li>
    <li>spring bean = <span th:text="${@helloBean.hello('Spring!')}"></span></li>
</ul>

</body>
</html>
```
![image](https://user-images.githubusercontent.com/79847020/154720468-6aef8363-143e-42dc-bdf7-c1768cb7c198.png)

### 2.2.4 유틸리티 객체와 날짜

타임리프는 문자, 숫자, 날짜. URI등을 편리하게 다루는 다양한 유틸리티 객체들을 제공한다.

타임리프 유틸리티 객체들
* #message : 메시지, 국제화처리
* #uris : URI 이스케이프 지원
* #dates : java.util.Date 서식지원
* #calendars : java.util.Calendar 서식지원
* #temporals : 자바8 날짜 서식 지원
* #numbers : 숫자 서식 지원
* #strings : 문자 관련 편의 기능
* #object : 객체 관련 기능 제공
* #bools : boolean 관련 기능 제공
* #arrays : 배열 관련 기능 제공
* #lists, #sets, #maps : 컬렉션 관련 기능 제공
* #ids : 아이디 처리 관련 기능 제공

다음 레퍼런스를 참고하자. 필요할 때 찾아서 사용하면 된다.

https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#expression-utility-objects

https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#appendix-b-expression-utility-objects

### 2.2.5 자바8 날짜

타임리프에서 자바8 날짜인 LocalDate, LocalDateTime, Instant를 사용하려면 추가 라이브러리가 필요하다. 스프링 부트 타임리프를 사용하면 해당 라이브러기 자동으로 추가되고 통합된다.

타임리프 자바8 날짜 지원 라이브러리

thymeleaf-extras-java8time

자바8 날짜용 유틸리티 객체

#temporals

```JAVA
    @RequestMapping(value = "/date")
    public String date(Model model) {
        model.addAttribute("localDateTime", LocalDateTime.now());
        return "basic/date.html";
    }
```
```HTML
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<h1>LocalDateTime</h1>
<ul>
    <li>default = <span th:text="${localDateTime}"></span></li>
    <li>yyyy-MM-dd HH:mm:ss = <span th:text="${#temporals.format(localDateTime, 'yyyy-MM-dd HH:mm:ss')}"></span></li>
</ul>

<h1>LocalDateTime - Utils</h1>
<ul>
    <li>${#temporals.day(localDateTime)} = <span th:text="${#temporals.day(localDateTime)}"></span></li>
    <li>${#temporals.month(localDateTime)} = <span th:text="${#temporals.month(localDateTime)}"></span></li>
    <li>${#temporals.monthName(localDateTime)} = <span th:text="${#temporals.monthName(localDateTime)}"></span></li>
    <li>${#temporals.monthNameShort(localDateTime)} = <span th:text="${#temporals.monthNameShort(localDateTime)}"></span></li>
    <li>${#temporals.year(localDateTime)} = <span th:text="${#temporals.year(localDateTime)}"></span></li>
    <li>${#temporals.dayOfWeek(localDateTime)} = <span th:text="${#temporals.dayOfWeek(localDateTime)}"></span></li>
    <li>${#temporals.dayOfWeekName(localDateTime)} = <span th:text="${#temporals.dayOfWeekName(localDateTime)}"></span></li>
    <li>${#temporals.dayOfWeekNameShort(localDateTime)} = <span th:text="${#temporals.dayOfWeekNameShort(localDateTime)}"></span></li>
    <li>${#temporals.hour(localDateTime)} = <span th:text="${#temporals.hour(localDateTime)}"></span></li>
    <li>${#temporals.minute(localDateTime)} = <span th:text="${#temporals.minute(localDateTime)}"></span></li>
    <li>${#temporals.second(localDateTime)} = <span th:text="${#temporals.second(localDateTime)}"></span></li>
    <li>${#temporals.nanosecond(localDateTime)} = <span th:text="${#temporals.nanosecond(localDateTime)}"></span></li>
</ul>

</body>
</html>
```
![image](https://user-images.githubusercontent.com/79847020/154722060-c5c21a3a-0288-4aac-a0ed-16b482ec04ca.png)

### 2.2.6 URL 링크

타임리프에서 URL을 생성할 때는 @{...}를 사용한다.

* 단순한 URL -> @{/hello} -> /hello

* 쿼리 파라미터 -> @{/hello(param1=${param1}, param2=${param2})} -> /hello?param1=data1&param2=data2

  ()에 있는 부분은 쿼리파라미터로 처리된다.
 
* 경로 변수 -> @{/hello/{param1}/{param2}(param1=${param1}, param2=${param2})

  URL 경로상에 변수가 있으면 () 부분은 경로 변수로 처리된다.
  
* 경로 변수 + 쿼리 파라미터 -> @{/hello/{param1}(param1=${param1}, param2=${param2})}

  경로 변수와 쿼리 파라미터를 함께 사용할 수 있다.
  
* 상대경로 -> hello -> http://localhost:8080/현재파일경로/hello

* 절대경로 -> /hello -> http://localhost:8080/hello

참조 : https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#link-urls

```JAVA
    @RequestMapping(value = "/link")
    public String link(Model model) {
        model.addAttribute("param1", "data1");
        model.addAttribute("param2", "data2");
        return "basic/link";
    }
```

```HTML
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>URL 링크</h1>
<ul>
    <li><a th:href="@{/hello}">basic url</a></li>
    <li><a th:href="@{/hello(param1=${param1}, param2=${param2})}">hello query param</a></li>
    <li><a th:href="@{/hello/{param1}/{param2}(param1=${param1}, param2=${param2})}">path variable</a></li>
    <li><a th:href="@{/hello/{param1}(param1=${param1}, param2=${param2})}">path variable + query parameter</a></li>
</ul>
</body>
</html>
```
![image](https://user-images.githubusercontent.com/79847020/154727552-cfe8d007-978a-432b-b821-b58425bc0eb3.png)

## 2.3 리터럴(Literal)

리터럴은 소스 코드상에 고정된 값을 말하는 용어이다. 

* 문자 : 'hello'
* 숫자 : 10
* 불린 : ture, false
* null : null

타임리프에서 문자 리터럴은 항상 \'(작은따옴표)로 감싸야한다.
```HTML
<span th:text="'hello">
```
  
그런데 문자를 항상 \'로 감싸는 것은 너무 귀찮은 일이다. `공백 없이` 쭉 이어진다면 하나의 의미있는 토큰으로 인지해서 다음과 같이 작은 따옴표를 생략할 수 있다.

  룰 : A-Z, a-z, 0-9, [], ., -, _
```HTML  
<span th:Text="hello">
```
  
* 오류
  ```HTML
  <span th:text="hello spring">
  ```
  하지만 다음과 같이 공백이 있다면 하나의 의미있는 토큰(?)으로 인식되지 않는다. 작은따옴표로 감싸면 정상 동작한다.
  ```HTML
  <span th:text="'hello spring'">
  ```
  
 
