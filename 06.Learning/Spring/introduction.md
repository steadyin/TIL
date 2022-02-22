# 스프링 입문

# 1. 프로젝트 환경 설정

* 스프링 부트 스타터 페이지 : http://start.spring.io

* Snapshop : "A snapshot version in Maven is one that has not been released."
  
* Maven : 필요한 라이브러리를 가져오고 빌드 라이프 사이클 관리 도구, 최근은 Gradle을 많이 사용

* Project Metadata
  * groupdId : 기업
  * artifactId : 프로젝트명
 
* build.gradle
  ```text
  plugins {
    id 'org.springframework.boot' version '2.4.4'
    id 'io.spring.dependency-management' version '1.0.11.RELEASE'
    id 'java'
  }
  
  group = 'hello'
  version = '0.0.1-SNAPSHOT'
  sourceCompatibility = '11'
  
  configurations {
    compileOnly {
      extendsFrom annotationProcessor
    }
  }
  
  repositories {
    mavenCentral()
  }
  
  dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
  }
  
  test {
    useJUnitPlatform()
  }
  ```
  mavenCentral() : 라이브러리 다운로드 받는 곳
  
* .gitignore : 필요없는 소스코드 관리

* 라이브러리
  * spring-boot-starter-web
    * spring-boot-starter-tomcat : 톰캣
    * spring-webmvc : 스프링 웹 MVC
  * spring-boot-starter-thymeleaf : 타임리프 템플릿 엔진
  * spring-boot-starter(공통) : 스프링 부트 + 스프링 코어 + 로깅
    * spring-boot
      * spring-core
    * spring-boot-starter-logging
      * log-back, slf4j

* 테스트 라이브러리
  * spring-boot-starter-test
    * junit : 테스트 프레임워크
    * mockito : 목 라이브러리
    * assertJ : 테스트 코드를 좀 더 편하게 작성하게 도와주는 라이브러리
    * spring-test : 스프링 통합 테스트 지원

* Intellij 설정
  * 빌드 설정
    * Preferences -> Build,Execution,Deployment -> Build Tools -> Gradle
    * Build and run using : Gradle -> Intellij IDEA 
    * Runt tests using : Gradle -> Intellij IDEA
    ![img.png](img.png)
   
  * 단축키 설정법
    * Preferences -> Keymap
    ![img_1.png](img_1.png) 

  * JDK 설정
    * Project Structure(Ctrl + Alt + Shift + S)
    ![img_2.png](img_2.png)

  * Html Recompile
    * spring-boot-devtools 라이브러리르 추가하면 html 파일을 컴파일만 해주면 서버 재시작 없이 View 파일 변경이 가능하다. 
  
* View 환경설정
  * 기본 Welcome Page
    * static/index.html
    * http://docs.spring.io/spring-boot/docs/2.3.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-welcome-page

* 빌드하고 실행하기
  * Console -> gradlew build 

* 기본 동작 환경 그림 
  ![img_3.png](img_3.png)
  컨트롤러에서 리턴 값으로 문자를 반환하면 뷰리졸버(ViewResolver)가 화면을 찾아서 처리한다. 스프링 부트 템플릿엔진 기본 viewName 매핑은 resources:templates/ + {ViewName} + .html 이다.

* 윈도우에서 Git Bash 경험하기
  참조 : https://violetboralee.medium.com/intellij-idea%EC%99%80-git-bash-%EC%97%B0%EB%8F%99%ED%95%98%EA%B8%B0-63e8216aa7de
  

# 스프링 웹 개발 기초

## 정적 컨텐츠

컨트롤러에서 리턴 값으로 문자를 반환하면 뷰 리졸버(ViewResolver)가 화면을 찾아서 처리한다.

* 스프링 부틑 템플릿 엔진 기본 viewName 매핑
* resoutce/template/[viewName].html



http://docs.spring.io/spring-boot/docs/2.3.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-welcome-page


   






  

  
   
  
 