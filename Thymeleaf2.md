# 2. 타임리프 스프링 통합

타임리프는 크게 2가지 메뉴얼을 제공한다.

기본메뉴얼 : https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html
스프링 통합 메뉴얼 : https://www.thymeleaf.org/doc/tutorials/3.0/thymeleafspring.html

* 스프링 통합으로 추가되는 기능들
  1. 스프링의 SprignEL 문법 통합
    - ${@myBean.doSomething()} 처럼 스프링 빈 호출 지원
  2. 편리한 폼 관리를 위한 추가 속성
    - th:object (기능 강화, 폼 커맨드 객체 선택)
    - th:field, th:errors, th:errorclass
  3. 폼 컴포넌트 기능
    - checkBox, radio button, List 등을 편리하게 사용할 수 있는 기능 지원
  4. 스프링의 메시지, 국제화 기능의 편리한 통합
  5. 스프링의 검증, 오류 처리 통합
  6. 스프링의 변환 서비스 통합(ConversionService)


설정 방법
타임리프 템플릿 엔진을 스프링 빈에 등록하고 타임리프 뷰 리졸버를 스프링 빈으로 등록하는 방법


  
