[Web Server, WAS](#-1.-Web-Server,-WAS(Web Application Server)) <br>
[Servlet](#-2.-서블릿) <br>

# 스프링 MVC

# 1.웹 애플리케이션 이해

## 1.1 Web Server, WAS(Web Application Server)

## WEB 이란 ?

Web이란 것은 HTTP 프로토콜위에서 이루어진다. 클라이언트에서 서버로 데이터를 전송할 때 서버에서 클라이언트로 데이터를 전송할 때 모두 HTTP프로토콜을 사용한다. 전송되는 HTML, TEXT, 이미지, 음성, 영상, 파일, JSON, XML 거의 모든 형태의 데이터를 전송할 수 있다. 

## Web Server(Web Server)

![img_1](https://user-images.githubusercontent.com/79847020/159015478-e77eaf9e-39ec-4542-bb33-840e50b84a78.png)

HTTP 프로토콜 기반으로 동작, 정적인 리소스를 제공하고 기타 부가기능을 제공한다. 정적인 리소스란 HTML, CSS, JS, 이미지, 영상 등을 말한다. 

대표적으로 NGINX, APACHE 가 있습니다. 

### WAS(Web Application Server)

![img](https://user-images.githubusercontent.com/79847020/159015491-a38a9161-c51f-4c72-a4c3-79f32ef0f5e6.png)

HTTP 프로토콜 기반으로 동작, Web Server 기능을 하면서 프로그램 코드를 실행해서 애플리케이션 로직을 수행합니다. 동적 HTML, HTTP API(JSON), 서블릿, JSP, 스프링 MVC 등을 처리할 수 있습니다. 

대표적으로 톰캣, Jetty, Undertow 가 있습니다.

### Web Server, WAS 차이

웹 서버는 정적 리소스를 주로 처리하고 WAS는 애플리케이션 로직을 주로 담당한다. 사실 웹 서버도 간단한 프로그램을 실행하는 기능을 수행하기도 하고 WAS도 정적인 리소스를 다룰 수 있기 때문에 경계가 모호하다. 

WAS는 애플리케이션 코드를 실행하는데 더 목적성이 있다고 정리하자.

### 웹 시스템 구성

사실 WebServer의 기능을 WAS가 포함하고 있기 때문에 WAS와 DB만으로도 충분히 시스템을 구성할 수 있다. 하지만 이런 경우 WAS가 많은 역할을 담당하게 되고 서버 과부하 우려가 있다. 그리고 WebServer에 비해 WAS는 상대적으로 비용이 비싸다. 정적인 리소스를 처리하기 WAS에 쓸데없는 부하가 발생한다.  또한 WAS 장애시 어떤 오류 페이지도 클라이언트에게 보여줄 수 없다.

![img_2](https://user-images.githubusercontent.com/79847020/159015504-91a6c13f-9bb6-4ac8-b3ac-25c228e3b99e.png)

따라서 WebServer, WAS, DB로 Web 시스템을 구성한다. 정적인 리소스는 Web Server가 처리하고 애플리케이션 로직같은 동적인 처리는 WAS가 담당한다.

정적 리소스가 과부하 되면 WebServer 증설, 애플리케이션 리소스가 과부하 되면 WAS 증설하면 된다.

![img_3](https://user-images.githubusercontent.com/79847020/159015509-f12ea14e-947f-4eea-a27b-ed5248a55881.png)

WebServer 도입으로 WAS 장애가 발생해도 오류 페이지를 볼 수 있다. 

## 1.2 서블릿

### 서블릿을 왜 사용할까 ?

![img_4](https://user-images.githubusercontent.com/79847020/159015518-586611a2-3614-43a6-8361-52cb60921041.png)

웹 브라우저에서 폼 데이터를 전송하기 위해 HTTP 요청 메시지를 생성한다.

만약 WAS에서 우리가 직접 HTTP 요청 메시지를 처리하기 위해서는 다음과 같은 과정이 필요하다.

![img_5](https://user-images.githubusercontent.com/79847020/159015528-6e4423f1-ee8f-4869-a525-c3ea9f4ea7d2.png)

직접 구현하면 비즈니스 로직은 전과정에서 비중이 그렇게 크지 않다. 기본적인 HTTP 통신을 하기 위한 과정들이 매번 반복되고 또 매번 구현하기에는 거대하다.

서블릿을 지원하는 WAS 사용하면 이 과정을 서블릿에게 위임하고 개발자는 비즈니스 로직에 집중할 수 있다. 

### 서블릿 구현부

```java
@WebServlet(name = "helloServlet", urlPatterns = "/hello")
public class HelloServlet extends HttpServlet {
  @Override
  protected void service(HttpServletRequest request, HttpServletResponse response){ 
      //애플리케이션 로직
  }
}
```
* 서블릿
  * urlPattern(/hello)에 URL이 호출되면 서블릿 코드가 실행
  * HTTP 요청 정보를 편리하게 사용할 수 있는 HttpServletRequest
  * HTTP 응답 정보를 편리하게 제공할 수 있는 HttpServletResponse
  * 개발자를 HTTP 스펙 구현을 쉽게 할 수 있다.

### 서블릿 흐름

![img_6](https://user-images.githubusercontent.com/79847020/159015540-d9a8a806-5aa8-4de1-b973-e8d4bf2673e8.png)

HTTP 요청오면
* WAS는 HTTP 요청정보로 Request, Response 객체를 새로 만들어서 서블릿 객체 호출
* 개발자는 Request 객체에서 HTTP 요청정보를 편리하게 꺼내서 사용
* 개발자는 Response 객체에 HTTP 응답정보를 편리하게 입력
* WAS는 Response 객체에 담겨있는 내용으로 HTTP 응답정보를 생성

### 서블릿 컨테이너, 서블릿 라이프사이클

![img_7](https://user-images.githubusercontent.com/79847020/159015550-cf69de12-6913-4432-8946-2af0c0afa26a.png)

* 톰캣처럼 서블릿을 지원하는 WAS를 서블릿 컨테이너라고 함
* 서블릿 컨테이너는 서블릿 객체를 생성, 초기화, 호출, 종료하는 생명주기 관리
* 서블릿 객체는 싱글톤으로 관리
  * init(), service() -> doGet(), doPost(), destroy()
  * 생성, 초기화 : 최초 로딩시점에 서블릿 객체를 미리 만들어 두고 재활용
  * 모든 고객 요청은 동일한 서블릿 객체 인스턴스에 접근
  * 공유 변수 사용 주의
  * 서블릿 컨테이너 종료시 함께 종료
* JSP도 서블릿으로 변환 되어서 사용
* 동시 요청을 위한 멀티 쓰레드 처리 지원

## 1.3 멀티 스레드 - 동시 요청

![img_8](https://user-images.githubusercontent.com/79847020/159015565-042605e4-0c17-421b-a3aa-dba5d0ae3d23.png)

![img_9](https://user-images.githubusercontent.com/79847020/159015572-4a8edb4f-8eb0-4c21-a91c-325182cfc41b.png)

### 스레드
  * 애플리케이션 코드를 하나씩 순차적으로 실행하는 것은 스레드
  * 자바 메인 메서드를 처음 실행하면 main이라는 이름의 스레드가 실행
  * 스레드는 한번에 하나의 코드 라인 수행
  * 동시 처리가 필요하면 스레드 추가로 생성

### 단일 요청 - 스레드 한개

![img_10](https://user-images.githubusercontent.com/79847020/159015585-67466bef-77af-405f-836b-1dfbe3b8a699.png)
  
스레드가 하나 있다고 가정하자. WAS를 구성하고 스레드가 현재 동작하고 있지 않다.
  
![img_11](https://user-images.githubusercontent.com/79847020/159015600-a8abdc8f-b54a-4f20-ab4d-1943665945f0.png)

요청이 오면 스레드를 할당하고 스레드에서 서블릿 코드를 실행하게 된다.

![img_12](https://user-images.githubusercontent.com/79847020/159015608-77e34120-6f70-4b25-980c-57c0a0050063.png)

그러고 나서 스레드에서 응답을 하고 최종적으로 WAS에서 클라이언트로 응답이 되게 됩니다.

![img_13](https://user-images.githubusercontent.com/79847020/159015617-f55cba1d-a2b1-4566-8642-390a1b44606a.png)

### 다중 요청 - 스레드 여러개

![img_14](https://user-images.githubusercontent.com/79847020/159015633-158a5ef6-f123-4e3c-86ad-11704a1c0002.png)
  
다중 요청시에도 요청1이 오면 스레드가 할당되고 서블릿을 실행한다. 그런데 처리가 지연됬다.  

![img_15](https://user-images.githubusercontent.com/79847020/159015642-ff3cdccd-115f-4123-a6ef-fc95b7c5692e.png)

이 상황에서 요청2가 WAS에 들어온다. 요청2를 처리할 수 있는 스레드가 없다면 요청2는 대기하게 된다.

![img_16](https://user-images.githubusercontent.com/79847020/159015649-d63da084-990a-4cb8-8d13-1d4bdeb0bed1.png)

그럼 요청1과 요청2과 둘다 처리되지 않는 상황이 발생한 것이다.

![img_17](https://user-images.githubusercontent.com/79847020/159015656-bd595cb4-d9a3-44fa-80a4-6c12f6827b0e.png)

그래서 요청 마다 스레드를 생성해서 요청을 처리한다.

장점
* 동시 요청을 처리할 수 있다.
* 리소스(CPU, 메모리)가 허용할 때 까지 처리 가능
* 하나의 스레드가 지연 되어도 나머지 스레드는 정상 동작한다.
* 스레드는 생성 비용은 매우 비싸다.
  * 요청이 올 때 마다 스레드를 생성하면 응답 속도가 늦어진다.
  * 스레드는 컨텍스트 스위칭 비용이 발생한다.
    * CPU 코어가 1개고 스레드가 2개일 때 코어가 스레드 2개를 동시에 실행하는 것이 아니라 스레드1과 스레드2를 스위칭하며 실행하게 된다. 이것을 컨텍스트 스위칭이라고 한다.
* 스레드 생성에 제한이 없다.
  * 요청이 너무 많으면 CPU, 메모리 임계점을 넘어 서버가 죽을 수 있다.

### 스레드 풀

따라서 이런 단점을 해결하기 위한 방도로 스레드 풀을 도입한다.

![img_18](https://user-images.githubusercontent.com/79847020/159015707-9df63c94-4d79-41b9-aaba-298d09217a0b.png)
 
WAS에 스레드 풀이 있다. 요청이 오면 각 요청 마다 스레드풀에 대기하고  있는 스레드를 배정해서 처리한다. 처리 후에는 스레드풀에 스레드를 다시 반납합니다. 

![img_19](https://user-images.githubusercontent.com/79847020/159015722-6629139a-0ad3-4442-80c9-e2b24bd82280.png)

만약 스레드풀에 있는 스레드보다 더 많은 요청이 오게 되면 요청은 대기하거나 거절합니다. 

특징
* 필요한 스레드를 스레드 풀에 보관하고 관리한다.
* 스레드 풀에 생성 가능한 스레드의 최대치를 관리한다. 톰캣은 최대 200개 기본 설정(변경 가능)

사용
* 스레드가 필요하면 이미 생성되어 있는 스레드를 스레드 풀에서 꺼내서 사용한다.
* 사용을 종료하면 스레드 풀에 해당 스레드를 반납한다.
* 최대 스레드가 모두 사용중이어서 스레드 풀에 스레드가 없으면 기다리는 요청은 거절하거나 특정 숫자만큼만 대기하도록 설정할 수 있다.

장점
* 스레드가 미리 생성되어 있으므로 스레드를 생성하고 종료하는 비용이 절약되고 응답 시간이 빠르다.
* 생성 가능한 스레드의 최대치가 있으므로 너무 많은 요청이 들어와도 기존 요청은 안전하게 처리할 수 있다.

실무 팁
* WAS의 주요 튜닝 포인트는 최대 스레드(Max Thread)수이다.
* 이 값을 너무 낮게 설정하면 ?
  * 동시 요청이 많으면, 서버 리소스는 여유롭지만 클라이언트는 금방 응답 지연
* 이 값을 너무 높게 설정하면 ?
  * 동시 요청이 많으면, CPU, 메모리 리소스 임계점 초과로 서버 다운
* 장애 발생시 ?
  * 클라우드면 일단 서버부터 늘리고 이후에 튜닝
* 클라우드가 아니면 열심히 튜닝

![img_20](https://user-images.githubusercontent.com/79847020/159015732-b6a1387b-8e6d-4308-a00b-bc0bd7891898.png)

스레드 풀 적정 숫자
* 애플리케이션, 로직의 복잡도, CPU, 메모리, IO 리소스 상황에 따라 모두 다름
* 성능 테스트
  * 최대한 실제 서비스와 유사한 성능 테스트 시도
  * 툴 : 아파치 ab, 제이미터, nGrinder

핵심
* 멀티 스레드에 대한 부분은 WAS가 처리
* 개발자가 멀티 스레드 관련 코드 신경쓰지 않아도된다.
* 싱글 스레드 프로그래밍 하듯 편리하게 개발
* 하지만 멀티 스레드 환경이므로 싱글톤 객체(서블릿, 스프링빈)는 주의

## 1.4 HTML, HTTP API, CSR, SSR

![img_21](https://user-images.githubusercontent.com/79847020/159015740-b5fe665e-d422-484b-b08e-586d33ad5666.png)

정적 리소스
* 고정된 HTML 파일, CSS, JS, 이미지, 영상 등을 제공
* 주로 웹 브라우저에서 요청
* 정적 리소스에 대한 요청잉 오면 Web Server에서 해당 리소스를 바로 제공한다.  

![img_22](https://user-images.githubusercontent.com/79847020/159015747-58f16594-67d3-43c5-9e0f-c84824ea600e.png)

HTML 페이지
* 동적으로 필요한 HTML 파일을 생성해서 전달
* 웹 브라우저는 HTML 해석

![img_23](https://user-images.githubusercontent.com/79847020/159015758-a919c95f-24ae-4fc2-b138-b874815f840a.png)

![img_24](https://user-images.githubusercontent.com/79847020/159015764-2d0422e7-9024-4cd7-bd09-0e1771257d67.png)

HTTP API
* HTML이 아니라 데이터를 전달
* 주로 JSON 형태로 통신
* 다양한 시스템에서 호출
  * UI가 필요하면 UI 클라이언트가 별도 처리
    * 앱 클라이언트(아이폰, 안드로이드, PC웹)
    * 웹 브라우저에서 자바스크립트를 통한 HTTP API호출
    * React, Vue.js 같은 웹 클라이언트
  * 서버 to 서버
    * 주문 서버 -> 결제
    * 기업간 데이터 통신

### CSR, SSR
![img_25](https://user-images.githubusercontent.com/79847020/159015871-bd1ea28c-3481-45c7-96f5-2838985767d4.png)
* SSR - 서버사이드 렌더링
  * 서버에서 최종 HTML을 생성해서 클라이언트에 전달
  * HTML 최종 결과를 서버에서 만들어서 웹브라우저에 전달
  * 주로 정적인 화면에 사용
  * 관련기술 : JSP, 타임리프 -> 백엔드 개발자

![img_26](https://user-images.githubusercontent.com/79847020/159015881-e1dbcbcf-bf98-4c28-a23c-2f082db2083c.png)
* CSR - 클라이언트 사이드 렌더링
  * HTML 결과를 자바스크립트를 사용해 웹 브라우저에서 동적으로 생성해서 적용
  * 주로 동적인 화면에 사용, 웹 환경을 마치 앱 처럼 필요한 부분부분 변경할 수 있음
  예) 구글 지도, Gmail, 구글 캘린더
  * 관련기술 : React, Vue.js > 웹 프론트엔드 개발자

* 참고
  * React, Vue.js를 CSR + SSR 동시에 지원하는 웹 프레임워크도 있음
  * SSR을 사용하더라도 자바스크립트를 사용해서 화면 일부를 동적으로 변경가능

### 어디까지 알아야 할까?

백엔드 개발자 - 서버 사이드 렌더링 기술
* JSP, 타임리프
* 화면이 정적이고 복잡하지 않을 때 사용
* 백엔드 개발자는 서버 사이드 렌더링 기술 학습 필요

웹 프론트엔드 - 클라이언트 사이드 렌더링 기술
* React, Vue.js
* 복잡하고 동적인 UI 사용
* 웹 프론트엔드 개발자의 전문 분야

선택과 집중
* 백엔드 개발자의 웹 프론트엔드 기술 학습은 옵션
* 백엔드 개발자는 서버, DB, 인프라 등등 수 많은 백엔드 기술을 공부해야 한다.
* 웹 프론트엔드도 깊이있게 잘 하려면 숙련에 오랜 시간이 필요하다.

## 1.5 자바 백엔드 웹 기술 역사

### 과거 기술

* 서블릿 - 1997년
  * HTML 생성이 어려움 (Java코드로 HTML 작성)
* JSP - 1999년
  * HTML 생성은 편리하지만, 비즈니스 로직까지 너무 많은 역할 담당
* 서블릿, JSP 조합 MVC 패턴 사용
  * 모델, 뷰, 컨트롤러 역할을 나누어 개발
* MVC 프레임워크 춘추 전국 시대 - 2000년초 ~ 2010년초
  * MVC 패턴 자동화, 복잡한 웹 기술을 편리하게 사용할 수 있는 다양한 기능 지원
  * 스트럿츠, 웹워크, 스프링 MVC

### 현재 사용 기술

* 애노테이션 기반의 스프링 MVC 등장
  * @Controller
  * MVC 프레임워크의 춘추 전국 시대 마무리
* 스프링 부트의 등장
  * 스프링 부트는 서버를 내장
  * 과거에는 서버에 WAS를 직접 설치하고 소스는 War 파일을 만들어서 설치한 WAS에 배포.
  * 스프링 부트는 빌드 결과(Jar)에 WAS 서버 포함 -> 빌드 배포 단순화

### 최신 기술

* 스프링 웹 기술의 분화
  * Web Servlet - Spring MVC (서블릿 기반)
  * Web Reactive - Srping WebFlux

* 스프링 웹 플럭스(WebFlux)
  * 비동기 넌 블러킹 처리 (Java를 Node.js 처럼 개발)
  * 최소 스레드로 최대 성능 - 스레드 컨텍스트 스위칭 비용 효율화
    * CPU 코어수가 4개가 있다면, 스레드수를 CPU 코어 수와 엇비슷하게 맞춘다. 스레드가 코어를 점유해서 계속 돌아가기 때문에 컨텍스트 스위칭이 발생하지 않는다.
  * 함수형 스타일로 개발 - 동시처리 코드 효율화
  * 서블릿 기술 사용 X (Netty 웹 프레임워크 사용)
* 그런데
  * 웹 플럭스는 기술적 난이도 높음. (성능이 중요하고 로직이 복잡한 경우에만 고려되고 있다.)
  * 아직은 RDB 지원 부족
  * 일반 MVC의 스레드 모델도 충분히 빠르다.
  * 실무에서 아직 많이 사용되지는 않음.

### 자바 뷰 템플릿 역사

* 뷰 템플릿
  * HTML을 백엔드에서 동적으로 생성하는 뷰 기술을 뷰 템플릿이라 한다.

* JSP
  * 속도 느림, 기능 부족
* 프리마터(Freemarker), Velocity(벨로시티)
  * 속도 문제 해결, 다양한 기능
* 타임리프(Thymeleaf)
  * 내추럴 템플릿 : HTML 모양을 유지하면서 뷰 템플릿 적용 가능
  * 스프링 MVC의 강력한 기능 통합
  * 최선의 선택, 단 성느은 프리마터, 벨로시티가 더 빠름

# 2. 서블릿

## 2.1 서블릿 - 프로젝트 생성
Intellij 기본 설정
* 빌드 설정
  * Preferences -> Build,Execution,Deployment -> Build Tools -> Gradle
  * Build and run using : Gradle -> Intellij IDEA
  * Runt tests using : Gradle -> Intellij IDEA
    ![img](https://user-images.githubusercontent.com/79847020/156157437-46f7f46b-de75-4d10-a4f2-5a77db96a06a.png)

lombok 설정

build.grald
```
//lombok 설정 추가 시작
configurations {
    compileOnly {
        extendsFrom annotationProcessor
    }
}
//lombok 설정 추가 끝


    //lombok 라이브러리 추가 시작
    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'

    testCompileOnly 'org.projectlombok:lombok'
    testAnnotationProcessor 'org.projectlombok:lombok'
    //lombok 라이브러리 추가 끝
```

Preference(윈도우 File > Setting) -> plugin -> lombok 검색 설치 Preference(윈도우 File > Setting) -> Annotation Processors 검색 ->
Enable Annotation Processing 체크

## 2.2 Hello 서블릿

### 스프링 부트 서블릿 환경 구성

스프링 부트 환경에서 서블릿을 등록하고 사용해보자.

서블릿은 톰캣 같은 웹 애플리케이션 서버를 직접 설치하고 그 위에 서블릿 코드를 클래스파일로 빌드해서 올린 다음, 톰캣 서버를 실행하면 된다. 스프링 부트는 톰캣 서버를 내장하고 있으므로 톰캣 서버 설치 없이 편리하게 서블릿 코드를 실행할 수 있다.

@ServletComponentScan
스프링 부트는 서블릿을 직접 등록해서 사용할 수 있도록 @ServletComponentScan을 지원한다.

hello.servlet.ServletApplication
```java
@SpringBootApplication
@ServletComponentScan //서블릿 자동 등록
public class ServletApplication {

    public static void main(String[] args) {
        SpringApplication.run(ServletApplication.class, args);
    }
}
```

### 서블릿 등록하기

hello.servlet.basic.HelloServlet
```java
@WebServlet(name = "helloSevlet", urlPatterns = "/hello")
public class HelloServlet extends HttpServlet {
    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("HelloServlet.service");
        System.out.println("request = " + request);
        System.out.println("response = " + response);

        String username = request.getParameter("username");

        System.out.println("username = " + username);

        response.setContentType("text/plane");
        response.setCharacterEncoding("utf-8");
        response.getWriter().write("hello " + username);
    }
}
```

@WebServlet : 서블릿 애노테이션
(name : 서블릿 이름, urlPatterns : URL 매핑)

HTTP 요청을 통해 매핑된 URL이 호출되면 서블릿 컨테이너는 다음 메서드를 실행핸다.

```text
protected void service(HttpServletRequest request, HttpServletResponse response) 
```

http://localhost:8080/hello?username=world
![img_27](https://user-images.githubusercontent.com/79847020/159015932-635bca05-0b48-4dc5-95ce-fef163ea18c6.png)
console
```text
HelloServlet.service
request = org.apache.catalina.connector.RequestFacade@62447096
response = org.apache.catalina.connector.ResponseFacade@6d4a9214
username = world
```

### Http 요청 메시지 로그로 확인하기

application.properties
```text
logging.level.org.apache.coyote.http11=debug
```
log
```text
Received [GET /favicon.ico HTTP/1.1
Host: localhost:8080
Connection: keep-alive
sec-ch-ua: " Not A;Brand";v="99", "Chromium";v="98", "Google Chrome";v="98"
sec-ch-ua-mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/98.0.4758.102 Safari/537.36
sec-ch-ua-platform: "Windows"
Accept: image/avif,image/webp,image/apng,image/svg+xml,image/*,*/*;q=0.8
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: no-cors
Sec-Fetch-Dest: image
Referer: http://localhost:8080/hello?username=world
Accept-Encoding: gzip, deflate, br
Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
]
```

### 서블릿 컨테이너 동작 방식 설명

내장 톰캣 서버 생성

![img_28](https://user-images.githubusercontent.com/79847020/159015956-bdc5d82f-c9b4-4ba2-867f-94ebd2ab12d7.png)

![img_29](https://user-images.githubusercontent.com/79847020/159015971-8f63b445-ae07-405e-97f4-8d1c1914c0e0.png)

![img_30](https://user-images.githubusercontent.com/79847020/159015976-1d925d75-98d8-46ca-95c4-5f98173e2fa2.png)

참고 : Http 응답에서 Content-Length는 웹 애플리케이션 서버가 자동으로 생성해준다.

### welcome 페이지 추가

webapp>index.html 생성하면 http://localhost:8080 접속 시 index.html 페이지가 열린다.

![img_31](https://user-images.githubusercontent.com/79847020/159015985-bc5ccc75-fb49-4878-93bf-842db20fa662.png)

## 2.3 HttpServletRequest 개요

HTTP 요청 메시지를 개발자가 직접 파싱해서 사용해도 되지만 서블릿은 개발자가 HTTP 요청 메시지를 편리하게 사용할 수 있도록 HTTP 요청 메시지를 파싱해준다. 그 결과가 HttpServletRequest 객체에 담겨 제공된다.

```text
POST /save HTTP/1.1
Host: localhost:8080
Content-Type: application/x-www-form-urlencoded

username=kim&age=20
```

Http 요청 Message 구조
* START LINE
  * HTTP 메소드
  * URL
  * 쿼리 스트링
  * 스키마, 프로토콜
* 헤더
  * 헤더 조회
* 바디
  * form 파라미터 형식 조회
  * message body 데이터 직접 조회

Http 요청 Message를 파싱해서 담고 있을 뿐만 아니라 HttpServletRequest 객체는 부가기능을 제공한다.

HttpServletRequest 부가기능
1. 임시 저장소 기능
   1. 저장 : request.setAttribute(name, value);
   2. 읽기 : request.getAttribute(name);
2. 세션 관리 기능
   1. request.getSession(crate: true);

중요
* HttpServletRequest, HttpServletResponse를 사용할 때 가장 중요한 점은 이 객체들이 HTTP 요청 메시지, HTTP 응답 메시지를 편리하게 사용하도록 도와주는 객체이라는 점이다. 이 기능에 대해서 깊이있는 이해를 하려면 HTTP 스펙이 제공하는 요청, 응답 메시지 자체를 이해해야 한다.

## 2.4 HttpServletRequest 기본 사용법

### HttpServletRequest - HttpRequestMessage StartLine
RequestHeaderServlet.java
```java
@WebServlet(name = "requestHeaderServlet", urlPatterns = "/request-header")
public class RequestHeaderServlet extends HttpServlet {
    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        printStartLine(request);
        response.getWriter().write("ok");
    }

    //start line 정보
    private void printStartLine(HttpServletRequest request) {
        System.out.println("--- REQUEST-LINE - start ---");
        System.out.println("request.getMethod() = " + request.getMethod()); //GET
        System.out.println("request.getProtocal() = " + request.getProtocol()); //        HTTP / 1.1
        System.out.println("request.getScheme() = " + request.getScheme()); //http

        // http://localhost:8080/request-header
        System.out.println("request.getRequestURL() = " + request.getRequestURL());
        // /request-test
        System.out.println("request.getRequestURI() = " + request.getRequestURI());
        //username=hi
        System.out.println("request.getQueryString() = " + request.getQueryString());
        System.out.println("request.isSecure() = " + request.isSecure()); //https 사용 유무
        System.out.println("--- REQUEST-LINE - end ---");
        System.out.println();
    }
}
```
http://localhost:8080/request-header?username=hi

console
```java
--- REQUEST-LINE - start ---
request.getMethod() = GET
request.getProtocal() = HTTP/1.1
request.getScheme() = http
request.getRequestURL() = http://localhost:8080/request-header
request.getRequestURI() = /request-header
request.getQueryString() = username=hi
request.isSecure() = false
--- REQUEST-LINE - end ---
```

### HttpServletRequest HttpRequestMessage Header

RequestHeaderServlet.java
```java
    //Header 모든 정보
    private void printHeaders(HttpServletRequest request) {
        System.out.println("--- Headers - start ---");

//        Enumeration<String> headerNames = request.getHeaderNames();
//        while (headerNames.hasMoreElements()) {
//            String headerName = headerNames.nextElement();
//            System.out.println(headerName + ": " + request.getHeader(headerName));
//        }

        request.getHeaderNames().asIterator()
                .forEachRemaining(headerName -> System.out.println(headerName + ": " + request.getHeader(headerName)));
        System.out.println("--- Headers - end ---");
        System.out.println();
    }
```
http://localhost:8080/request-header?username=hi

console
```java
--- Headers - start ---
host: localhost:8080
connection: keep-alive
cache-control: max-age=0
sec-ch-ua: " Not A;Brand";v="99", "Chromium";v="98", "Google Chrome";v="98"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
upgrade-insecure-requests: 1
user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/98.0.4758.102 Safari/537.36
accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
sec-fetch-site: none
sec-fetch-mode: navigate
sec-fetch-user: ?1
sec-fetch-dest: document
accept-encoding: gzip, deflate, br
accept-language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
--- Headers - end ---
```

### HttpServletRequest HttpRequestMessage HeaderUtil
RequestHeaderServlet.java
```java
//Header 편리한 조회
private void printHeaderUtils(HttpServletRequest request) {
    System.out.println("--- Header 편의 조회 start ---");
    System.out.println("[Host 편의 조회]");
    System.out.println("request.getServerName() = " + request.getServerName()); //Host 헤더
    System.out.println("request.getServerPort() = " + request.getServerPort()); //Host 헤더
    System.out.println();
    System.out.println("[Accept-Language 편의 조회]");
    request.getLocales().asIterator().forEachRemaining(locale -> System.out.println("locale = " +
            locale));
    System.out.println("request.getLocale() = " + request.getLocale());
    System.out.println();
    System.out.println("[cookie 편의 조회]");
    if (request.getCookies() != null) {
        for (Cookie cookie : request.getCookies()) {
            System.out.println(cookie.getName() + ": " + cookie.getValue());
        }
    }
    System.out.println();
    System.out.println("[Content 편의 조회]");
    System.out.println("request.getContentType() = " +
            request.getContentType());
    System.out.println("request.getContentLength() = " + request.getContentLength());
    System.out.println("request.getCharacterEncoding() = " + request.getCharacterEncoding());
    System.out.println("--- Header 편의 조회 end ---");
    System.out.println();
}
```
http://localhost:8080/request-header?username=hi

console
```java
--- Header 편의 조회 start ---
[Host 편의 조회]
request.getServerName() = localhost
request.getServerPort() = 8080

[Accept-Language 편의 조회]
locale = ko_KR
locale = ko
locale = en_US
locale = en
request.getLocale() = ko_KR

[cookie 편의 조회]

[Content 편의 조회]
request.getContentType() = null
request.getContentLength() = -1
request.getCharacterEncoding() = UTF-8
--- Header 편의 조회 end ---
```

### HttpServletRequest 기타 정보

기타정보는 HTTP 메시지 정보는 아니다.

```java
private void printEtc(HttpServletRequest request) {
    System.out.println("--- 기타 조회 start ---");
    System.out.println("[Remote 정보]");
    System.out.println("request.getRemoteHost() = " + request.getRemoteHost()); //
    System.out.println("request.getRemoteAddr() = " + request.getRemoteAddr()); //
    System.out.println("request.getRemotePort() = " + request.getRemotePort()); //
    System.out.println();
    System.out.println("[Local 정보]");
    System.out.println("request.getLocalName() = " + request.getLocalName()); //
    System.out.println("request.getLocalAddr() = " + request.getLocalAddr()); //
    System.out.println("request.getLocalPort() = " + request.getLocalPort()); //
    System.out.println("--- 기타 조회 end ---");
    System.out.println();
}
```
http://localhost:8080/request-header?username=hi

console
```java
--- 기타 조회 start ---
[Remote 정보]
request.getRemoteHost() = 0:0:0:0:0:0:0:1
request.getRemoteAddr() = 0:0:0:0:0:0:0:1
request.getRemotePort() = 62984

[Local 정보]
request.getLocalName() = 0:0:0:0:0:0:0:1
request.getLocalAddr() = 0:0:0:0:0:0:0:1
request.getLocalPort() = 8080
--- 기타 조회 end ---
```

참고
* 로컬에서 테스트 하면 IPv6정보가 나오는데 IPv4정보를 보고 싶으면 다음 옵션을 VM Option에 넣어주면 된다

```java
-Djava.net.preferIPv4Stack=true
```

## 2.5 HTTP 요청 데이터 - 데이터

### HTTP 요청 데이터 - 개요

HTTP 요청 메시지를 통해 클라이언트에서 서버로 데이터를 전달하는 방법을 알아보자.

주로 3가지를 사용한다.
1. GET - 쿼리 파라미터
   * /url?username=hello&age=20
   * MessageBody 없이 URL의 쿼리파라미터에 데이터를 전달
   * 주로 검색, 필터, 페이징 등에서 많이 사용하는 방식
2. POST - HTML FORM
   * content-type:application/x-www.form-urlencoded
   * 메시지 바디에 쿼리 파라미터 형식으로 전달 username=hello&age=20
   * 회원 가입, 상품 주문, HTML Form 사용
3. HTTP message body에 데이터를 직접 담아서 전달
   * HTTP API에서 주로 사용
   * 주로 JSON 사용, 그외 XML, TEXT
   * HTTP Method : POST, PUT, PATCH

## 2.6 HTTP 요청 데이터 - GET 쿼리 파라미터

다음 데이터를 클라이언트에서 서버로 전송해보자.

```
username=hello
age=20
```
MessageBody가 아닌 URL의 쿼리 파라미터를 사용해서 데이터를 전달하자. 주로 검색, 필터, 페이징 등에서 많이 사용하는 방식

쿼리 파라미터는 URL에 다음과 같이 ? 를 시작으로 보낼 수 있다. 추가 파라미터는 & 으로 구분한다.

http://localhost:8080/request-param?username=hello&age=20

서버에서는 HttpServletRequest가 제공하는 메서드를 통해 쿼리 파라미터를 편리하게 조회할 수 있다.

```java
String username = request.getParameter("username")
Enumeration<String> parameterNames = request.getParameterNames();
Map<String, String[]> parameterMap = request.getParameterMap();
String[] username = request.getParameterValeus("username");
```

RequestParamServlet.java
```java
/**
 * 1. 파라미터 전송 기능
 * http://localhost:8080/request-param?username=hello&age=28
 * 
 * http://localhost:8080/request-param?username=hello&username=howareu&age=28
 */
@WebServlet(name = "requestParamServlet", urlPatterns = "/request-param")
public class RequestParamServlet extends HttpServlet {

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("RequestParamServlet.service");

        System.out.println("[전체 파라미터 조회] - start");

        request.getParameterNames().asIterator()
                .forEachRemaining(paramName -> System.out.println(paramName + "=" + request.getParameter(paramName)));

        System.out.println("[전체 파라미터 조회] - end");
        System.out.println();

        System.out.println("[단일 파라미터 조회] - start");

        String username = request.getParameter("username");
        String age = request.getParameter("age");
        System.out.println();

        System.out.println("[이름이 같은 복수 파라미터 조회");
        System.out.println("request.getParameterValues(username)");
        String[] usernames = request.getParameterValues("username");
        for (String name : usernames) {
            System.out.println("username = " + name);
        }

        response.getWriter().write("ok");
    }
}
```
http://localhost:8080/request-param?username=hello&username=kim&age=20
console
```text
RequestParamServlet.service
[전체 파라미터 조회] - start
username=hello
age=20
[전체 파라미터 조회] - end

[단일 파라미터 조회] - start

[이름이 같은 복수 파라미터 조회
request.getParameterValues(username)
username = hello
username = kim
```

복수 파라미터에서 단일 파라미터 조회
* username=hello&username=kim 과 같이 파라미터 이름은 하나인데 값이 중복이면 어떻게 될까? request.getParameter()는 하나의 파라미터 이름에 대해서 단 하나의 값만 있을 때 사용해야 한다. 지금처럼 중복일 때는 request.getParameterValues()를 사용해야 한다. 
* 참고로 이렇게 중복일 때 request.getParameter()를 사용하면 request.getParameterValues()의 첫번째 값이 반환된다.

## 2.7 HTTP 요청 데이터 - POST HTML Form

이번에는 HTML의 FOrm을 사용해서 클라이언트에서 서버로 데이터를 전송해보자.
주로 회원 가입, 상품 주문 등에서 사용하는 방식이다.

특징
* content-type: application/x-www-form-urlencoded
* 메시지 바디에 쿼리 파라미터 형식으로 데이터를 전달한다. ex) username=hello&age=20

hello-form.html
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<form action="/request-param" method="post">
    username: <input type="text" name="username"/>
    age: <input type="text" name="age"/>
    <button type="submit">전송</button>
</form>
</body>
</html>
```
http://localhost:8080/basic/hello-form.html

![img_32](https://user-images.githubusercontent.com/79847020/159016020-ea9c4e03-53e3-4673-8c85-bfc1e6cb0adf.png)

주의
* 웹 브라우저가 결과를 캐시하고 있어서, 과거에 작성했던 html 결과가 보이는 경우도 있다. 이때는 웹 브라우저의 새로 고침을 직접 선택해주면 된다. 물론 서버를 재시작 하지 않아서 그럴 수 있다.

POST의 HTML Form을 전송하면 웹 브라우저는 다음 형식으로 HTTP 메시지를 만든다. (웹 브라우저 개발자 모드 확인)
* 요청 URL : http://localhost:8080/request-param
* content-type : application/x-www-form-urlencoded
* message body : username=hello&age=20

![img_33](https://user-images.githubusercontent.com/79847020/159016030-f84fa79f-b3c0-4469-99d2-d6ab2d0787e5.png)

![img_34](https://user-images.githubusercontent.com/79847020/159016038-ed49ca80-c6d2-4f2e-bfd0-c1909a342970.png)

application/x-www-from-urlencoded 형식은 앞서 GET에서 살펴본 쿼리 파라미터 형식과 같다. 따라서 쿼리 파라미터 조회 메서드를 그대로 사용하면 된다. 클라이언ㅌ(웹 브라우저) 입장에서는 두 방식에 차이가 있지만 서버 입장에서는 둘의 형식이 동일하므로 request.getParameter()로 편리하게 구분없이 조회할 수 있다.

참고
* content-type는 HTTP Message Body의 데이터 형식을 지정한다.
* GET URL 쿼리 파라미터 형식은 클라이언트에서 서버로 데이터를 전달 할 때는 HTTP 메시지 바디를 사용하지 않기 때문에 content-type이 없다.
* POST HTML Form 형식으로 데이터를 전달하면 HTTP 메시지 바디에 해당 데이터를 포함해서 보내기 때문에 바디에 포함된 데이터가 어떤 형식인지 content-type을 꼭 지정해야 한다. 이렇게 폼으로 데이터를 전송하는 형식을 application/x-www-form-urlencoded라 한다.

Postman을 이용한 테스트
* POST 전송시
  * body : x-www-form-urlencoded
  * Header에서 content-type : application/x-www-form-urlencoded로 지정된 부분 확인

## 2.8 HTTP 요청 데이터 - API 메시지 바디 - 단순 텍스트

HTTP Message Body에 데이터를 직접 담아서 요청
* HTTP API에서 주로 사용
* 데이터 형식은 주로 JSON 사용, 그 외 XML, TEXT
* POST, PUT, PATCH

HTTP Message Body의 데이터를 서블리에서는 InputSTream을 사용해서 직접 읽을 수 있다.

RequestBodyStringServlet
```java
@WebServlet(name = "requestBodyStringServlet", urlPatterns = "/request-body-string")
public class RequestBodyStringServlet extends HttpServlet {
    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        ServletInputStream inputSTream = request.getInputStream();
        String messageBody = StreamUtils.copyToString(inputSTream, StandardCharsets.UTF_8);

        System.out.println("messageBody = " + messageBody);

        response.getWriter().write("OK");
    }
}
```

참고
* inputStream은 byte코드를 반환한다. byte 코드를 우리가 읽을 수 있는 문자(String)로 보려면 문자표(Charset)을 설정해주어야 한다. UTF-8 Charset을 지정했다.

문자 전송
* POST http://localhost:8080/request-body-string
* content-type : text/plain
* http-message-body : hello

console
```
messageBody = hello
```

## 2.9 HTTP 요청 데이터 - API 메시지 바디 - JSON

HTTP API에서 주로 사용하는 JSON 형식으로 데이터를 전달해보자.
helloData
```java
@Getter
@Setter
public class HelloData {

    private String username;
    private int age;
}
```
```java
@WebServlet(name = "requestBodyJsonServlet", urlPatterns = "/request-body-json")
public class RequestBodyJsonServlet extends HttpServlet {

    private ObjectMapper objectMapper = new ObjectMapper();

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        ServletInputStream inputStream = request.getInputStream();
        String messageBody = StreamUtils.copyToString(inputStream, StandardCharsets.UTF_8);

        System.out.println("messageBody = " + messageBody);

        HelloData helloData = objectMapper.readValue(messageBody, HelloData.class);

        System.out.println("helloData.username = " + helloData.getUsername());
        System.out.println("helloData.age = " + helloData.getAge());

        response.getWriter().write("ok");
    }
}
```

JSON 형식 전송
* POST http://localhost:8080/request-body-json
* content-type: application/json
* http-message-body: {"username": "hello", "age":20}

console
```
messageBody = {"username":"hello", "age":20}
helloData.username = hello
helloData.age = 20
```

String>JSON 파싱
```java
HelloData helloData = objectMapper.readValue(messageBody, HelloData.class);
```

참고
* JSON 결과를 파싱해서 사용할 수 있는 자바 객체로 변환하려면 jackson, Gson 같은 JSON 변환 라이브러리를 추가해서 사용해야 한다. 스프링 부트로 Spring MVC를 선택하면 기본으로 Jackson 라이브러리 ObjectMapper을 제공한다.

참고
* HTML form 데이터도 메시지 바디를 통해 전송되므로 직접 읽을 수 있다. 하지만 편리한 파라미터 조회 기능(request.getParameter(..))을 이미 제공하기 때문에 Parsing없이 파라미터 조회 기능을 사용하면 된다.

## 2.10 HttpServletResponse - 기본 사용법

HttpServletResponse 역할
1. HTTP 응답 메시지 생성
2. HTTP 응답코드 지정
3. 헤더 생성
4. 바디 생성
5. 부가 기능 제공
   1. ContentType
   2. 쿠키
   3. Redirect

HttpServletResponse 기본 사용법

ResponseHeaderServlet
```java
@WebServlet(name = "responseHeaderServlet", urlPatterns = "/response-header")
public class ResponseHeaderServlet extends HttpServlet {
    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //[status-line]
        response.setStatus(HttpServletResponse.SC_OK);

        //[response-header]
        response.setHeader("Content-Type", "text/plain");
        response.setHeader("Cache-Control", "no-cache, no-stroe, must-revalidate");
        response.setHeader("Pragma", "no-cache");
        response.setHeader("my-header", "hello");

        //[Header 편의 메서드]
        content(response);
        cookie(response);
        redirect(response);

        response.getWriter().write("ok");
    }
```

Content 편의 메서드
```java
    private void content(HttpServletResponse response) {
        //Content-Type: text/plain;charset=utf-8
        // Content-Length: 2
        //response.setHeader("Content-Type", "text/plain;charset=utf-8");     response.setContentType("text/plain");
        response.setCharacterEncoding("utf-8");
        //response.setContentLength(2); //(생략시 자동 생성)
        }
```

쿠키 편의 메서드
```java
    private void cookie(HttpServletResponse response) {
        //Set-Cookie: myCookie=good; Max-Age=600;
        response.setHeader("Set-Cookie", "myCookie=good; Max-Age=600");
        Cookie cookie = new Cookie("myCookie", "good");
        cookie.setMaxAge(600); //600초
        response.addCookie(cookie);
    }
```

Redirect 편의 메서드
```java
    private void redirect(HttpServletResponse response) throws IOException {
        //Status Code 302
        //Location: /basic/hello-form.html
        //response.setStatus(HttpServletResponse.SC_FOUND); //302
        //response.setHeader("Location", "/basic/hello-form.html");
        response.sendRedirect("/basic/hello-form.html");
  }
```

http://localhost:8080//response-header
-> Redirect
http://localhost:8080/basic/hello-form.html

![img_37](https://user-images.githubusercontent.com/79847020/159016106-272d2fe6-cd1b-43b0-b05d-c78bd3af9165.png)

## 2.11 HTTP 응답 데이터 - 단순 텍스트, HTML

HTTP 응답 메시지는 주로 다음 내용을 담아서 전달한다.
1. 단순 텍스트 응답
   * writer.println("ok");
2. HTML 응답
3. HTTP API - MessageBody JSON 응답

ResponseHtmlServlet
```java
@WebServlet(name = "responseHtmlServlet", urlPatterns = "/response-html")
public class ResponseHtmlServlet extends HttpServlet {
    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // Content-type; text/html;charset=utf-8
        response.setContentType("text/html");
        response.setCharacterEncoding("utf-8");

        PrintWriter writer = response.getWriter();
        writer.write("<html>");
        writer.write("<body>");
        writer.write("<div>안녕</div>");
        writer.write("</body>");
        writer.write("</html>");
    }
}
```
HTTP 응답으로 HTML을 반환할 때는 content-type을 text/html로 지정해야 한다.

브라우저 소스 보기
```html
<html>
<body>
<div>안녕</div>
</body>
</html>
```
![img_38](https://user-images.githubusercontent.com/79847020/159016124-899424ee-b8d7-4877-89da-baeca4516c57.png)

## 2.12 HTTP 응답 데이터 - API JSON

```java
@WebServlet(name = "responseJsonServlet", urlPatterns = "/response-json")
public class ResponseJsonServlet extends HttpServlet {

    ObjectMapper objectMapper = new ObjectMapper();

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //Content-type: application/json
        response.setContentType("application/json");
        response.setCharacterEncoding("utf-8");

        HelloData helloData = new HelloData();
        helloData.setUsername("test");
        helloData.setAge(99);

        //{"username":"kim", "age":20}
        String result = objectMapper.writeValueAsString(helloData);
        response.getWriter().write(result);
    }
}
```

HTTP 응답으로 JSON을 반환할 때는 content-type을 application/json로 지정해야 한다. Jackson 라이브러리가 제공하는 objectMapper.writeValueAsString()을 사용하면 객체를 JSON문자로 변경할 수 있다.

ObjectMapper
* HttpBodyMessage(String) -> JSON
  ```java
  ObjectMapper objectMapper = new ObjectMapper();
  HelloData helloData = objectMapper.readValue(messageBody, HelloData.class);  
  ``` 
* JSON -> HttpBodyMessage(String)
  ```java
  ObjectMapper objectMapper = new ObjectMapper();
  String result = objectMapper.writeValueAsString(helloData);
  ```
  
참고
* application/json은 스펙상 utf-8형식으로 사용하도록 정의되어 있다. 그래서 스펙에서 charset=utf-8 같은 추가 파라미터를 지원하지 않는다. 따라서 application/json 이라고만 사용해야지 application/json;charset=utf-8 이라고 전달하는 것은 의미 없는 파라미터를 추가한 것이 된다. 
* 그런데 response.getWriter()는 부가기능으로 추가 파라미터를 자동으로 추가해버린다. 이 때는 response.getOutputStream()으로 출력하면 그런 문제를 피할 수 있다.

# 3. 서블릿, JSP, MVC 패턴
## 3.1 회원 관리 웹 애플리케이션 요구사항

회원 관리 웹 애플리케이션 요구사항

회원정보
* 이름 : username
* 나이 : age

기본 요구사항
* 회원 저장
* 회원 목록 조회

회원 도메일 모델
```java
@Getter
@Setter
public class Member {
    private Long id;
    private String username;
    private int age;

    public Member() {
    }

    public Member(String username, int age) {
        this.username = username;
        this.age = age;
    }
}
```
id는 Member를 회원 저장소에 저장하면 회원 저장소가 할당한다.

회원 저장소
```java
/**
 * 동시성 문제가 고려되어 있지 않음, 실무에서는 ConcurrentHashMap, AtomicLong 사용 고려
 */
public class MemberRepository {
    private Map<Long, Member> store = new HashMap<>();

    private static long sequence = 0L;

    private static final MemberRepository insatnce = new MemberRepository();

    private MemberRepository() {
    }

    public static MemberRepository getInstance() {
        return insatnce;
    }

    public Member save(Member member) {
        member.setId(++sequence);
        store.put(member.getId(), member);
        return member;
    }

    public Member findByID(Long id) {
        return store.get(id);
    }

    public List<Member> findAll() {
        return new ArrayList<>(store.values());
    }

    public void clearStore() {
        store.clear();
    }
}
```

회원 저장소는 싱글톤 패턴을 적용했다. 스프링을 사용하면 스프링 빈으로 등록하면 되지만 지금은 최대한 스프링 없이 순수 서블릿 만으로 구현하는 것이 목적이다. 싱글톤 패턴은 객체를 단 하나만 생성해서 공유해야 하므로 생성자를 private 접근자로 막아둔다.

회원 저장소 테스트 코드
```java
class MemberRepositoryTest {

    MemberRepository memberRepository = MemberRepository.getInstance();

    @AfterEach
    void afterEach() {
        memberRepository.clearStore();
    }

    @Test
    public void save() throws Exception {
        //given
        Member member = new Member("hello", 20);

        //when
        Member savedMember = memberRepository.save(member);

        //then
        Member findMember = memberRepository.findByID(savedMember.getId());

        assertThat(findMember).isEqualTo(savedMember);
    }

    @Test
    public void findAll() throws Exception {
        //given
        Member member1 = new Member("member1", 20);
        Member member2 = new Member("member2", 30);

        memberRepository.save(member1);
        memberRepository.save(member2);

        //when
        List<Member> members = memberRepository.findAll();

        //then
        assertThat(members.size()).isSameAs(2);
        assertThat(members).contains(member1, member2);
    }
}
```
## 3.2 서블릿 - 회원 관리 웹 애플리케이션 만들기

서블릿으로 회원 관리 웹 애플리케이션을 만들어보자.

가장 먼저 서블릿으로 회원 등록 HTML 폼을 제공해보자.

MemberFormServlet
```java
@WebServlet(name = "memberFormServlet", urlPatterns = "/servlet/members/new-form")
public class MemberFormServlet extends HttpServlet {

    private MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html");
        response.setCharacterEncoding("utf-8");

        PrintWriter w = response.getWriter();
        w.write("<!DOCTYPE html>\n" +
                "<html>\n" +
                "<head>\n" +
                "    <meta charset=\"UTF-8\">\n" +
                "    <title>Title</title>\n" +
                "</head>\n" +
                "<body>\n" +
                "<form action=\"/servlet/members/save\" method=\"post\">\n" +
                "    username: <input type=\"text\" name=\"username\" />\n" +
                "    age:      <input type=\"text\" name=\"age\" />\n" +
                "    <button type=\"submit\">전송</button>\n" +
                "</form>\n" +
                "</body>\n" +
                "</html>\n");
    }
}
```

MemberFormServlet은 단순하게 회원 정보를 입력할 수 있는 HTML Form을 만들어 응답한다.

http://localhost:8080/servlet/members/new-form

![img_39](https://user-images.githubusercontent.com/79847020/159016145-18c07d83-3603-4ace-86d0-b60c527c749b.png)

이번에는 HTML Form에서 데이터를 입력하고 전송을 누르면 실제 회원 데이터가 저장되도록 해보자.

```java
@WebServlet(name = "memberSaveServlet", urlPatterns = "/servlet/members/save")
public class MemberSaveServlet extends HttpServlet {

    MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("MemberSaveServlet.service");
        String username = request.getParameter("username");
        int age = Integer.parseInt(request.getParameter("age"));

        Member member = new Member(username, age);
        memberRepository.save(member);

        response.setContentType("text/html");
        response.setCharacterEncoding("utf-8");
        PrintWriter w = response.getWriter();
        w.write("<html>\n" +
                "<head>\n" +
                "    <meta charset=\"UTF-8\">\n" +
                "</head>\n" +
                "<body>\n" +
                "성공\n" +
                "<ul>\n" +
                "    <li>id=" + member.getId() + "</li>\n" +
                "    <li>username=" + member.getUsername() + "</li>\n" +
                "    <li>age=" + member.getAge() + "</li>\n" +
                "</ul>\n" +
                "<a href=\"/index.html\">메인</a>\n" +
                "</body>\n" +
                "</html>");
    }
}
```

MemberSaveServlet은 다음 순서로 동작한다.
1. 파라미터를 조회해서 Member 객체를 만든다.
2. Member 객체를 MemberRepository를 통해서 저장한다.
3. Member 객체를 사용해서 결과 화면을 HTML을 동적으로 만들ㅇ어서 응답한다.

http://localhost:8080/servlet/members/new-form

![img_40](https://user-images.githubusercontent.com/79847020/159016159-d1f2f3c5-d1e8-41b0-abc6-16b7b7b08a4e.png)

![img_41](https://user-images.githubusercontent.com/79847020/159016171-395b18d9-b387-420d-b857-bd2c598a9979.png)

MemberListServlet 회원 목록
```java
@WebServlet(name = "memberListServlet", urlPatterns = "/servlet/members")
public class MemberListServlet extends HttpServlet {

    MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html");
        response.setCharacterEncoding("utf-8");
        List<Member> members = memberRepository.findAll();
        PrintWriter w = response.getWriter();
        w.write("<html>");
        w.write("<head>");
        w.write("    <meta charset=\"UTF-8\">");
        w.write("    <title>Title</title>");
        w.write("</head>");
        w.write("<body>");
        w.write("<a href=\"/index.html\">메인</a>");
        w.write("<table>");
        w.write("    <thead>");
        w.write("    <th>id</th>");
        w.write("    <th>username</th>");
        w.write("    <th>age</th>");
        w.write("    </thead>");
        w.write("    <tbody>");

        for (Member member : members) {
            w.write("    <tr>");
            w.write("        <td>" + member.getId() + "</td>");
            w.write("        <td>" + member.getUsername() + "</td>");
            w.write("        <td>" + member.getAge() + "</td>");
            w.write("    </tr>");
        }

        w.write("    </tbody>");
        w.write("</table>");
        w.write("</body>");
        w.write("</html>");
    }
}
```

MemberListServlet은 다음 순서로 동작한다.
1. memberRepository.findAll()을 통해 모든 회원을 조회한다.
2. 회원 목록 HTML을 for 루프를 통해서 회원 수 만큼 동적으로 생성하고 응답한다.

http://localhost:8080/servlet/members

![img_42](https://user-images.githubusercontent.com/79847020/159016189-1378c098-4b7e-47cf-8079-1f35a7f3275d.png)

템플릿 엔진으로
* 지금까지 서블릿과 자바 코드만으로 HTML을 만들어 보았다. 서블릿 덕분에 동적으로 원하는 HTML을 마음껏 만들 수 있다. 정적인 HTML문서라면 화면이 계속 달라지는 회원의 저장 결과라던가 회원 목록 같은 동적인 HTML을 만드는 일은 불가능 할 것이다.
* 코드에서 코드에서 보듯이 이것은 매우 복잡하고 비효율적이다. 자바 코드로 HTML을 만들어 내는 것보다 차라리 HTML 문서에 동적으로 변경해야 하는 부분만 자바 코드를 넣을 수 있다면 더 편리할 것이다.
* 이것이 바로 템플릿 엔진이 나온 이유다. 템플릿 엔진을 사용하면 HTML 문서에서 필요한 곳만 코드를 적용해서 동적으로 변경할 수 있다.
* 템플릿 엔진에는 JSP, Thymeleaf, Freemaker, Velocity 등이 있다.

참고
* JSP는 성능과 기능면에서 다른 템플릿 엔진과의 경쟁에서 밀리면서 점점 사장되어 가는 추세이다. 템플릿엔진들은 각각 장단점이 있는데 강의에서는 JSP는 앞부분에서 잠깐 다루고 스프링과 잘 통합되는 Thymeleaf를 사용한다.

## 3.3 JSP - 회원 관리 웹 애플리케이션 만들기

JSP 라이브러리 추가
* JSP를 사용하려면 라이브러리를 추가해야 한다.
```
//JSP 추가 시작
implementation 'org.apache.tomcat.embed:tomcat-embed-jasper'
implementation 'javax.servlet:jstl'
//JSP 추가 끝
```

회원 등록 폼 JSP
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<form action="/jsp/members/save.jsp" method="post">
    username: <input type="text" name="username"/>
    age: <input type="text" name="age"/>
    <button type="submit">전송</button>
</form>
</body>
</html>
```

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
```
첫줄은 JSP문서라는 의미다. JSP 문서는 이렇게 시작해야 한다.

회원 등록 폼 JSP를 보면 첫 줄을 제외하고는 완전히 HTML과 같다. JSP는 서버 내부에서 서블릿으로 변환되는데 우리가 만들었던 MemberFormServlet과 거의 비슷한 모습으로 변환된다.

그래서 JSP안에서 "<%","%>" 안에서 Java코드가 동작하고 서블릿에 자동 주입되는 HttpServletRequest, HttpServletResponse를 JSP에서도 사용할 수 있다. Request, Response로 접근할 수 있다. 

http://localhost:8080/jsp/members/new-form.jsp

![img_43](https://user-images.githubusercontent.com/79847020/159016203-79e2a0d2-2f03-4433-a6b4-1cca0aae9d04.png)

회원 저장 JSP

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ page import="hello.servlet.domain.member.Member" %>
<%@ page import="hello.servlet.domain.member.MemberRepository" %>
<%
    MemberRepository memberRepository = MemberRepository.getInstance();

    System.out.println("MemberSaveServlet.service");
    String username = request.getParameter("username");
    int age = Integer.parseInt(request.getParameter("age"));

    Member member = new Member(username, age);
    memberRepository.save(member);
%>
<html>
<head>
    <title>Title</title>
</head>
<body>
성공
<ul>
    <li>id=<%= member.getId() %>
    </li>
    <li>username=<%= member.getUsername() %>
    </li>
    <li>age=<%= member.getAge() %>
    </li>
</ul>
<a href="/index.html">메인</a>
</body>
</html>
```

JSP도 결국 Servlet으로 변경된다. 눈에는 보이지 않지만 자동으로 변환되서 사용된다.
사실 우리가 작성한 Servlet클래스로 변경되서 사용된다고 생각하면 된다.

서블릿과 JSP의 한계
* 서블릿으로 개발할 때는 View 화면을 위한 HTML을 만드는 작업이 자바 코드에 섞여서 지저분하고 복잡해진다. JSP를 사용한 덕분에 뷰를 생성하는 HTML 작업을 깔끔하게 가져가고 중간중간 동적으로 변경이 필요한 부분에만 자바 코드를 적용했다.
* 하지만 코드에 비즈니스 로직과 HTML 뷰 로직이 뒤엉켜 있다. 한 개의 파일에서 너무 많은 관심사를 다루고 있다. 이는 유지보수에 좋지 않다.

MVC 패턴의 등장
* 비즈니스 로직은 서블릿 처럼 다른 곳에서 처리하고 JSP는 목적에 맞게 HTML로 화면을 그리는 일에 집중하도록 하자. 그래서 MVC 패턴이 등장했다.

## 3.4 MVC패턴 - 개요

![img_44](https://user-images.githubusercontent.com/79847020/159016218-97984c27-92aa-47b7-bfcd-88508228fd63.png)

![img_45](https://user-images.githubusercontent.com/79847020/159016225-0020c97c-a4ea-4a60-b7d6-b91acd415864.png)

![img_46](https://user-images.githubusercontent.com/79847020/159016230-a6340975-5784-460c-aad3-bdcf19b9c65a.png)

MVC 등장
* 서블릿과 JSP는 한 곳에서 비즈니스 로직과 뷰 렌더링까지 모두 처리하게 되므로 너무 많은 역할을 담당하게 된다.
* 비즈니스 로직과 뷰 렌더링은 변경 라이프 사이클 다르다. UI를 수정하는 일과 비즈니스 로직을 수정하는 일은 각각 발생한다. 라이프 사이클이 다른 부분이 하나의 코드에서 관리되는 것은 유지보수가 좋지 않다.
* 또한 JSP 같은 뷰 템플릿은 화면을 렌더링 하는데 더 최적화 되있기 때문에 이 부분의 업무만 담당하는 것이 효율적이다.
* Model View Controller
  * MVC 패턴은 지금까지 학습한 것처럼 하나의 서블릿이나 JSP로 처리하던 것을 컨트롤러와 뷰라는 영역으로 서로 역할을 나눈 것을 말한다.
  * Controller : HTTP 요청을 받아서 파라미터를 검증하고 비즈니스 로직을 실행한다. 그리고 뷰에 전달할 데이터를 조회해서 모델에 담는다.
  * Model : 뷰에 출력할 데이터를 담는다. 뷰는 비즈니스 로직이나 데이터 접근을 몰라도 되고 화면을 렌더링 하는 일에 집중할 수 있다.
  * VIew : 모델에 담겨있는 데이터를 사용해서 화면을 그리는 일에 집중한다.

참고
* 컨트롤러에 비즈니스 로직을 둘 수도 있지만 그렇게 되면 또 컨트롤러가 너무 많은 역할을 담당하게 된다. 컨트롤러는 이름 그대로 중간에는 제어하는 역할을 담당한다. 그래서 비즈니스 역할도 수행하기 보다는 비즈니스 역할을 호출하는 역할까지 하는 것이 더 합리적이다. 일반적으로 비즈니스 로직은 서비스 계층에서 담당한다. 거의 비즈니스 로직이 없는 경우 리포지토리는 바로 호출하기도 한다. 

## 3.5 MVC패턴 - 적용

서블릿을 컨트롤러로 사용하고 JSP를 뷰로 사용해서 MVC패턴을 적용해보자.

Model은 HttpServletRequest 객체를 사용한다. request는 내부에 데이터 저장소를 가지고 있는데, request.setAttribute(), request.getAttribute()를 사용하면 데이터를 보관하고 조회할 수 있다.

MvcMemberFormServlet.java
```java
@WebServlet(name = "mvcMemberFormServlet", urlPatterns = "/servlet-mvc/members/new-form")
public class MvcMemberFormServlet extends HttpServlet {

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String VIEW_PATH = "/WEB-INF/views/new-form.jsp";
        RequestDispatcher dispatcher = request.getRequestDispatcher(VIEW_PATH);
        dispatcher.forward(request, response);
    }
}
```

/WEB-INF
* 이 경로안에 JSP가 있으면 외부에서 직접 JSP를 호출할 수 없다. 우리가 기대하는 것은 항상 컨트롤러에를 통해서 JSP를 호출하는 것이다.

redirect vs forward
* 리다이렉트는 실제 클라이언트(웹브라우저)에 응답이 나갔다가 클라이언트가 redirect 경로로 다시 요청한다. 따라서 클라이언트가 인지할 수 있고 URL 경로도 실제로 변경된다. 반면에 포워드는 서버 내부에서 일어나는 호출이기 때문에 클라이언트가 전혀 인지하지 못한다.

회원 등록 폼 뷰
new-form.jsp
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%-- 상대경로 사용, [전체 URL의 속한 계층 경로 + /save] --%>
<form action="save" method="post">
    username: <input type="text" name="username"/>
    age: <input type="text" name="age"/>
    <button type="submit">전송</button>
</form>

</body>
</html>
```

여기서 form 액션을 보면 절대 경로가 아니라 상대 경로하는 것을 확인할 수 있다. 이렇게 상대 경로를 사용하면 폼 전송시 현재 URL이 속한 계층 경로 + save가 호출된다.
현재 계층 경로 : /servlet-mvc/members/
결과 : /servlet-mvc/members/save

실행

http://localhost:8080/servlet-mvc/members/new-form

![img_47](https://user-images.githubusercontent.com/79847020/159016250-54f54f19-c9d8-46f2-88a4-66b27bc9ff7e.png)
경로
* 절대 경로 : "/save" -> http://localhost:8080/save
* 상대 경로 : "save" -> http://localhost:8080/servlet-mvc/members/save

이후 코드에서 해당 JSP를 상대경로를 활용해서 여러 URL에서 활용할 수 있다.

회원 저장 - 컨트롤러
```java
@WebServlet(name = "mvcMemberSaveServlet", urlPatterns = "/servlet-mvc/members/save")
public class MvcMemberSaveServlet extends HttpServlet {

    private MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("MvcMemberSaveServlet.service");

        String username = request.getParameter("username");
        int age = Integer.parseInt(request.getParameter("age"));

        Member member = new Member(username, age);
        memberRepository.save(member);

        //Model에 데이터를 보관
        request.setAttribute("member", member);

        String VIEW_PATH = "/WEB-INF/views/save-result.jsp";

        RequestDispatcher dispatcher = request.getRequestDispatcher(VIEW_PATH);
        dispatcher.forward(request, response);
    }
}
```

HttpServletRequest를 Model로 사용한다. request가 제공하는 setAttribute()를 사용하면 request 객체에 데이터를 보관해서 뷰에 전달할 수 있다. getAttribute()를 사용해서 데이터를 꺼내면 된다.

회원 저장 뷰
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
성공
<ul>
    <li>id=${member.id}</li>
    <li>username=${member.username}</li>
    <li>age=${member.age}</li>
</ul>

<a href="/index.html">메인</a>
</body>
</html>
```

<%= request.getAttribute("member")%>로 모델에 저장한 member 객체를 꺼낼 수 있지만 너무 복잡해진다. JSP는 ${} 문법을 제공하는데 이 문법을 사용하면 request의 attribute에 담긴 데이터를 편리하게 조회할 수 있다.

http://localhost:8080/servlet-mvc/members/new-form

![img_48](https://user-images.githubusercontent.com/79847020/159016271-bfac49d5-8883-4e72-9eb6-e6b6ee55f6c0.png)

![img_49](https://user-images.githubusercontent.com/79847020/159016277-cd621858-a3cd-4718-834b-827cd059a01d.png)

회원 목록 조회 - 컨트롤러
MvcMemberListServlet
```java
@WebServlet(name = "mvcMemberListServlet", urlPatterns = "/servlet-mvc/members")
public class MvcMemberListServlet extends HttpServlet {

    MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("MvcMemberListServlet.service");

        List<Member> members = memberRepository.findAll();

        request.setAttribute("members", members);

        String VIEW_PATH = "/WEB-INF/views/members.jsp";
        RequestDispatcher dispatcher = request.getRequestDispatcher(VIEW_PATH);
        dispatcher.forward(request, response);
    }
}
```

request 객체를 사용해서 List<Member> members를 모델에 보관했다.

회원 목록 조회 - 뷰
members.jsp
```HTML
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

<html>
<head>
    <meta charset="UTF-8">
    <title>Title</title></head>
<body><a href="/index.html">메인</a>
<table>
    <thead>
    <th>id</th>
    <th>username</th>
    <th>age</th>
    </thead>
    <tbody>
    <c:forEach var="item" items="${members}">
        <tr>
            <td>${item.id}</td>
            <td>${item.username}</td>
            <td>${item.age}</td>
        </tr>
    </c:forEach>
    </tbody>
</table>
</body>
</html>
```

JSLT
JSP가 제공하는 taglib기능을 사용하서 반복 출력했다. forEach를 사용하기 위해서 다음 태그를 추가한다.
```html
<%@ taglib prefix="c" url="http://java.sun.com/jsp/jstl/core"%>
```
![img_50](https://user-images.githubusercontent.com/79847020/159016324-6ca153da-dc37-4741-9e63-a12b3289c7ec.png)

## 3.6 MVC패턴 - 한계

MVC 패턴을 적용한 덕분에 컨트롤러의 역할과 뷰를 렌더링 하는 역할을 명확하게 구분할 수 있다. 특히 뷰는 화면을 그리는 역할에 충실한 덕분에 깔끔하고 직관적이다. 단순하게 모델에서 필요한 데이터를 꺼내 화면을 렌더링하면 된다. 그런데 각 컨트롤러는 중복된 코드들이 많이 보인다.

MVC 컨트롤러의 단점
1. 포워드 중복
```java
RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
dispatcher.forward(request, response);
```

2. ViewPath 중복
```java
String viewPath = "/WEB-INF/views/new-form.jsp";
```
  prefix : /WEB-INF/views/
  suffix : .jsp

그리고 만약 jsp가 아닌 thymeleaf 같은 뷰로 변경한다면 전체 코드를 다 변경해야 한다.

3. 사용하지 않는 코드

다음 코드를 사용할 때도 있고, 사용하지 않을 때도 있다. 특히 resposne는 현재 코드에서 사용하지 않는다.
```java
HttpServletRequest request, HttpServletResponse response
```

그리고 HttpServletREquest, HttpServletResponse를 사용하는 코드는 테스트케이스를 작성하기도 어렵다.

4. 공통 처리가 어렵다.

각 컨트롤러에서 공통으로 처리해야 하는 부분이 점점 많이 더 증가할 것이다. 단순히 공통 기능을 메서드로 뽑으면 될 것 같지만 결과적으로 해당 메서드를 항상 호출해야 하고 실수로 호출하지 않으면 문제가 될 것이다. 그리고 호출하는 것자체도 중복이다.

5. 정리하면 공통 처리가 어렵다는 문제가 있다.

이 문제를 해결하려면 컨트롤러 호출 전에 먼저 공통 기능을 처리해야 한다. 소위 수문장 역할을 하는 기능이 필요하다. 프론트 컨트롤러(FrontController)패턴을 도입하면 이런 문제를 깔끔하게 해결할 수 있다.

# 4. MVC 프레임워크 만들기

## 4.1 프론트 컨트롤러 패턴 소개

프론트 컨트롤러 도입 전

![img_51.png](img_51.png)

프론트 컨트롤러 도입 후

![img_52.png](img_52.png)

FronController 패턴 특징
1. 프론트 컨트롤러 서블릿 하나로 클라이언트의 요청을 받음
2. 프론트 컨트롤러가 요청에 맞는 컨트롤러를 찾아서 호출
공통 처리 기능
3. 프론트 컨트롤러를 제외한 나머지는 컨트롤러는 서블릿을 사용하지 않아도됨.

스프링 웹 MVC와 프론트 컨트롤러
1. 스프링 웹 MVC의 핵심도 바로 FrontController
2. 스프링 웹 MVC의 DispatcherServlet이 FrontControll 패턴으로 구현되어 있음

## 4.2 프론트 컨트롤러 도입 - v1

프론트 컨트롤러를 단계적으로 도입해보자. 기존 코드를 최대한 유지하면서 프론트 컨트롤러를 도입하는 것이다. 

![img_53.png](img_53.png)

ControllerV1
ControllerV1.java
```java
public interface ControllerV1 {

    void process(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException;
}
```

서블릿과 비슷한 모양의 컨트롤러 인터페이스를 도입한다. 각 컨트롤러들은 이 인터페이스를 구현하면 된다. 프론트 컨트롤러는 이 인터페이스를 호출해서 구현과 관계없이 로직의 일관성을 가져갈 수 있다.

MemberFormControllerV1.java
```java
public class MemberFormControllerV1 implements ControllerV1 {
    @Override
    public void process(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("MemberFormControllerV1.process");
        String VIEW_PATH = "/WEB-INF/views/new-form.jsp";
        RequestDispatcher dispatcher = request.getRequestDispatcher(VIEW_PATH);
        dispatcher.forward(request, response);
    }
}
```
MemberListControllerV1.java
```java
public class MemberListControllerV1 implements ControllerV1 {

    private MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    public void process(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("MemberListControllerV1.process");

        List<Member> members = memberRepository.findAll();

        request.setAttribute("members", members);

        String VIEW_PATH = "/WEB-INF/views/members.jsp";
        RequestDispatcher dispatcher = request.getRequestDispatcher(VIEW_PATH);
        dispatcher.forward(request, response);
    }
}
```
MemberSaverControllerV1.java
```java
public class MemberSaverControllerV1 implements ControllerV1 {

    private MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    public void process(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("MemberSaverControllerV1.process");

        String username = request.getParameter("username");
        int age = Integer.parseInt(request.getParameter("age"));

        Member member = new Member(username, age);
        memberRepository.save(member);

        //Model에 데이터를 보관
        request.setAttribute("member", member);

        String VIEW_PATH = "/WEB-INF/views/save-result.jsp";

        RequestDispatcher dispatcher = request.getRequestDispatcher(VIEW_PATH);
        dispatcher.forward(request, response);
    }
}
```

각 컨트롤러의 내부로직은 서블릿과 거의 같다.

FrontControllerServletV1 - 프론트 컨트롤러
```java
@WebServlet(name = "freshControllerServletV1", urlPatterns = "/front-controller/v1/*")
public class FrontControllerServletV1 extends HttpServlet {

    private Map<String, ControllerV1> controllerMap = new HashMap<>();

    public FrontControllerServletV1() {
        controllerMap.put("/front-controller/v1/members/new-form", new MemberFormControllerV1());
        controllerMap.put("/front-controller/v1/members/save", new MemberSaverControllerV1());
        controllerMap.put("/front-controller/v1/members", new MemberListControllerV1());
    }

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("FrontControllerServletV1.service");

        String requestURI = request.getRequestURI();

        ControllerV1 controller = controllerMap.get(requestURI);

        if (controller == null) {
            response.setStatus(HttpServletResponse.SC_NOT_FOUND);
            return;
        }

        controller.process(request, response);
    }
}
```

프론트 컨트롤러 분석

urlPatterns 
* urlPatterns - "/front-cotroller/v1/*" : /front-controller/v1를 포함한 하위 모든 요청은 이 서블릿에서 받아들인다.

controllerMap
* key : 매핑 URL
* value : 호출될 컨트롤러

service()
* 먼저, requestURI를 조회해서 실제 호출할 컨트롤러를 controllerMap에서 찾는다. 만약 없다면 404(SC_NOT_FOUND) 상태 코드를 반환한다. 컨트롤러를 찾고 controller.process(request, response)을 호출해서 해당 컨트롤러를 실행한다.

JSP
* JSP는 이전 MVC에서 사용했던 것을 그대로 사용한다.

실행
1. 등록 : http://localhost:8080/front-controller/v1/members/new-form
2. 목록 : http://localhost:8080/front-controller/v1/members

## 4.3 View 분리 - v2

모든 컨트롤러에서 뷰로 이동하는 부분에 중복이 있고 깔끔하지 않다.
```java
String viewPath = "/WEB-INF/views/new-form.jsp";
RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
dispatcher.forward(request, response);
```

![img_54.png](img_54.png)

MyView.java
```java
public class MyView {
    private String viewPath;

    public MyView(String viewPath) {
        this.viewPath = viewPath;
    }

    public void render(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
        RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
        dispatcher.forward(request, response);
    }
}
```
s
MyView를 활용하는 컨트롤러 인터페이스를 만들어보자. 컨트롤러가 뷰를 반환하는 특징이 있다.

ControllerV2
```java
public interface ControllerV2 {
    MyView process(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException;
}
```

MemberFormControllerV2 - 회원 등록 폼
```java
public class MemberFormControllerV2 implements ControllerV2 {
    @Override
    public MyView process(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        return new MyView("/WEB-INF/views/new-form.jsp");
    }
}
```

각 컨트롤러는 복잡한 dispatcher.forward()를 직접 생성해서 호출하지 않아도 된다. MyView 객체를 생성하고 거기에 뷰 이름만 넣고 반환하면 된다.

ControllerV1을 구현한 클래스와 ControllerV2를 구현한 클래스를 비교해보면 이 부분의 중복이 확실하게 제거된 것을 확인할 수 있다.

MemberSaveControllerV2 - 회원 저장
```java
public class MemberSaveControllerV2 implements ControllerV2 {

    private MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    public MyView process(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String username = request.getParameter("username");
        int age = Integer.parseInt(request.getParameter("age"));

        Member member = new Member(username, age);
        memberRepository.save(member);

        //Model에 데이터를 보관
        request.setAttribute("member", member);

        return new MyView("/WEB-INF/views/save-result.jsp");
    }
}
```

MemberListControllerV2 - 회원 목록
```java
public class MemberListControllerV2 implements ControllerV2 {

    private MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    public MyView process(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        List<Member> members = memberRepository.findAll();

        //Model에 데이터를 보관한다.
        request.setAttribute("members", members);

        return new MyView("/WEB-INF/views/members.jsp");
    }
}
```

프론트 컨트롤러 V2
```java
@WebServlet(name = "frontControllerServletV2", urlPatterns = "/front-controller/v2/*")
public class FrontControllerServletV2 extends HttpServlet {
    Map<String, ControllerV2> controllerMap = new HashMap<>();

    public FrontControllerServletV2() {
        controllerMap.put("/front-controller/v2/members/new-form", new MemberFormControllerV2());
        controllerMap.put("/front-controller/v2/members/save", new MemberSaveControllerV2());
        controllerMap.put("/front-controller/v2/members", new MemberListControllerV2());
    }

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("FrontControllerServletV2.service");

        String requestURI = request.getRequestURI();

        ControllerV2 controller = controllerMap.get(requestURI);

        if (controller == null) {
            response.setStatus(HttpServletResponse.SC_NOT_FOUND);
            return;
        }

        MyView view = controller.process(request, response);
        view.render(request, response);
    }
}
```

ControllerV2의 반환 타입이 MyView 이므로 프론트 컨트롤러는 컨트롤러의 호출 결과로 MyView를 반환 받는다. 그리고 view.render()를 호출하면 forward 로직을 수행해서 JSP가 실행된다.

MyView.render()
```java
    public void render(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
        RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
        dispatcher.forward(request, response);
    }
```

프론트 컨트롤러의 도입으로 MyView 객체의 render()를 호출하는 부분을 모두 일관되게 처리할 수 있따. 각각의 컨트롤러는 MyView 객체를 생성만 해서 반환하면 된다.

## 4.4 Model 추가 - v3

서블릿 종속성 제거
* 컨트롤러 입장에서 HttpServletRequest, HttpServletResponse이 꼭 필요할까 ? 요청 파라미터 정보는 자바의 Map에 담아서 넘기면 지금 구조에서는 컨트롤러가 서블릿 기술을 몰라도 동작할 수 있다.
* 또는 request 객체를 Model로 사용하는 대신 별도의 Model 객체를 만들어서 반환하면 된다. 우리가 구현하는 컨트롤러가 서블릿 기술을 전혀 사용하지 않도록 변경해보자. 이렇게 하면 구현 코드가 매우 단순해지고 테스트 코드 작성이 쉽다.

뷰 이름 중복 제거
* 컨트롤러에서 지정하는 뷰 이름에 중복이 있는 것을 확인할 수 있다. 컨트롤러는 뷰의 논리 이름을 반환하고 실제 물리 위치의 이름은 프론트 컨트롤러에서 처리하도록 단순화 하자. 이렇게 해두면 향후 뷰의 폴더 위치가 함께 이동해도 프론트 컨트롤러만 고치면 된다.
* /WEB-INF/views/new-form.jsp -> new-from
* /WEB-INF/views/save-result.jsp -> save-result
* /WEB-INF/views//members.jsp -> members
* 변경의 지점을 축소할 수 있는 변경은 좋은 변경이다.

![img_55.png](img_55.png)

ModelAndView
* 지금까지 컨트롤러에서 서블릿에 종속적인 HttpServletReuest를 사용했다. Model도 request.setAttribute()를 통해 데이터를 저장하고 뷰에 전달했다. 
* 서블릿의 종속성을 제거하기 위해 Model을 직접 만들고 추가로 View 이름까지 전달하는 객체를 만들어보자.
* 이번 버전은 컨트롤러에서 HttpServletRequest를 사용할 수 없으므로 request.setAttribute()를 호출할 수도 없다. 따라서 Model이 별도로 필요하다.

```java
@Getter
@Setter
public class ModelView {
    private String viewName;
    private Map<String, Object> model = new HashMap<>();

    public ModelView(String viewName) {
        this.viewName = viewName;
    }
}
```

뷰의 이름과 뷰를 렌더링할 때 필요한 model 객체를 가지고 온다. model은 단순히 map으로 되어 있으므로 컨트롤러에서 뷰에 필요한 데이터를 key, value로 넣어주면 된다.

ControllerV3
```java
public interface ControllerV3 {
    ModelView process(Map<String, String> paramMap);
}
```

이 컨트롤러는 서블릿 기술을 전혀 사용하지 않는다. 따라서 구현이 매우 단순해지고, 테스트 코드 작성시 테스트 하기 쉽다. 
HttpServletRequest가 제공하는 파라미터는 프론트 컨트롤러가 paramMap에 담아서 호출해주면 된다. 응답 결과로 뷰 이름과 뷰에 전달할 Model 데이터를 포함하는 ModelView 객체를 반환하면 된다.

MemberFormControllerV3 - 회원 등록 폼
```java
public class MemberFormControllerV3 implements ControllerV3 {
    @Override
    public ModelView process(Map<String, String> paramMap) {
        return new ModelView("new-form");
    }
}
```

MemberSaveControllerV3 - 회원 저장
```java
public class MemberSaveControllerV3 implements ControllerV3 {

    private MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    public ModelView process(Map<String, String> paramMap) {
        String username = paramMap.get("username");
        int age = Integer.parseInt(paramMap.get("age"));

        Member member = new Member(username, age);
        memberRepository.save(member);

        ModelView mv = new ModelView("save-result");
        mv.getModel().put("member", member);

        return mv;
    }
}
```

paramMap.get("username");
* 파라미터 정보는 map에 담겨있다. map에서 필요한 요청 파라미터를 조회하면 된다.

mv.getModel().put("member", member);
* 모델은 단순한 map이므로 모델에 뷰에서 필요한 member 객체를 담고 반환한다.

MemberListControllerV3 - 회원 목록
```java
public class MemberListControllerV3 implements ControllerV3 {

    private MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    public ModelView process(Map<String, String> paramMap) {
        List<Member> members = memberRepository.findAll();

        ModelView mv = new ModelView("members");
        mv.getModel().put("members", members);

        return mv;
    }
}
```

FrontControolverServletV3
```java
@WebServlet(name = "frontControllerServletV3", urlPatterns = "/front-controller/v3/*")
public class FrontControllerServletV3 extends HelloServlet {
    Map<String, ControllerV3> controllerMap = new HashMap<>();

    public FrontControllerServletV3() {
        controllerMap.put("/front-controller/v3/members/new-form", new MemberFormControllerV3());
        controllerMap.put("/front-controller/v3/members/save", new MemberSaveControllerV3());
        controllerMap.put("/front-controller/v3/members", new MemberListControllerV3());
    }

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("FrontControllerServletV3.service");

        String requestURI = request.getRequestURI();

        ControllerV3 controller = controllerMap.get(requestURI);

        if (controller == null) {
            response.setStatus(HttpServletResponse.SC_NOT_FOUND);
            return;
        }

        //paramMap
        Map<String, String> paramMap = createParam(request);

        ModelView mv = controller.process(paramMap);

        String viewName = mv.getViewName();

        MyView view = viewResolver(viewName);
        view.render(mv.getModel(), request, response);
    }

    private MyView viewResolver(String viewName) {
        MyView view = new MyView("/WEB-INF/views/" + viewName + ".jsp");
        return view;
    }

    private Map<String, String> createParam(HttpServletRequest request) {
        Map<String, String> paramMap = new HashMap<>();

        request.getParameterNames().asIterator()
                .forEachRemaining(paramName -> paramMap.put(paramName, request.getParameter(paramName)));
        return paramMap;
    }
}
```

createParamMap()
* HttpServletRequest에서 파라미터 정보를 꺼내서 Map으로 반환한다. 그리고 해당 Map(paramMap)을 컨트롤러에 전달하면 호출한다.

뷰 리졸버
1. MyView view = viewResolerver(viewName);
   * 컨트롤러가 반환한 논리 뷰 이름을 실제 물리 뷰 경로로 변경한다. 그리고 실제 물리 경로가 있는 MyView 객체를 반환한다.
2. 논리 뷰 이름 : members
3. 물리 뷰 경로 : /WEB-INF/views/members.jsp

view.render(mv.getMode(), request, response)
1. 뷰 객체를 통해서 HTML 화면을 렌더링 한다.
2. 뷰 객체의 render()는 모델 정보도 함께 받는다.
3. JSP는 request.getAttribute()로 데이터를 조회하기 때문에 모델의 데이터를 꺼내서 request.setAttribute()로 담아둔다.
4. JSP로 포워드 해서 JSP를 렌더링 한다.

```java
public class MyView {
    private String viewPath;

    public MyView(String viewPath) {
        this.viewPath = viewPath;
    }

    public void render(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
        RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
        dispatcher.forward(request, response);
    }

    public void render(Map<String, Object> model, HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        modelToRequest(model, request);

        RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
        dispatcher.forward(request, response);
    }

    private void modelToRequest(Map<String, Object> model, HttpServletRequest request) {
        model.forEach((key, value) -> request.setAttribute(key, value));
    }
}
```

## 4.5 단순하고 실용적인 컨트롤러 - v4

앞서 만든 v3 컨트롤러는 서블릿 종속성을 제거하고 뷰 경로의 중복을 제거하는 등 잘 설계된 컨트롤러이다. 그런데 실제 컨트롤러 인터페이스를 구현하는 개발자 입장에서 보면 항상 ModelView 객체를 생성하고 반환해야 하는 부분이 조금은 번거롭다.

좋은 프레임워크는 아키텍처도 중요하지만 그와 더불어 실제 개발하는 개발자가 단순하고 편리하게 사용할 수 있어야 한다. 소위 실용성이 있어야 한다.

이번에는 v3를 조금 변경해서 실제 구현하는 개발자들이 매우 편리하게 개발할 수 있는 v4 버전을 개발해보자.

![img_56.png](img_56.png)

ControllerV4
```java
public interface ControllerV4 {
    String process(Map<String, String> paramMap, Map<String, Object> model);
}
```

이번 버전은 인터페이스에 ModelView가 없다. model 객체는 파라미터를 전달하기 때문에 그냥 사용하면 되고 결과로 뷰의 이름만 반환해주면 된다.

MemberFormControllerV4
```java
public class MemberFormControllerV4 implements ControllerV4 {
    @Override
    public String process(Map<String, String> paramMap, Map<String, Object> model) {
        return "new-form";
    }
}
```
MemberListControllerV4
```java
public class MemberListControllerV4 implements ControllerV4 {
    private MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    public String process(Map<String, String> paramMap, Map<String, Object> model) {
        List<Member> members = memberRepository.findAll();

        model.put("members", members);

        return "members";
    }
}
```
MemberSaveControllerV4
```java
public class MemberSaveControllerV4 implements ControllerV4 {
    private MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    public String process(Map<String, String> paramMap, Map<String, Object> model) {
        String username = paramMap.get("username");
        int age = Integer.parseInt(paramMap.get("age"));

        Member member = new Member(username, age);
        memberRepository.save(member);

        model.put("member", member);

        return "save-result";
    }
}
```
FrontControllerServletV4
```java
@WebServlet(name = "frontControllerServletV4", urlPatterns = "/front-controller/v4/*")
public class FrontControllerServletV4 extends HelloServlet {
    Map<String, ControllerV4> controllerMap = new HashMap<>();

    public FrontControllerServletV4() {
        controllerMap.put("/front-controller/v4/members/new-form", new MemberFormControllerV4());
        controllerMap.put("/front-controller/v4/members/save", new MemberSaveControllerV4());
        controllerMap.put("/front-controller/v4/members", new MemberListControllerV4());
    }

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("FrontControllerServletV4.service");

        String requestURI = request.getRequestURI();

        ControllerV4 controller = controllerMap.get(requestURI);

        if (controller == null) {
            response.setStatus(HttpServletResponse.SC_NOT_FOUND);
            return;
        }

        //paramMap
        Map<String, String> paramMap = createParam(request);
        Map<String, Object> model = new HashMap<>();

        //Controller
        String viewName = controller.process(paramMap, model);

        //view
        MyView view = viewResolver(viewName);
        view.render(model, request, response);
    }

    private MyView viewResolver(String viewName) {
        MyView view = new MyView("/WEB-INF/views/" + viewName + ".jsp");
        return view;
    }

    private Map<String, String> createParam(HttpServletRequest request) {
        Map<String, String> paramMap = new HashMap<>();

        request.getParameterNames().asIterator()
                .forEachRemaining(paramName -> paramMap.put(paramName, request.getParameter(paramName)));
        return paramMap;
    }
}
```

모델 객체 전달
* Map<String, Object> model = new HashMap<>(); 모델 객체를 프론트 컨트롤러에서 생성해서 넘겨준다. 컨트롤러에서 모델 객체에 값을 담으면 여기에 그대로 담겨있게 된다.

뷰의 논리 이름을 직접 반환
```java
String viewName = controller.process(paramMap, model);
MyView view = viewResolver(viewName);
```
컨트롤러가 직접 뷰의 논리 이름을 변환하므로 이 값을 사용해서 실제 물리 뷰를 찾을 수 있다.

정리
이번 버전의 컨트롤러는 매우 단순하고 실용적이다. 기본 구조에서 모델을 파라미터로 넘기고 뷰의 논리 이름을 반환한다는 적은 아이디어를 적용했을 뿐인데 컨트롤러를 구현하는 개발자 입장에서 보면 이제 군더더기 없는 코드를 작성할 수 있다.
또한 중요한 사실은 여기까지 한번에 온 것이 아니라는 점이다. 프레임워크가 점진적으로 발견하는 과정측에서 이런 방법도 찾을 수 있었다.

## 4.6 유연한 컨트롤러 1 - v5

만약 어떤 개발자는 ControllerV3 방식으로 개발하고 싶고 어떤 개발자는 ControllverV4 방식으로 개발하고 싶다면 어떻게 해야할까 ?

ControllerV3
```java
public interface ControllerV3 {
    ModelView process(Map<String, String> paramMap);
}
```

ControllerV4
```java
public interface ControllerV4 {
    String process(Map<String, String> paramMap, Map<String, Object> model);
}
```

어댑터 패턴
* 지금까지 우리가 개발한 프론트 컨트롤러는 한가지 방식의 컨트롤러 인터페이스만 사용할 수 있다. ControllerV3, ControllerV4는 완전히 다른 인터페이스이다. 따라서 호환이 불가능하다. 마치 v3는 110v이고 v4는 220v 전기 콘센트 같은 것이다. 이럴 때 사용하는 것이 어댑터이다.

![img_57.png](img_57.png)

핸들러 어댑터
* 중간에 어댑터 역할을 하는 어댑터가 추가되었는데 핸들러 어댑터이다. 어댑터 역할을 해주는 덕분에 다양한 종류의 컨트롤러를 호출할 수 있다.

핸들러
* 컨트롤러의 이름을 더 넓은 범위인 핸들러로 변경했다. 그 이유는 이제 어댑터가 있기 때문에 꼭 컨트롤러의 개념 뿐만 아니라 어떠한 것이든 해당하는 종류의 어댑터만 있으면 다 처리할 수 있기 때문이다.
```java
public interface MyHandlerAdapter {
    boolean supports(Object handler);

    ModelView handle(HttpServletRequest request, HttpServletResponse response, Object handler);
}
```

MyHandlerAdapterV3
```java
public class ControllerV3HandlerAdapter implements MyHandlerAdapter {
    @Override
    public boolean supports(Object handler) {
        return (handler instanceof ControllerV3);
    }

    @Override
    public ModelView handle(HttpServletRequest request, HttpServletResponse response, Object handler) {
        ControllerV3 controller = (ControllerV3) handler;

        Map<String, String> paramMap = createParam(request);
        ModelView mv = controller.process(paramMap);

        return mv;
    }

    private Map<String, String> createParam(HttpServletRequest request) {
        Map<String, String> paramMap = new HashMap<>();

        request.getParameterNames().asIterator()
                .forEachRemaining(paramName -> paramMap.put(paramName, request.getParameter(paramName)));
        return paramMap;
    }
}
```

MyHandlerAdapterV3
```java
public class ControllerV4HandlerAdapter implements MyHandlerAdapter {
    @Override
    public boolean supports(Object handler) {
        return (handler instanceof hello.servlet.web.frontcontroller.v4.ControllerV4);
    }

    @Override
    public ModelView handle(HttpServletRequest request, HttpServletResponse response, Object handler) {
        ControllerV4 controller = (ControllerV4) handler;

        Map<String, String> paramMap = createParam(request);
        Map<String, Object> model = new HashMap<>();

        String viewName = controller.process(paramMap, model);

        ModelView mv = new ModelView(viewName);
        mv.setModel(model);

        return mv;
    }

    private Map<String, String> createParam(HttpServletRequest request) {
        Map<String, String> paramMap = new HashMap<>();

        request.getParameterNames().asIterator()
                .forEachRemaining(paramName -> paramMap.put(paramName, request.getParameter(paramName)));
        return paramMap;
    }
}
```
```java
boolean supports(Object handler)
```
1. handler는 컨트롤러를 말한다.
2. 어댑터가 해당 컨트롤러를 처리할 수 있는지 판단하는 메서드다.

```java
ModelView handle(HttpServletRequest request, HttpServletResponse response, Object handler) 
```
1. 어댑터는 실제 컨트롤러를 호출하고 그 결과로 ModelView를 반환해야 한다.
2. 실제 컨트롤러는 ModelView를 반환하지 못하면 어댑터가 ModelView를 직접 생성해서라도 반환해야 한다.
3. 이전에는 프론트 컨트롤러가 실제 컨트롤러를 호출했지만 이제는 이 어댑터를 통해서 실제 컨트롤러가 호출된다.

```java
ControllerV3 controller = (ControllerV3) handler;

Map<String, String> paramMap = createParam(request);
ModelView mv = controller.process(paramMap);

return mv;
```
handle를 컨트롤러 v3로 변환한 다음에 v3형식에 맞도록 호출한다. supports()를 통해 ControllverV3만 지원하기 때문에 타입 변환은 걱정없이 실행해도 된다. ControllverV3는 ModelView를 반환하므로 그대로 ModelView를 반환하면 된다.

FrontControllServletV5
```java
@WebServlet(name = "fronControllerServletV5", urlPatterns = "/front-controller/v5/*")
public class FrontControllerServletV5 extends HttpServlet {

    private final Map<String, Object> handlerMappingMap = new HashMap<>();
    private final List<MyHandlerAdapter> handlerAdapters = new ArrayList<>();

    public FrontControllerServletV5() {
        initHandlerMappingMap();
        initHandlerAdapters();
    }

    private void initHandlerAdapters() {
        handlerAdapters.add(new ControllerV3HandlerAdapter());
    }

    private void initHandlerMappingMap() {
        handlerMappingMap.put("/front-controller/v5/v3/members/new-form", new MemberFormControllerV3());
        handlerMappingMap.put("/front-controller/v5/v3/members/save", new MemberSaveControllerV3());
        handlerMappingMap.put("/front-controller/v5/v3/members", new MemberListControllerV3());
    }

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("FrontControllerServletV5.service");

        // 핸들러 - 요청을 실제 처리하는 컨트롤러 기능
        Object handler = getHandler(request);

        if (handler == null) {
            response.setStatus(HttpServletResponse.SC_NOT_FOUND);
            return;
        }

        // 핸들러어댑터 - 핸들러에 실행
        MyHandlerAdapter adapter = getHandlerAdapter(handler);

        ModelView mv = adapter.handle(request, response, handler);

        // 뷰리졸버 - 뷰를 찾아 랜더링한다.
        String viewName = mv.getViewName();
        MyView view = viewResolver(viewName);

        view.render(mv.getModel(), request, response);
    }

    private Object getHandler(HttpServletRequest request) {
        String requestURI = request.getRequestURI();
        return handlerMappingMap.get(requestURI);
    }

    private MyHandlerAdapter getHandlerAdapter(Object handler) {
        for (MyHandlerAdapter adapter : handlerAdapters) {
            if (adapter.supports(handler)) return adapter;
        }
        throw new IllegalArgumentException("handler adapter를 찾을 수 없습니다. " + handler);
    }

    private MyView viewResolver(String viewName) {
        MyView view = new MyView("/WEB-INF/views/" + viewName + ".jsp");
        return view;
    }
}
```

컨트롤러(Controller) -> 핸들러(Handler)
* 이전에는 컨트롤러를 직접 매핑해서 사용했다. 그런데 이제는 어댑터를 사용하기 때문에 컨트롤러 뿐만 아니라 어댑터가 지원하기만 하면 어떤 것이라도 URL에 매핑해서 사용할 수 있다. 그래서 이름을 컨트롤러에서 다 넓은 범위의 핸들러로 변경했따.

생성자
```java
public FrontControllerServletV5() {
    initHandlerMappingMap(); //핸들러 매핑 초기화
    initHandlerAdapters(); //어댑터 초기화
}
```
생성자는 핸들러 매핑과 어댑터를 등록한다.

매핑 정보
* private final Map<String, Object> handlerMappingMap = new HashMap<>();
* 매핑 정보의 값이 ControllerV3, ControllerV4 같은 인터페이스에서 아무 값이나 받을 수 있는 Object로 변경되었다.

핸들러 매핑
* Object handler = getHandler(request);
```java
    String requestURI = request.getRequestURI();
    eturn handlerMappingMap.get(requestURI);
```
* 핸들러 매핑 정보인 handlerMappingMap에서 URL에 매핑된 핸들러 객체를 찾아서 반환한다.

핸들러를 처리할 수 있는 어댑터 조회
```java
for (MyHandlerAdapter adapter : handleAdapters) {
    if (adapter.supports(handler)) {
        return adapter;
    }    
}
```
handle를 처리할 수 있는 어댑터를 adapter.supports(handler)를 통해서 찾는다. handler가 ControllerV3 인터페이스를 구현했따면 ControllerV3HandlerAdapter 객체가 반환된다.

어댑터 호출
* ModelView mv = adapter.handler(requestm response, handler);
* 어댑터의 handle(request, response, handler) 메서드를 통해 실제 어댑터가 호출된다.
* 어댑터는 handler를 호출하고 그 결과를 어댑터에 맞추어 반환한다.
* ControllerV3HandlerAdapter의 경우 어댑터의 모양과 컨트롤러의 모양이 유사해서 변환 로직이 단순한다.

정리
지금은 V3컨트로러를 사용할 수 있는 어댑터와 ControllerV3만 들어 있어서 크게 감흥이 없다. ControllerV4를 사용할 수 있도록 기능을 추가해보자.

## 4.7 유연한 컨트롤러 2 - v5

FrontControllerServletV5에 ControllerV4 기능도 추가해보자.
```java
@WebServlet(name = "fronControllerServletV5", urlPatterns = "/front-controller/v5/*")
public class FrontControllerServletV5 extends HttpServlet {
    ...
    private void initHandlerAdapters() {
        handlerAdapters.add(new ControllerV3HandlerAdapter());
        handlerAdapters.add(new ControllerV4HandlerAdapter());
    }
    
    private void initHandlerMappingMap() {
        handlerMappingMap.put("/front-controller/v5/v3/members/new-form", new MemberFormControllerV3());
        handlerMappingMap.put("/front-controller/v5/v3/members/save", new MemberSaveControllerV3());
        handlerMappingMap.put("/front-controller/v5/v3/members", new MemberListControllerV3());

        handlerMappingMap.put("/front-controller/v5/v4/members/new-form", new MemberFormControllerV4());
        handlerMappingMap.put("/front-controller/v5/v4/members/save", new MemberSaveControllerV4());
        handlerMappingMap.put("/front-controller/v5/v4/members", new MemberListControllerV4());
    }
```
핸들러 매핑(handleMappingMap)에 ControllerV4를 사용하는 컨트롤러를 추가하고 해당 컨트롤러를 처리할 수 있는 어댑터인 ControllerV4HandlerAdapter도 추가하자.

ControllerV4HandlerAdapter
```java
public class ControllerV4HandlerAdapter implements MyHandlerAdapter {
    @Override
    public boolean supports(Object handler) {
        return (handler instanceof hello.servlet.web.frontcontroller.v4.ControllerV4);
    }

    @Override
    public ModelView handle(HttpServletRequest request, HttpServletResponse response, Object handler) {
        ControllerV4 controller = (ControllerV4) handler;

        Map<String, String> paramMap = createParam(request);
        Map<String, Object> model = new HashMap<>();

        String viewName = controller.process(paramMap, model);

        ModelView mv = new ModelView(viewName);
        mv.setModel(model);

        return mv;
    }

    private Map<String, String> createParam(HttpServletRequest request) {
        Map<String, String> paramMap = new HashMap<>();

        request.getParameterNames().asIterator()
                .forEachRemaining(paramName -> paramMap.put(paramName, request.getParameter(paramName)));
        return paramMap;
    }
}
```
supports
```java
@Override
    public boolean supports(Object handler) {
        return (handler instanceof hello.servlet.web.frontcontroller.v4.ControllerV4);
    }
```
handler가 ControllerV4인 경우에만 처리하는 어댑터이다.

실행로직
```java
ControllerV4 controller = (ControllerV4) handler;
Map<String, String> paramMap = createParam(request);
Map<String, Object> model = new HashMap<>();

String viewName = controller.process(paramMap, model);
```
handler를 ControllerV4로 캐스팅 하고 paramMap, model을 만들어서 해당 컨트롤러를 호출한다. 그리고 viewName을 반환 받는다.

어댑터 변ㅌ환
```java

```
어댑터에서 이 부분이 단순하지만 중요한 부분이다. 어댑터가 호출하는 ControllerV4는 뷰의 이름을 반환한다. 여기서 어댑터가 꼭 필요한 이유가 나온다. ControllerV4는 뷰의 이름을 반환했지만 어댑터는 이것을 ModelView로 만들어서 형식을 맞추어 반환한다. 마치 110v 전기 콘센트를 220v 전기 콘센트로 변경하듯이.

어댑터와 ControllerV4
```java
public interface ControllerV4 {
    String process(Map<String, String> paramMap, Map<String, Object> model);
}

public interface MyHandlerAdapter {
    boolean supports(Object handler);

    ModelView handle(HttpServletRequest request, HttpServletResponse response, Object handler);
}
```
## 4.8 정리

정리
1. 기존
   * 기존에는 URL 마다 서블릿 클래스를 생성한다.
   * 서블릿 클래스 -> HttpServletRequest, HttpServletResponse를 받아서 RequestDispatcher를 사용해서 뷰를 렌더링 했다.

2. V1 프론트 컨트롤러를 도입
   * ControllerMap을 사용해서 URL과 Controller를 매핑한다.(핸들러 매핑 정보)
   * Controller -> HttpServletRequest, HttpServletResponse를 받아서 RequestDispatcher를 사용해서 뷰를 렌더링 했다.

3. V2 View 로직 분리
   * 단순 반복되는 뷰(MyView) 로직 분리
   * Controller ->  HttpServletRequest, HttpServletResponse를 받아서 MyView를 반환 후 MyView.render()를 통해 렌더링한다.

4. V3 Model 추가
   * 서블릿 종속성 제거, ModelView 도입
   * ModelView는 String viewname과 Map<String, Object> model를 멤버로 가진다.
   * Controller -> Map<String, String> paramMap를 통해 파라미터를 받아서 ModelView를 반환한다. ModelView의 View정보를 이용하여 MyView를 생성해서(ViewResolver) render()한다.

5. V4 단순하고 실용적인 컨트롤러
   * V3과 거의 비슷한 실용적인 컨트롤러
   * ModelView를 직접 생성해서 반환하지 않도록 편리한 인터페이스 제공
   * Controller -> Map<String, String> paramMap으로 파라미터 정보를 받고 Map<String, Object> model객체를 받아서 view파일명을 반환한다.
   
6. V5 유연한 컨트롤러
   * 핸들러 어댑터를 도입하여 V3, V4 핸들러 어댑터가 V3, V4 핸들러를 실행할 수 있게 되었다. 따라서 ControllerV3의 구현 Controller와 ControllerV4의 구현 Controller를 동시에 사용할 수 있다.

7. 추가 확장
  * 애노테이션 핸들러, 애노테이션 핸들러 어댑터를 추가하면 애노테이션 MVC를 구현할 수 있게 된다.
  * 스프링 MVC에서 @RequestMapping("/hello") 컨트롤러는 RequestMappingHandlerAdapter가 실행한다.
  


