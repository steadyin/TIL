# 1. 타임리프(Thymeleaf)

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

* 타임리프 사용 선언

  ```HTML
  <html xmlns:th="http://www.thymeleaf.org">
  ```

* 기타 표현식

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
