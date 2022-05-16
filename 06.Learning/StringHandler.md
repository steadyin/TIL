# Java Spring 과제 연습

목표 - 서비스 기업으로 이직 방향성 제대로 잡기

* 목차
  * 1 주차 - 과제를 함께 풀어보고 공유
  * 2 주차 - 저의 과제 풀이 공유
  * 3 주차 - 협업/테스트/리팩토링 관점에서 과제 개선해보기

* SI기업
  
  * 고객이 원하는 서비스를 기한내에 납품하여 매출 발생
  * 서비스 출시 후 유지보수 계약까지 가는 경우 희박
  * SI개발자 -> 기한 내에 동작하는 기능을 만들어 내는 것에 집중

* 서비스 기업

  * 고객이 서비스를 직접 사용해서 매출을 올림
  * 잠깐의 오류로 매출에 큰 손해가 날 수 있음
  * ![image](https://user-images.githubusercontent.com/79847020/165892365-8ff9cc5d-6d7e-4cf6-a863-b86b3a3a1d99.png)
  * 서비스 기업 개발자 -> 많은 고객에게 정확하게 동작하는 기능을 만들어 내는 것에 집중
  * 코드리뷰 
    *  실수는 없는지 이중체크
    *  요구사항 추가/변경에 유리하도록 리팩토링 제안

![image](https://user-images.githubusercontent.com/79847020/165892528-8c67885c-4228-4e32-8628-8daf9b345467.png)

* 알고리즘 테스트
  * 1 ~ 5 시간
  * 메서드 1개
  * 다수의 지원자를 저비용으로 필터링 하기 위한 목적

* 과제 테스트
  * 1 ~ 7일
  * 프로젝트 구조 전체
  * 함께 일하기 좋은 개발자인지 판단하려는 목적
  * 기술스택 확인 가능(이력서와 일치여부, 회사스택 부합여부)
  * 면접 질문 뽑기 유용

* 과제에서 중점적으로 보는 능력
  * 문제 이해 / 해결 능력
  * 협업능력(커밋 단위, 변수명 등)
  * 클린코드(테스트, 유지보수, 가독성)
  * 자바/스프링 이해도(DI 방법, @Transaction 활용 등)

# 1 주차 - 과제를 함께 풀어보고 공유

## 문제 - URL 파싱 후 데이터 가공하기

![image](https://user-images.githubusercontent.com/79847020/165892888-7eb5a54a-b431-4bd6-8d14-5a5f14d72266.png)

1. URL 입력을 하면 그  링크 안에있는 모든 HTML코드를 불러온다.
2. 노출 유형
   * 노출 유형이 HTML 일  경우는 태그를 제거한다
      1. \<div>abc\</div> -> abc
   * 노출 유형이 TEXT 일  경우에는 모든 텍스트 포함 
      1. \<div>abc\</div> -> \<div>abc\</div>
3. 영어, 숫자만 출력
   * 결과 데이터는 영어와 숫자로만 구성된 데이터만 출력한다
4. 오름차순 정렬
   * 영어 : AaBbCcDd ... Zz 
   * 숫자 : 0123456789
5. 교차 출력
   * 영어 숫자 영어 숫자
   * 더  이상 출력될 숫자 또는 영어가 없을 경우 남아있는 정렬된 문자열 그대로 출력 
6. 출력 묶음 단위
   * 사용자가 입력한 자연수의 묶음 단위를 기준으로 몫과 나머지를 구하여 노출
   * 예) ‘A0a1B2b3’, 출력 묶음 단위 3인경우 
   * 몫 ‘A0a1B2’
   * 나머지 ‘b3’

## 과제전 팁 몇 가지

1. 프론트 페이지는 없어도 됨 -> API 만드는게 목적
2. 예외처리 꼼꼼히
3. 테스트코드 필수
4. 메서드/클래스를 다른 사람이 사용할 거라고 생각하고 작성해보기
5. Swagger적용해보기(면접관이 테스트하기 편해짐)
6. 만약 노출 유형의 종류가 하나 더 늘어난다면 ? (요구사하 추가)
7. README.md 파일 정리하기(프로젝트 실행방법, 구현방법 설명)
8. 여러가지 방법이 있다면 그 중 왜 이 방법을 선택했는지 생각해보기
9. 더 개선할 점은 없는지 생각해보고 리팩토링 하기
10. 커밋 단위는 기능당 한개
11. 커밋 메시지 통일성 있게 
( 잘 모른다면 앵귤러 커밋 컨벤션 참고 - https://velog.io/@outstandingboy/Git-%EC%BB%A4%EB%B0%8B-%EB%A9%94%EC%8B%9C%EC%A7%80-%EA%B7%9C%EC%95%BD-%EC%A0%95%EB%A6%AC-the-AngularJS-commit-conventions)
  * fix : 버그 수정
  * docs : 문서 관련
  * style : 스타일 변경 (포매팅 수정, 들여쓰기 추가, …)
  * refactor : 코드 리팩토링
  * test : 테스트 관련 코드
  * build : 빌드 관련 파일 수정
  * chore : 그 외 자잘한 수정 

## 프로젝트 시작

### Git Repo 생성

![image](https://user-images.githubusercontent.com/79847020/165891645-7431c12b-511b-40f2-b0f7-8fc7309c3d3c.png)

### 프로젝트 생성

![image](https://user-images.githubusercontent.com/79847020/165893494-60215d35-5248-4884-b15d-beab2393f45a.png)

![image](https://user-images.githubusercontent.com/79847020/165893599-5e52aa39-90b7-4363-991d-cd8a4a1631dc.png)

### Git init

```
> echo "# string-handler" >> README.md
> git init
> git add .
> git commit -m "setting:프로젝트 세팅"
> git branch -m main
> git remote add origin git@github.com:steadyin/String-Handler.git
> git push -u origin main
```

### Git Author 정보

* 글로벌 설정

 ```
 git config --global user.name Jihoon-Park/Engineer
 git config --global user.email steadyin95@gmail.com
 ```

* 일반 설정
 
 ```
 git config user.name Jihoon-Park/Engineer
 git config user.email steadyin95@gmail.com
 ```

* Commit 수정

 ```
 git commit --amend --author Jihoon-Park<steadyin95@gmail.com>
 git push -f
 ```

* Issue 만들기

  ![image](https://user-images.githubusercontent.com/79847020/165899869-f9160faa-fe07-44d9-afbc-c9aea53a1f35.png)

  Tracking Number는 PullRequest가 점유할수도 있으므로 순차적이지는 않다. 
  
* 패키지 구조 잡기  

  사실 개발을 하기전에 패키지 구조가 먼저 잡혀져 있으면 좋을 것 같습니다.

  String-handler

  ㄴcontroller

  ㄴservice
  
  구글에서 RestAPI 관련 문서를 살펴보는 것을 추천드립니다. URL 설계시 도움이 됩니다.
  
  URL 파싱 후 HTML 가져오는 기능의 경우 Get, Post 상관없습니다. 여기서는 Post로 하겠습니다.
  
  깃 명령어로 체크아웃을 하며 브랜치를 바로 생성할 수 있습니다. 브랜치 명명 규칙은 회사마다 다릅니다. 여기서는 feature/[이슈번호] 으로 생성하겠습니다.
  
  ```
  git checkout -b feature/1
  ```

  API 만들때 생각해야 될게 요구사항 request, response가 뭔지 생각해야되는데 여기서는 URL, TYPE, 출력 묶음 단위가 request가 되는 거고 몫, 나머지가 response가 됩니다.
  
  controller/ParseController.java
  ```java
  @RestController
  @RequiredArgsConstructor
  public class ParseController {
      private final ParseService parseService;
  
    @PostMapping("api/parse")
    public ResponseEntity<ParseResponse> parse(@RequestBody @Valid ParseRequest request) {
        final ParseResponse response = parseService.parse(request);
        return ResponseEntity.ok(response);
    }
  }
  ```

  dto/ParseRequest.java
  ```java
  //request -> url, type(노출 유형), 출력 묶음 단위
  @RequiredArgsConstructor
  @Getter
  public class ParseRequest {
      private final String url;
      private final String exposureType;
      private final Integer unitCount;
  }
  ```

  dto/ParseResponse.java
  ```java
  //response -> 몫, 나머지
  @RequiredArgsConstructor
  public class ParseResponse {
     private final String quotient;
     private final String remainder;
  } 
  ```

  DTO의 필드는 final를 붙여두고 @RequiredArgsConstructor 롬복 애노테이션을 적용합니다.

  service/ParseService.java
  ```java
  @Service
  public class ParseService {
      public ParseResponse parse(ParseRequest request) {
          return null;
      }
  }
  ```

* 의존성

  의존성 주입(DI)
  - 필드 주입, Setter 주입, 생성자 주입
  - 생성자 주입으로 하자.
  - 생성자 주입으로 할 때는 필드에 final을 적용하고 @RequireArgsConstructor Lombok 애노테이션을 활용하자.
  - @Autowired를 기피하자.

  여기까지해서 커밋을 해보겠습니다. 
  
  

```markdown
PS C:\study\98.WORK\string-handler> git status
On branch feature/1
Untracked files:
(use "git add <file>..." to include in what will be committed)
src/main/java/com/steadyin/stringhandler/controller/
src/main/java/com/steadyin/stringhandler/dto/
src/main/java/com/steadyin/stringhandler/service/

nothing added to commit but untracked files present (use "git add" to track)
PS C:\study\98.WORK\string-handler> git add .
```

git add . 할 때는 커밋 대상이 아닌 대상이 있는지 잘 살펴보아야 합니다. 이 커밋은 세팅에 가까우므로 setting 커밋 메시지를 사용하겠습니다.

```markdown
PS C:\study\98.WORK\string-handler> git commit -m "setting:컨트롤러, 서비스 세팅"
[feature/1 0c67625] setting:컨트롤러, 서비스 세팅
4 files changed, 61 insertions(+)
create mode 100644 src/main/java/com/steadyin/stringhandler/controller/ParseController.java
create mode 100644 src/main/java/com/steadyin/stringhandler/dto/ParseRequest.java
create mode 100644 src/main/java/com/steadyin/stringhandler/dto/ParseResponse.java
create mode 100644 src/main/java/com/steadyin/stringhandler/service/ParseService.java
```

사실은 현재 커밋에 해당하는 이슈를 하나 더 추가했었으면 좋은데 이슈를 이미 만들었으므로 이번 커밋을 'URL 파싱 후 HTML 가져오기' 이슈에 적용하도록 하겠습니다.

과제 팁에서 얘기드린게 다른 사람이 구현한 기능을 사용할 수 있을것이라고 생각하고 작성해보자 라고 했었는데 이 패키지 담당이 아닌 사람이 봤을 때 가져다 써도 괜찮다는 의미의 패키지를 만드는 것을 추천드립니다. 예를 들면 util이나 common같은. 여기서는 util패키지를 만들고 URLConnector라는 클래스를 생성하겠습니다.

```java
// static method vs @Component
package com.steadyin.parse.util;

@Component
public class UrlConnector {
}
```

유틸성 클래스를 만들 때 Static 메소드를 만드는 것을 나을까 스프링의 컴포넌트를 활용하는 것이 나을까 고민해볼만한 주제가 있는데 검색을 해보니깐 @Component가 낫다고 합니다. 만약 여기서 @Component를 활용했다고 치면 두 가지 방식의 차이점에 대해서 면접 질문을 받을수도 있습니다. 그래서 선택을 할 때 하나하나 구글링 해가면서 선택을 해가셨으면 좋겠습니다. 

여기서는 @Component 애노테이션을 사용해서 스프링 빈으로 등록해서 클래스의 기능을 사용할 수 있도록 하겠습니다. 

URL을 어떻게 가져올 것인가에 대해서 고민을 해보아야 되는데 여러가지 방법이 있는데 RestTemplate를 사용하거나, WebClient을 사용하거나, Jsoup를 사용하거나 등등 여러가지 방법이 있을텐데 어떤 것을 사용하든 크게 중요하지 않아서 Jsoup를 임의로 선택해서 사용해보도록 하겠습니다. 하지만 사용하더라도 장/단점을 찾아보시는게 좋을 것 같습니다.

build.gradle에 라이브러리를 추가하겠습니다.
```
implementation 'org.jsoup:jsoup:1.14.3'
```
라이브러리 추가 된 것을 확인합니다.
![image](https://user-images.githubusercontent.com/79847020/168031736-eaa91b26-b605-491d-9c23-5a7aebd600cd.png)

Jsoup를 이용해서 다음과 같이 작성합니다.
```java
package com.steadyin.parse.util;

import org.jsoup.Jsoup;

public class UrlConnector {

    public String getHtml(final String url) {
        try {
            return Jsoup.connect(url).get().html();
        } catch (IOException e) {
            throw new IllegalArgumentException("접근할 수 없는 url입니다.");
        }
    }
}
```
커스텀 익셉션을 작성하는 것이 더 좋겠죠. 일단 이렇게 하고 나중에 바꾸겠습니다.

테스트를 작성하겠습니다.

## 테스트 작성

일단 URLConnector를 사용할 수 있도록 세팅을 해야 하자나요. 그런데 URLConnector를 보면 아시겠지만 생성자가 없잖아요. 꼭 컴포넌트라고 해서 테스트를 할 때 꼭 Autowired를 해야되는 것은 아니거든요. 일반 클래스와 같이 때문에 테스트 코드에서는 조금 더 빨리 로딩할 수 있게 new 연산자를 사용해서 다음과 같이 가져오는 방법이 있습니다. 

```java
class UrlConnectorTest  {
   private final UrlConnector urlConnector = new UrlConnector();
}
``` 

다음과 같이 두가지 테스트 메소드를 추가하겠습니다.

```java
   @DisplayName("url입력시 html을 가져올 수 있다.")
   @Test
   void urlSuccessTest() {

   }

   @DisplayName("잘못된 url입력시 IllegalArgumentException이 발생한다.")
   @Test
   void urlFaildTest() {

   }
```

다음과 같이 성공 테스트를 간단하게 작성할 수 있습니다.
```java
    @DisplayName("url입력시 html을 가져올 수 있다.")
    @Test
    void urlSuccessTest() {
        final String result = urlConnector.getHtml("http://www.naver.com");

        assertThat(result.contains("<title>NAVER</title>")).isTrue();
    }
```
일단 네이버만 테스트를 했는데 다른 URL 테스트도 진행하고 싶잖아요.

@ParameterizedTest를 활용할 수 있습니다. @ParameterizedTest를 적용하고 @CsvSource를 활용해서 파라미터를 넘겨서 받을 수 있습니다.
```java
    @DisplayName("url입력시 html을 가져올 수 있다.")
    @ParameterizedTest
    @CsvSource({"https://www.naver.com, <title>NAVER</title>", "https://www.google.com, <title>Google</title>"})
    void urlSuccessTest(final String url, final String title) {
        final String result = urlConnector.getHtml(url);

        assertThat(result.contains(title)).isTrue();
    }
```

완성 된 테스트는 다음과 같다.
```java
class UrlConnectorTest {
    private final UrlConnector urlConnector = new UrlConnector();

    @DisplayName("url입력시 html을 가져올 수 있다.")
    @ParameterizedTest
    @CsvSource({"https://www.naver.com, <title>NAVER</title>", "https://www.google.com, <title>Google</title>"})
    void urlSuccessTest(final String url, final String title) {
        final String result = urlConnector.getHtml(url);

        assertThat(result.contains(title)).isTrue();
    }

    @DisplayName("잘못된 url입력시 IllegalArgumentException이 발생한다.")
    @ParameterizedTest
    @CsvSource({"https://www.naver.fail", "https://www.fail.fail"})
    void urlFaildTest(final String url) {
        assertThatThrownBy(() -> urlConnector.getHtml(url)).isInstanceOf(IllegalArgumentException.class).hasMessage("접근할 수 없는 url입니다.");
    }
}
```

추가로 hasMessage()로 예외 메시지까지 테스트 확인할 수 있음을 알아두자.

## 서비스 작성

UrlConnector를 Component로 등록했기 때문에 ParseService에 DI할 수 있습니다. 

```java
@Service
@RequiredArgsConstructor
public class ParseService {

    private final UrlConnector urlConnector;

    public ParseResponse parse(ParseRequest request) {
        System.out.println(urlConnector.getHtml("https://naver.com"));
        return null;
    }
}
```

그럼 이슈 'URL 파싱 후 HTML 가져오기'는 끝났습니다. git status 확인 해보고   
```markdown
PS C:\Study\String-Handler> git status
On branch feature/1
Your branch is ahead of 'origin/feature/1' by 1 commit.
  (use "git push" to publish your local commits)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   src/main/java/com/steadyin/stringhandler/service/ParseService.java

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        src/main/java/com/steadyin/stringhandler/util/
        src/test/java/com/steadyin/stringhandler/util/

no changes added to commit (use "git add" and/or "git commit -a")
```
Commit하겠습니다.
```markdown
PS C:\Study\String-Handler> git add .
PS C:\Study\String-Handler> git commit -m "feat:UrlConnector 추가"
[feature/1 adb2f0e] feat:UrlConnector 추가
 3 files changed, 56 insertions(+)
 create mode 100644 src/main/java/com/steadyin/stringhandler/util/UrlConnector.java
 create mode 100644 src/test/java/com/steadyin/stringhandler/util/UrlConnectorTest.java
```
```markdown
PS C:\Study\String-Handler> git status
On branch feature/1
Your branch is ahead of 'origin/feature/1' by 2 commits.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean

```
```markdown
PS C:\Study\String-Handler> git push --set-upstream origin feature/1
warning: redirecting to https://github.com/steadyin/string-handler/
Enumerating objects: 46, done.
Counting objects: 100% (46/46), done.
Delta compression using up to 8 threads
Compressing objects: 100% (20/20), done.
Writing objects: 100% (35/35), 3.72 KiB | 761.00 KiB/s, done.
Total 35 (delta 6), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (6/6), completed with 2 local objects.
remote: This repository moved. Please use the new location:
remote:   https://github.com/steadyin/String-Handler.git
To http://github.com/steadyin/string-handler
   e53f42b..adb2f0e  feature/1 -> feature/1
Branch 'feature/1' set up to track remote branch 'feature/1' from 'origin'.
```
그럼 Github페이지에서 Pull Request를 확인할 수 있습니다. Compare & Pull Request 버튼을 클릭합니다.

![image](https://user-images.githubusercontent.com/79847020/168230206-75b1c22d-e982-48bf-b0c2-3068275d9b15.png)

PullRequest Title과 Assignees 설정을 하고 Pull Request를 생성합니다.

Development메뉴에서 이슈를 연결시킬 수 있습니다.

![image](https://user-images.githubusercontent.com/79847020/168230737-b620eb1d-cc0d-4bf8-a1ce-e5c4f09fb296.png)

PullRequest가 완성되었습니다.

![image](https://user-images.githubusercontent.com/79847020/168231062-3790fc7a-5258-4f2d-9098-447f214bea49.png)

 그 다음 Reviewer가 확인하고 메시지를 남기거나 코드에 Review를 남기거나 개발을 해 나가는거죠. 

Merge Pull Request 옵션이 3가지 있는데 Squash는 여러가지 Commit을 하나의 Pull Request에 올렸을 때 하나로 뭉쳐서 Merge를 하는 옵션이 있고 Rebase는 MergeComit이 올라가는게 아니라 여러분이 작성했던 Setting이랑 Feat가 그대로 Main Branch에 합쳐지게 되는 옵션입니다. 이번에는 Rebase And Merge 옵션을 사용하겠습니다.

![image](https://user-images.githubusercontent.com/79847020/168231119-efd15518-57c7-4222-9839-5f9cf7008d6e.png)

Pull request successfully merged and closed 메시지가 출력되고 이슈 메뉴에 다시 들어가면 Close된 것을 확인할 수 있습니다. 

![image](https://user-images.githubusercontent.com/79847020/168231228-b2cd717e-c3c4-4647-8a02-cffa5b15226b.png)

Commit History에 Rebase And Merge를 했으므로 setting과 feat이 그대로 Main Branch에 쌓여지게 된 것을 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/79847020/168231269-99113737-bd3a-43fc-b420-eac3f84633c4.png)

History를 보면 Commit들이 반영되어 있습니다.

![image](https://user-images.githubusercontent.com/79847020/168231527-5cbd84cd-3f6c-48ba-a3f9-cecf775189ae.png)


그 다음 'feature/1' 브랜치는 완료했으므로 main브랜치로 돌아갑니다. 

```markdown
PS C:\study\98.WORK\string-handler> git checkout main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
```

Git pull 명령을 실행합니다.
```markdown
PS C:\study\98.WORK\string-handler> git pull
remote: Enumerating objects: 53, done.
remote: Counting objects: 100% (53/53), done.
remote: Compressing objects: 100% (18/18), done.
remote: Total 42 (delta 8), reused 40 (delta 8), pack-reused 0
Unpacking objects: 100% (42/42), 4.57 KiB | 334.00 KiB/s, done.
From https://github.com/steadyin/string-handler
   9ac3f04..488a8ba  main       -> origin/main
Updating 9ac3f04..488a8ba
Fast-forward
 build.gradle                                       |  1 +
 .../StringHandlerApplication.java                  |  2 +-
 .../steadyin/parse/controller/ParseController.java | 28 ++++++++++++++++++++
 .../java/com/steadyin/parse/dto/ParseRequest.java  | 11 ++++++++
 .../java/com/steadyin/parse/dto/ParseResponse.java | 10 ++++++++
 .../com/steadyin/parse/service/ParseService.java   | 19 ++++++++++++++
 .../java/com/steadyin/parse/util/UrlConnector.java | 18 +++++++++++++
 .../StringHandlerApplicationTests.java             |  2 +-
 .../com/steadyin/parse/util/UrlConnectorTest.java  | 30 ++++++++++++++++++++++
 9 files changed, 119 insertions(+), 2 deletions(-)
 rename src/main/java/com/steadyin/{stringhandler => parse}/StringHandlerApplication.java (89%)
 create mode 100644 src/main/java/com/steadyin/parse/controller/ParseController.java
 create mode 100644 src/main/java/com/steadyin/parse/dto/ParseRequest.java
 create mode 100644 src/main/java/com/steadyin/parse/dto/ParseResponse.java
 create mode 100644 src/main/java/com/steadyin/parse/service/ParseService.java
 create mode 100644 src/main/java/com/steadyin/parse/util/UrlConnector.java
 rename src/test/java/com/steadyin/{stringhandler => parse}/StringHandlerApplicationTests.java (84%)
 create mode 100644 src/test/java/com/steadyin/parse/util/UrlConnectorTest.java
```

feature/2 작업을 시작하겠습니다.

```markdown
PS C:\study\98.WORK\string-handler> git checkout -b feature/2
Switched to a new branch 'feature/2'
```

str Enum클래스를 만들겠습니다. 값들을 정하시기 나름인대 프론트담당자와 얘기해서 마추셔도 되고 문서를 작싱하셔도 됩니다. 사실은 REMOVE_HTML, ALL_TEXT 2가지만 정의하면 끝나는데 여기에 기능이 추가되겠죠. REMOVE_HTML일 때는 HTML을 다 지워야 하고 ALL_TEXT일 때는 HTML도 포함해야겠죠. 

이것을 다루는 방법이 많은데 처음에 전략패턴을 사용해서 다음과 같이 작성하는 방법도 있습니다. 
```java
public enum str {
    REMOVE_HTML(new A전략),
    ALL_TEXT(new B전략)
}
```
여기서는 Function을 사용해서 람다식을 이용하는 방법을 사용하도록 하겠습니다.

Function인터페이스를 살펴보면 다음과 같습니다.
```java
/**
 Represents a function that accepts one argument and produces a result.  This is a functional interface whose functional method is apply(Object).
 Since: 1.8
 Type parameters:
 <T> – the type of the input to the function
 <R> – the type of the result of the functio
 */
@FunctionalInterface
public interface Function<T, R> {
   ...생략
}
```
그래서 다음과 같이 작성하면 String을 입력하면 String을 리턴하는 Function을 정의할 수 있습니다. 대략 다음과 같이 작성할 수 있습니다.

```java
@RequiredArgsConstructor
public enum ExposureType {
   REMOVE_HTML(str -> str.replaceAll("<[^>]*>", "")), 
   ALL_TEXT(str -> str);

   private final Function<String, String> function;
}
```

ALL_TEXT는 들어온 str이 그대로 return 되면 됩니다. REMOVE_HTML은 replaceAll 메소드로 정규식을 ""(empty)로 replace하겠습니다.
여기서는 간단하게 \<,\>만 제거하는 것으로 생각해서 정규식으로 표현했습니다. ""또한 마찬가지 입니다.
정규식의 경우 코드에 "\<[^\>]*\>"로만 작성되어 있으면 다른 사람이 이해하기 어려우므로 상수로 뺍니다. 
인텔리 J 단축키 Introduce Constant : Ctrl + Alt + C 로 상수를 정리하면 다음과 같습니다.

```java
@RequiredArgsConstructor
public enum ExposureType {
    REMOVE_HTML(str -> str.replaceAll(Constants.REMOVE_TAG_PATTERN, Constants.EMPTY)),
    ALL_TEXT(str -> str);

    private static class Constants {
        public static final String REMOVE_TAG_PATTERN = "<[^>]*>";
        public static final String EMPTY = "";
    }
}
```

function을 정의하고 

```java
    private final Function<String, String> function;
```

사용하는부분을 작성합니다.

```java
    public String getExposedHtml(final String str) {
        return function.apply(str);
    }
```    
    
```java
@RequiredArgsConstructor
public enum ExposureType {
    REMOVE_HTML(str -> str.replaceAll(Constants.REMOVE_TAG_PATTERN, Constants.EMPTY)),
    ALL_TEXT(str -> str);

    private final Function<String, String> function;

    public String getExposedHtml(final String str) {
        return function.apply(str);
    }

    private static class Constants {
        public static final String REMOVE_TAG_PATTERN = "<[^>]*>";
        public static final String EMPTY = "";
    }
}
```

이런식으로 작성해서 각 ENUM에 다른 기능을 넣어줄 수 있습니다. 테스트를 만들어보도록 하겠습니다.

```java
class ExposureTypeTest {
   @DisplayName("REMOVE_HTML 타입인 경우 태그를 삭제한다")
   @Test
   void removeHtml() {
      final String result = ExposureType.REMOVE_HTML.getExposedHtml("<div>abc</div>");
      assertThat(result).isEqualTo("abc");
   }

   @DisplayName("ALL_TEXT 타입인 경우 모든 텍스트를 가져온다.")
   @Test
   void allText() {
      final String result = ExposureType.ALL_TEXT.getExposedHtml("<div>abc</div>");
      assertThat(result).isEqualTo("<div>abc</div>");
   }
}
```

상동작하고 이것을 이제 서비스에 녹이면 되겠죠.

ParseRequest에 Getter과 ExposureType 타입으로 변경해주고.
```java
@RequiredArgsConstructor
@Getter
public class ParseRequest {
    private final String url;
    private final ExposureType exposureType;
    private final Integer unitCount;
}
```
ParseService를 작성합니다.
```java
@Service
@RequiredArgsConstructor
public class ParseService {

    private final UrlConnector urlConnector;

    public ParseResponse parse(ParseRequest request) {
        final String html = urlConnector.getHtml(request.getUrl());
        final String exposedHtml = request.getExposureType().getExposedHtml(html);

        return null;
    }
}
```
여기까지 하나의 Issue가 끝났죠. 

git status
```markdown
PS C:\study\98.WORK\string-handler> git status
On branch feature/2
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   src/main/java/com/steadyin/parse/dto/ParseRequest.java
        modified:   src/main/java/com/steadyin/parse/service/ParseService.java

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        src/main/java/com/steadyin/parse/dto/ExposureType.java
        src/test/java/com/steadyin/parse/dto/

no changes added to commit (use "git add" and/or "git commit -a")
```

git add / git commit
```markdown
PS C:\study\98.WORK\string-handler> git add .
PS C:\study\98.WORK\string-handler> git commit -m "feat:노출 유형 추가"
[feature/2 2442bc8] feat:노출 유형 추가
 4 files changed, 51 insertions(+), 2 deletions(-)
 create mode 100644 src/main/java/com/steadyin/parse/dto/ExposureType.java
 create mode 100644 src/test/java/com/steadyin/parse/dto/ExposeTypeTest.java
```

git push
```markdown
PS C:\Study\String-Handler> git push --set-upstream origin feature/2
warning: redirecting to https://github.com/steadyin/string-handler/
Enumerating objects: 34, done.
Counting objects: 100% (34/34), done.
Delta compression using up to 8 threads
Compressing objects: 100% (11/11), done.
Writing objects: 100% (20/20), 2.11 KiB | 1.05 MiB/s, done.
Total 20 (delta 3), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (3/3), completed with 3 local objects.
remote: This repository moved. Please use the new location:
remote:   https://github.com/steadyin/String-Handler.git
remote: 
remote: Create a pull request for 'feature/2' on GitHub by visiting:
remote:      https://github.com/steadyin/String-Handler/pull/new/feature/2
remote:
To http://github.com/steadyin/string-handler
 * [new branch]      feature/2 -> feature/2
Branch 'feature/2' set up to track remote branch 'feature/2' from 'origin'.

```

Create PullRequest / Linking Issue / Rebase and Merge
![image](https://user-images.githubusercontent.com/79847020/168232730-dd2bd0ca-b388-4897-a21e-a2e107d63db4.png)

git status
```markdown
PS C:\study\98.WORK\string-handler> git status
On branch feature/2
nothing to commit, working tree clean
```

git checkout main
```markdown
PS C:\study\98.WORK\string-handler> git checkout main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
```

git pull
```markdown
PS C:\study\98.WORK\string-handler> git pull
remote: Enumerating objects: 34, done.
remote: Counting objects: 100% (34/34), done.
remote: Compressing objects: 100% (8/8), done.
remote: Total 20 (delta 3), reused 19 (delta 3), pack-reused 0
Unpacking objects: 100% (20/20), 2.14 KiB | 66.00 KiB/s, done.
From https://github.com/steadyin/string-handler
   488a8ba..fa10b08  main       -> origin/main
Updating 488a8ba..fa10b08
Fast-forward
 .../java/com/steadyin/parse/dto/ExposureType.java  | 22 ++++++++++++++++++++++
 .../java/com/steadyin/parse/dto/ParseRequest.java  |  5 ++++-
 .../com/steadyin/parse/service/ParseService.java   |  4 +++-
 .../com/steadyin/parse/dto/ExposeTypeTest.java     | 22 ++++++++++++++++++++++
 4 files changed, 51 insertions(+), 2 deletions(-)
 create mode 100644 src/main/java/com/steadyin/parse/dto/ExposureType.java
 create mode 100644 src/test/java/com/steadyin/parse/dto/ExposeTypeTest.java
```

github 확인
![image](https://user-images.githubusercontent.com/79847020/168241964-071cbaa1-c7aa-41d0-9944-44e68c81f4d1.png)
![image](https://user-images.githubusercontent.com/79847020/168242015-62eed6e6-7eea-459e-8a64-12ab6d6f308d.png)

## 영어, 숫자 분리

feature/3 브랜치를 체크아웃 하겠습니다.
```markdown
PS C:\study\98.WORK\string-handler> git checkout -b feature/3
Switched to a new branch 'feature/3'
```

Separator클래스를 생성합니다. Separator는 영어와 숫자를 분리하는 역할을 담당할것입니다. 방법이 여러가지가 있을텐데 정규식을 이용하도록 하겠습니다.

정규식에 그룹핑하는 기능이 있습니다. 그룹핑 기능을 이용해서 영어와 숫자를 분리합니다.

Separator
```java
@Component
public class Separator {
    private final Pattern PATTERN = Pattern.compile("([A-Za-z]*)([0-9]*)");

    @Getter
    private String english;

    @Getter
    private String number;

    public Separator separate(final String str) {
        final Matcher matcher = PATTERN.matcher(str);

        StringBuilder sbStr = new StringBuilder();
        StringBuilder sbNUm = new StringBuilder();

        while(matcher.find()) {
            sbStr.append(matcher.group(1));
            sbNUm.append(matcher.group(2));
        }

        this.english = sbStr.toString();
        this.number = sbNUm.toString();
        return this;
    }
}
```

Separator Test
```java
class SeparatorTest {
    private final Separator separator = new Separator();

    @DisplayName("String을 입력받으면 영어부분과 숫자부분을 얻을 수 있다.")
    @Test
    void normalCase() {
        final Separator sep = separator.separate("e4C3dBA1aDf2cEAb0bF");
        assertAll(
                () -> assertThat(sep.getEnglish()).isEqualTo("eCdBAaDfcEAbbF"),
                () -> assertThat(sep.getNumber()).isEqualTo("43120")
        );
    }

    @DisplayName("String을 입력받을 때 숫자와 영어 이외의 값이 포함된 경우 지운다")
    @Test
    void removeSpecialChars() {
        final Separator sep = separator.separate("e4C3!d@BA#1a$Df$2c^EA[b0]bF");
        assertAll( //AssertAll
                () -> assertThat(sep.getEnglish()).isEqualTo("eCdBAaDfcEAbbF"),
                () -> assertThat(sep.getNumber()).isEqualTo("43120")
        );
    }
}
```

여러가지 테스트 같은 메서드에 동시에 할 때가 있습니다.

다음과 같이 작성할 수 있습니다.
```java
    assertThat(sep.getEnglish()).isEqualTo("eCdBAaDfcEAbbF"),
    assertThat(sep.getNumber()).isEqualTo("43120")
```
이렇게 작성하면 둘 다 정상동작하지 않을 때 첫번째 메소드만 테스트하게 됩니다. 이런 것들이 불편할수 있는데 assertAll을 사용해서 위와 같이 작성할 수도 있습니다. assertAll을 사용하면 각 테스트 명령어에 상관없이 모든 테스트 명령어가 실행됩니다. 여러가지 테스트를 한 메서드에 실행할 때 사용하면 유용하다.

Separator에서 영어와 숫자를 뽑아 올 수도 있겠지만 Separator 인스턴스르 통째로 넘겨서 다음 기능을 추가할 것이기 때문에 다음과 같이 작성했습니다.

ParseService
```java
@Service
@RequiredArgsConstructor
public class ParseService {

    private final UrlConnector urlConnector;

    private final Separator separator;

    public ParseResponse parse(ParseRequest request) {
        final String html = urlConnector.getHtml(request.getUrl());
        final String exposedHtml = request.getExposureType().getExposedHtml(html);
        final Separator separate = separator.separate(exposedHtml);

        return null;
    }
}
```





git status
```markdown
PS C:\study\98.WORK\string-handler> git status
On branch feature/3
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        src/main/java/com/steadyin/parse/util/Separator.java
        src/test/java/com/steadyin/parse/util/SeparatorTest.java

nothing added to commit but untracked files present (use "git add" to track)
```

git add/ git commit
```markdown
PS C:\study\98.WORK\string-handler> git add .
PS C:\study\98.WORK\string-handler> git commit -m "feat:영어 숫자 분리 추가"
[feature/3 a767288] feat:영어 숫자 분리 추가
 2 files changed, 65 insertions(+)
 create mode 100644 src/main/java/com/steadyin/parse/util/Separator.java
 create mode 100644 src/test/java/com/steadyin/parse/util/SeparatorTest.java
```

git push
```markdown
PS C:\study\98.WORK\string-handler> git push --set-upstream origin feature/3
Enumerating objects: 34, done.
Counting objects: 100% (34/34), done.
Delta compression using up to 8 threads
Compressing objects: 100% (12/12), done.
Writing objects: 100% (20/20), 2.20 KiB | 2.20 MiB/s, done.
Total 20 (delta 4), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (4/4), completed with 4 local objects.
remote:
remote: Create a pull request for 'feature/3' on GitHub by visiting:
remote:      https://github.com/steadyin/string-handler/pull/new/feature/3
remote:
To https://github.com/steadyin/string-handler.git
 * [new branch]      feature/3 -> feature/3
Branch 'feature/3' set up to track remote branch 'feature/3' from 'origin'.
```

Create Pull Request, Rebase And Merge
![image](https://user-images.githubusercontent.com/79847020/168242783-e563ced9-6313-4f38-8146-7236e1968c13.png)

git pull
```
PS C:\study\98.WORK\string-handler> git pull
remote: Enumerating objects: 34, done.
remote: Counting objects: 100% (34/34), done.
remote: Compressing objects: 100% (8/8), done.
remote: Total 20 (delta 4), reused 19 (delta 4), pack-reused 0
Unpacking objects: 100% (20/20), 2.21 KiB | 141.00 KiB/s, done.
From https://github.com/steadyin/string-handler
   fa10b08..2af1697  main       -> origin/main
Updating fa10b08..2af1697
Fast-forward
 .../com/steadyin/parse/service/ParseService.java   |  4 +++
 .../java/com/steadyin/parse/util/Separator.java    | 34 ++++++++++++++++++++++
 .../java/com/steadyin/parse/util/UrlConnector.java |  3 +-
 .../com/steadyin/parse/util/SeparatorTest.java     | 31 ++++++++++++++++++++
 4 files changed, 71 insertions(+), 1 deletion(-)
 create mode 100644 src/main/java/com/steadyin/parse/util/Separator.java
 create mode 100644 src/test/java/com/steadyin/parse/util/SeparatorTest.java
```



## 오름차순 정렬

git checkout -b feature/4
```java
PS C:\study\98.WORK\string-handler> git checkout -b feature/4
Switched to a new branch 'feature/4'
```
Sorter
```java
@Component
public class Sorter {
    public String sort(final String str) {
        return Arrays.stream(str.split(""))
                .sorted() //AAAAABBBBBCCCCCC...aaaabbcc
                .sorted(String.CASE_INSENSITIVE_ORDER) // AAAAAaaaaBBBBBbbCCCCCCcc...
                .collect(Collectors.joining()); //
    }
}
```
String.CASE_INSENSITIVE_ORDER은 대소문자 구분없이 정렬할 수 있는 Comparator 입니다.

Sorted() 메소드를 2번 호출하므로 성능이 좋지않은 것 아닌가? 라는 의문이 들 수 있다. 성능이 안 좋을 수 있지만 하나의 steram안에서 수행되기 때문에 큰 부담이 없을 거라 생각했습니다. 그래도 개선할 수 있다면 개선해서 수정하는 것이 면접시에 어필 포인트가 되겠죠.

```java
class SorterTest {
    private final Sorter sorter = new Sorter();

    @ParameterizedTest
    @DisplayName("영어를 정렬할 때 AaBb 순으로 오른차순 정렬한다.")
    @CsvSource({"fdfdsfasfsdfTEStsstfasasaszA,AaaaadddEffffffSssssssssTttz"})
    void englishSortTest(String org, String target) {
        final String result = sorter.sort(org);

        assertThat(result).isEqualTo(target);
    }

    @Test
    @DisplayName("숫자를 정렬할 때 0~9 순으로 오름차순으로 정렬한다.")
    void numberSortTest() {
        final String result = sorter.sort("78272844709124");

        assertThat(result).isEqualTo("01222444777889");
    }
}
```

근데 생각해보셔야 되는게 Sorter만으로 해결되지 않습니다. 왜냐하면 Separator도 하고 Sorter도 둘다 적용되어야 합니다. 

Separated된 영문과 숫자를 받아서 Sorter는 자동으로 해주는 Arranger를 하나 더 만들어보겠습니다. Arranger는 Separator와 Sorter를 받아서 그 둘을 합쳐주는 용도로 사용하면 됩니다. 

```java
@Component
@RequiredArgsConstructor
public class Arranger {

    private final Separator separator;

    private final Sorter sorter;

    @Getter
    private String sortedEnglish;

    @Getter
    private String sortedNumber;

    public Arranger rearrange(final String str) {
        Separator separate = separator.separate(str);
        this.sortedEnglish = sorter.sort(separator.getEnglish());
        this.sortedNumber = sorter.sort(separator.getNumber());
        return this;
    }
}
```

ArrangerTest
```java
class ArrangerTest {
    private final Arranger arranger = new Arranger((new Separator()), new Sorter());

    @DisplayName("String을 입력받으면 정렬된 영어 = 정렬된 숫자를 얻을 수 있다.")
    @Test
    void normalCase() {
        final Arranger arrange = arranger.rearrange("e4C3dBA1aDf2cEAb0bF");
        assertAll(
                () -> assertThat(arrange.getSortedEnglish()).isEqualTo("AAaBbbCcDdEeFf"),
                () -> assertThat(arrange.getSortedNumber()).isEqualTo("01234")
        );
    }

    @DisplayName("String을 입력받을 때 숫자와 영어 이외의 값이 포함된 경우 지운다.")
    @Test
    void removeSpecialChars() {
        final Arranger arrange = arranger.rearrange("e4C3dBA1aDf2cEAb0@bF");
        assertAll(
                () -> assertThat(arrange.getSortedEnglish()).isEqualTo("AAaBbbCcDdEeFf"),
                () -> assertThat(arrange.getSortedNumber()).isEqualTo("01234")
        );
    }
}
```

ParseService
```java
@Service
@RequiredArgsConstructor
public class ParseService {

    private final UrlConnector urlConnector;

    private final Arranger arranger;

    public ParseResponse parse(ParseRequest request) {
        final String html = urlConnector.getHtml(request.getUrl());
        final String exposedHtml = request.getExposureType().getExposedHtml(html);
        final Arranger rearrange = arranger.rearrange(exposedHtml);

        return null;
    }
}
```

2가지 컴포넌트가 추가되었으므로 Github Issue를 다시 수정하겠습니다.

![image](https://user-images.githubusercontent.com/79847020/168524793-6d69889f-5900-4bdf-85bb-749fbaeff7e4.png)

git status
```
PS C:\study\98.WORK\string-handler> git status
On branch feature/4
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   src/main/java/com/steadyin/parse/service/ParseService.java
        modified:   src/test/java/com/steadyin/parse/util/SeparatorTest.java

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        src/main/java/com/steadyin/parse/util/Arranger.java
        src/main/java/com/steadyin/parse/util/Sorter.java
        src/test/java/com/steadyin/parse/util/ArrangerTest.java
        src/test/java/com/steadyin/parse/util/SorterTest.java

no changes added to commit (use "git add" and/or "git commit -a")
```
git add .
```java
PS C:\study\98.WORK\string-handler> git add .
```

Commit을 나누는게 좋아보이므로 나누겠습니다.

git status
```
PS C:\study\98.WORK\string-handler> git status
On branch feature/4
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   src/main/java/com/steadyin/parse/service/ParseService.java
        new file:   src/main/java/com/steadyin/parse/util/Arranger.java
        new file:   src/main/java/com/steadyin/parse/util/Sorter.java
        new file:   src/test/java/com/steadyin/parse/util/ArrangerTest.java
        modified:   src/test/java/com/steadyin/parse/util/SeparatorTest.java
        new file:   src/test/java/com/steadyin/parse/util/SorterTest.java
```
git restore
```
git restore --staged src/main/java/com/steadyin/parse/util/Arranger.java src/test/java/com/steadyin/parse/util/ArrangerTest.java src/main/java/com/steadyin/parse/service/ParseService.java
```

git status
```
PS C:\study\98.WORK\string-handler> git status
On branch feature/4
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   src/main/java/com/steadyin/parse/util/Sorter.java
        modified:   src/test/java/com/steadyin/parse/util/SeparatorTest.java
        new file:   src/test/java/com/steadyin/parse/util/SorterTest.java

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        src/main/java/com/steadyin/parse/util/Arranger.java
        src/test/java/com/steadyin/parse/util/ArrangerTest.java
        src/main/java/com/steadyin/parse/service/ParseService.java
```
git commit
```
git commit -m "feat:Sorter 추가"
```
git add / git commit
```
PS C:\study\98.WORK\string-handler> git add .
PS C:\study\98.WORK\string-handler> git commit -m "feat:Arranger 추가"
[feature/4 64274ec] feat:Arranger 추가
 3 files changed, 61 insertions(+), 2 deletions(-)
 create mode 100644 src/main/java/com/steadyin/parse/util/Arranger.java
 create mode 100644 src/test/java/com/steadyin/parse/util/ArrangerTest.java
```

git push
```java
PS C:\study\98.WORK\string-handler> git push origin feature/4
  Enumerating objects: 51, done.
  Counting objects: 100% (51/51), done.
  Delta compression using up to 8 threads
  Compressing objects: 100% (22/22), done.
  Writing objects: 100% (37/37), 3.47 KiB | 1.16 MiB/s, done.
  Total 37 (delta 10), reused 0 (delta 0), pack-reused 0
  remote: Resolving deltas: 100% (10/10), completed with 4 local objects.
  remote:
  remote: Create a pull request for 'feature/4' on GitHub by visiting:
  remote:      https://github.com/steadyin/string-handler/pull/new/feature/4
  remote:
  To https://github.com/steadyin/string-handler.git
  * [new branch]      feature/4 -> feature/4
```

PullRequest & Rebase And Merge
![image](https://user-images.githubusercontent.com/79847020/168525029-1c7a487c-cd74-4ea7-8d3e-0b1f5692cbf1.png)

feature/4 브랜치에서 작업한 Commit2개 'feat;Sorter 추가', 'feat:Arranger 추가'가 main 브랜치에 잘 추가된 것을 확인할 수 있다.

로컬저장소에 상태를 업데이트 한다

git checkout main / git pull
```
PS C:\study\98.WORK\string-handler> git checkout main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.

PS C:\study\98.WORK\string-handler> git pull
remote: Enumerating objects: 51, done.
remote: Counting objects: 100% (51/51), done.
remote: Compressing objects: 100% (12/12), done.
remote: Total 37 (delta 10), reused 35 (delta 10), pack-reused 0
Unpacking objects: 100% (37/37), 3.52 KiB | 156.00 KiB/s, done.
From https://github.com/steadyin/string-handler
   2af1697..3308b37  main       -> origin/main
Updating 2af1697..3308b37
Fast-forward
 .../com/steadyin/parse/service/ParseService.java   |  5 ++--
 .../java/com/steadyin/parse/util/Arranger.java     | 27 +++++++++++++++++++
 src/main/java/com/steadyin/parse/util/Sorter.java  | 18 +++++++++++++
 .../java/com/steadyin/parse/util/ArrangerTest.java | 31 ++++++++++++++++++++++
 .../com/steadyin/parse/util/SeparatorTest.java     |  4 +--
 .../java/com/steadyin/parse/util/SorterTest.java   | 29 ++++++++++++++++++++
 6 files changed, 110 insertions(+), 4 deletions(-)
 create mode 100644 src/main/java/com/steadyin/parse/util/Arranger.java
 create mode 100644 src/main/java/com/steadyin/parse/util/Sorter.java
 create mode 100644 src/test/java/com/steadyin/parse/util/ArrangerTest.java
 create mode 100644 src/test/java/com/steadyin/parse/util/SorterTest.java
```
## 교차 출력

요구사항을 보고 추가로 Issue를 등록하겠습니다. 

![img_25.png](img_25.png)

git checkout -b feature/9
```java
PS C:\study\98.WORK\string-handler> git checkout -b feature/9
Switched to a new branch 'feature/9'
```

교차출력 기능을 가진 Interleaver 클래스를 생성합니다. 영어 문자열과 숫자 문자열이 들어왔을 때 교차로 출력해주는 역할을 담당합니다.

Interleaver
```java
@Component
public class Interleaver {
    public String interleave(final String str1, final String str2) {
        final StringBuilder sb = new StringBuilder();

        final Iterator<String> iterator1 = Arrays.stream(str1.split("")).iterator();
        final Iterator<String> iterator2 = Arrays.stream(str2.split("")).iterator();

        while (iterator1.hasNext() || iterator2.hasNext()) {
            if (iterator1.hasNext()) {
                sb.append(iterator1.next());
            }
            if (iterator2.hasNext()) {
                sb.append(iterator2.next());
            }
        }

        return sb.toString();
    }
}
```

InterleaverTest
```java
class InterleaverTest {

    private final Interleaver interleaver = new Interleaver();

    @DisplayName("문자열 두개가 주어질 때 하나의 교차하여 출력한다. 남은 것은 뒤에 붙인다.")
    @ParameterizedTest
    @CsvSource({
            "aaaaaaa, 1111, a1a1a1a1aaa",
            "AAbbbccDD, 3252, A3A2b5b2bccDD",
            "zzzAAfdsaf, 01239442789, z0z1z2A3A9f4d4s2a7f89"
    })
    void test(final String str1, final String str2, final String result) {
        assertThat(interleaver.interleave(str1, str2)).isEqualTo(result);
    }
}
```

ParseService
```java
@Service
@RequiredArgsConstructor
public class ParseService {

    private final UrlConnector urlConnector;

    private final Arranger arranger;

    private final Interleaver interleaver;

    public ParseResponse parse(ParseRequest request) {
        final String html = urlConnector.getHtml(request.getUrl());
        final String exposedHtml = request.getExposureType().getExposedHtml(html);
        final Arranger rearrange = arranger.rearrange(exposedHtml);
        final String interleaveText = interleaver.interleave(rearrange.getSortedEnglish(), rearrange.getSortedNumber());

        return null;
    }
}
```

교차출력 기능 메소드를 호출할 때 다음같은 형태로 호출하는 것이 가능합니다. 이것도 나쁘지는 않지만
```java
final String interleaveText = interleaver.interleave(rearrange.getSortedEnglish(), rearrange.getSortedNumber());
```
다음 형태가 나은 코드일 수도 있습니다.
```java
final String interleaveText = interleaver.interleave(rearrange);
```
Iterleaver 클래스에 다음 메소드를 오버로딩합니다.
```java
    public String interleave(final Arranger rearrange) {
        return interleave(rearrange.getSortedEnglish(), rearrange.getSortedNumber());
    }
```

interleave(final String str1, final String str2) 메서드는 외부에서 사용하지 않고 오직 테스트에서만 사용하게 되는데 같은 패키지내에서만 사용할 때는 default 접근제어자로 변경하는 것도 하나의 방법입니다.

그렇게하면 외부에서 호출할 때는 반드시 Rearrange 객체를 파라미터로 호출하는 것을 강요할 수 있겠죠.

git status / git add . / git commit -m "feat:Interleaver 추가" / git push
```
PS C:\study\98.WORK\string-handler> git status
On branch feature/9
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   src/main/java/com/steadyin/parse/service/ParseService.java

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        src/main/java/com/steadyin/parse/util/Interleaver.java
        src/test/java/com/steadyin/parse/util/InterleaverTest.java

no changes added to commit (use "git add" and/or "git commit -a")
PS C:\study\98.WORK\string-handler> git add .
PS C:\study\98.WORK\string-handler> git commit -m "feat:Interleaver 추가"
[feature/9 19165dc] feat:Interleaver 추가
 3 files changed, 59 insertions(+)
 create mode 100644 src/main/java/com/steadyin/parse/util/Interleaver.java
 create mode 100644 src/test/java/com/steadyin/parse/util/InterleaverTest.java
PS C:\study\98.WORK\string-handler> git push origin feature/9
Enumerating objects: 32, done.
Counting objects: 100% (32/32), done.
Delta compression using up to 8 threads
Compressing objects: 100% (11/11), done.
Writing objects: 100% (19/19), 2.18 KiB | 2.18 MiB/s, done.
Total 19 (delta 4), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (4/4), completed with 4 local objects.
remote:
remote: Create a pull request for 'feature/9' on GitHub by visiting:
remote:      https://github.com/steadyin/string-handler/pull/new/feature/9
remote:
To https://github.com/steadyin/string-handler.git
 * [new branch]      feature/9 -> feature/9
```

git checkout main / git pull
```
PS C:\study\98.WORK\string-handler> git checkout main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
PS C:\study\98.WORK\string-handler> git pull
remote: Enumerating objects: 32, done.
remote: Counting objects: 100% (32/32), done.
remote: Compressing objects: 100% (7/7), done.
remote: Total 19 (delta 4), reused 18 (delta 4), pack-reused 0
Unpacking objects: 100% (19/19), 2.19 KiB | 149.00 KiB/s, done.
From https://github.com/steadyin/string-handler
   3308b37..e6b5bce  main       -> origin/main
Updating 3308b37..e6b5bce
Fast-forward
 .../com/steadyin/parse/service/ParseService.java   |  5 ++++
 .../java/com/steadyin/parse/util/Interleaver.java  | 31 ++++++++++++++++++++++
 .../com/steadyin/parse/util/InterleaverTest.java   | 23 ++++++++++++++++
 3 files changed, 59 insertions(+)
 create mode 100644 src/main/java/com/steadyin/parse/util/Interleaver.java
 create mode 100644 src/test/java/com/steadyin/parse/util/InterleaverTest.java
```

## 출력 묶음 단위 추가

git checkout
```
PS C:\study\98.WORK\string-handler> git checkout -b feature/10
Switched to a new branch 'feature/10'
PS C:\study\98.WORK\string-handler>
PS C:\study\98.WORK\string-handler>
PS C:\study\98.WORK\string-handler>
```

출력 묶음 단위는 일정 단위 기준으로 문자열을 몫과 나머지로 나누는 기능입니다. 에를 들어 a0b1c2이 있을 때 단위는 최대 6까지 가능하고 3이면 몫은 a0b1c2 나머지는 없음 4이면 a0b1 나머지는 c2입니다.   

OutputUnit 클래스를 작성할 텐데 출력 묶음 단위 기능을 컴포넌트로 개발해야 할것인가에 대해서는 고민이 드는데 일반 클래스로 작성하겠습니다. 

몫과 나머지를 필드로 가지되 생성자로 문자열과 UnitCnt를 받습니다. 그리고 계산과정을 통해서 몫과 나머지에 저장이 되는 클래스로 설계를 하겠습니다.

```java
@Getter
public class OutputUnit {
    private final String quotient;
    private final String remainder;

    public OutputUnit(final String str, final Integer unitCount) {
        final int length = str.length();
        final int remainCount = length % unitCount;
        final int devideStandard = length - remainCount;

        this.quotient = str.substring(0, devideStandard);
        this.remainder = str.substring(devideStandard, length);
    }
}
```
OutputUnitTest
```java
class OutputUnitTest {
    public static final String SAMPLE_TEXT = "A0B0a0b0ccc";

    @DisplayName("문자열과 자연수 unitCount가 주어질 때 몫 문자열과 나머지 문자열 부분을 얻을 수 있다.")
    @Test
    void normal() {
        final OutputUnit outputSet = new OutputUnit(SAMPLE_TEXT, 3);

//        assertThat(outputSet.getQuotient()).isEqualTo("A0B0a0b0c");
//        assertThat(outputSet.getRemainder()).isEqualTo("cc");

        assertAll(
                () -> assertThat(outputSet.getQuotient()).isEqualTo("A0B0a0b0c"),
                () -> assertThat(outputSet.getRemainder()).isEqualTo("cc")
        );
    }

    @DisplayName("목이 딱 떨어지는 경우 나머지는 empty이다")
    @Test
    void remainderEmpty() {
        final OutputUnit outputSet = new OutputUnit(SAMPLE_TEXT, SAMPLE_TEXT.length());

        assertAll(
                () -> assertThat(outputSet.getQuotient()).isEqualTo(SAMPLE_TEXT),
                () -> assertThat(outputSet.getRemainder()).isEmpty()
        );
    }

    @DisplayName("출력묶음단위로 나눠지지 않으면 몪ㅅ은 empty이다")
    @Test
    void quotientEmpty() {
        final OutputUnit outputSet = new OutputUnit(SAMPLE_TEXT, SAMPLE_TEXT.length() + 1);

        assertAll(
                () -> assertThat(outputSet.getQuotient()).isEmpty(),
                () -> assertThat(outputSet.getRemainder()).isEqualTo(SAMPLE_TEXT)
        );
    }

    @DisplayName("targetString이 empty이면 몫과 나머지 모두 empty이다.")
    @Test
    void allEmpty() {
        final OutputUnit outputSet = new OutputUnit("", 1);

        assertAll(
                () -> assertThat(outputSet.getQuotient()).isEmpty(),
                () -> assertThat(outputSet.getRemainder()).isEmpty()
        );
    }

    @DisplayName("unitCount가 0이면 ArithmeticException이 발생한다")
    @Test
    void devideZero() {
        assertThatThrownBy(() -> new OutputUnit(SAMPLE_TEXT, 0))
                .isInstanceOf(ArithmeticException.class);
    }
}
```
이제 ParseResponse으로 응답을 하면 되는데 다른 Issue로 등록되어 있으므로 여기까지만 작성해서 커밋하겠습니다.

git add .
git commit -m "feat;추력 묶음 단위 추가"
git push origin feature/10
git checkout main
git pull

## ParseService 완성

ParseResponse
```java
@RequiredArgsConstructor
public class ParseResponse {
    private final String quotient;
    private final String remainder;

    public ParseResponse(OutputUnit outputUnit) {
        this.quotient = outputUnit.getQuotient();
        this.remainder = outputUnit.getRemainder();
    }
}
```

ParseService
```java
@Service
@RequiredArgsConstructor
public class ParseService {

    private final UrlConnector urlConnector;

    private final Arranger arranger;

    private final Interleaver interleaver;

    public ParseResponse parse(ParseRequest request) {
        final String html = urlConnector.getHtml(request.getUrl());
        final String exposedHtml = request.getExposureType().getExposedHtml(html);
        final Arranger rearrange = arranger.rearrange(exposedHtml);
        final String interleaveText = interleaver.interleave(rearrange);
        final OutputUnit outputUnit = new OutputUnit(interleaveText, request.getUnitCount());
        return new ParseResponse(outputUnit);
    }
}
```

git status
git add .
git commit -m "feat:ParseService완성 "
git push origin feature/11
git checkout main
git pull

## Swagger 적용

Swagger 적용해서 눈으로 보기 편하게 한다음에 API를 확인하도록 하겠습니다.

git checkout -b feature/12

스프링 2.6.4 랑 스웨거3.0이랑 안맞는 문제가 있습니다. 검색해보면 해결방법이 간단하게 있습니다. springdoc-openapi 공식문서를 살펴보면 Swagger에 대한 내용이 나와있습니다.

springdoc-openapi 라이브러리를 추가합니다.

build.gradle
```java
implementation 'org.springdoc:springdoc-openapi-ui:1.6.6'
```

Swagger 3.0부터 URL이 변경되었는데 http://localhost:8080/swagger-ui/index.html

![img_26.png](img_26.png)

Controller 혹은 Request Response에다가 설명을 추가할 수 있습니다. Swagger에 대한 애노테이션을 사용할 있습니다..

* @Operation
  * summary : API 요약
  * description : API 설명

```java
@RestController
@RequiredArgsConstructor
public class ParseController {
   private final ParseService parseService;

   @Operation(summary = "url data process", description = "URL 파싱후 HTML 데이터 가공")
   @PostMapping("api/parse")
   public ResponseEntity<ParseResponse> parse(@RequestBody @Valid ParseRequest request) {
      final ParseResponse response = parseService.parse(request);
      return ResponseEntity.ok(response);
   }
}
``` 

![img_27.png](img_27.png)

다음과 같이 Request의 스키마를 작성할 수 있습니다.
```java
@RequiredArgsConstructor
@Getter
public class ParseRequest {

    @Schema(description = "url", defaultValue = "https://www.naver.com")
    private final String url;

    @Schema(description = "노출유형", defaultValue = "REMOVE_HTML")
    private final ExposureType exposureType;

    @Schema(description = "unitCount", defaultValue = "10")
    private final Integer unitCount;
}
```

![img_29.png](img_29.png)

![img_30.png](img_30.png)

Response 항목도 출력되도록 Response에 @Getter를 적용하겠습니다. @Getter가 적용되지 않으면 멤버 필드가 스키마에 표시되지 않네요.

git status
git commit -m "feat:Swagger 설정"
git --amend ("setting:Swagger 설정"으로 변경)
git push origin feature/12
git checkout main
git pull

Swagger Test

![img_32.png](img_32.png)

![img_33.png](img_33.png)

정상 동작한다. 

![img_34.png](img_34.png)

![img_35.png](img_35.png)

그런데 unitCount를 0으로 입력하면 ArithmeticException가 발생하는 것을 확인할 수 있다. 

![img_36.png](img_36.png)

이제 예외 핸들러를 작성해보자.

## Global Exception Handler 추가

다음과 같이 @RestControllerAdvice를 통해 글로벌익센션핸들러를 작성할 수 있습니다.  

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(ArithmeticException.class)
    @ResponseStatus(value = HttpStatus.BAD_REQUEST)
    public void methodArgumentNotValidException(final ArithmeticException e) {
        System.out.println(e.getMessage());
    }
}
```

실행해서 다시 ArithmeticException를 발생시키면 이제 로그창에 더이상 예외 관련 로그는 보이지 않고 메시지만 출력됩니다. 그리고 Response도 차이가 있습니다.

전
![img_38.png](img_38.png)
![img_39.png](img_39.png)

후
![img_37.png](img_37.png)
![img_40.png](img_40.png)

그런데 ArithmeticException를 처리하는 것보다는 맨 처음 세팅할 때 spring-boot-starter-validation을 추가했으므로 Controller에서 @Valid 애노테이션을 사용해서 검증을 추가할 수 있습니다. 그래서 @Valid에 대한 에러메시지를 출력해주는 것이 더 깔끔할것 같습니다. 0으로 나눌 때 발생하는 예외보다는 Request 정의 할 때 해당 되는 패턴으로 입력되지 않았을 때 메시지를 직접 정의줄 수 있으므로 그 에러에 대한 익셉션을 처리하는게 조금 더 나은 방법일 것 같습니다.

@Pattern(regexp ="regexp", message = "meesage")
* @Pattern을 사용해서 url 필드에 정규식을 이용해서 해당 정규식이 아니면 에러 메시지를 정의할 수 있습니다. 

@Min(value = 1, message = "message")
* @Min을 사용해서 최소값을 지정할 수 있습니다.

그리고 예외 핸들러에서 MethodArgumentNotValidException로 예외를 처리할 수 있습니다.

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(MethodArgumentNotValidException.class)
    @ResponseStatus(value = HttpStatus.BAD_REQUEST)
    public void methodArgumentNotValidException(final MethodArgumentNotValidException e) {
//        log.error(e.getMessage());
        System.out.println(e.getMessage());
//        return ResponseEntity.badRequest().body(new ResponseError(e.get))
    }
}
```
![img_41.png](img_41.png)

정의한 메시지가 포함되어 있는 것을 확인할 수 있습니다. 그래서 잘 필터링 해서 응답하면 프론트 담당자가 어떤 작업을 하다가 에러가 났을 때 왜 잘못됬는지, 뭐가 잘못됬는지 파악하기 수월합니다.

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(MethodArgumentNotValidException.class)
    @ResponseStatus(value = HttpStatus.BAD_REQUEST)
    public ResponseEntity<ErrorResponse> methodArgumentNotValidException(final 
        return ResponseEntity.badRequest()
                .body(new ErrorResponse(e.getBindingResult().getAllErrors().get(0).getDefaultMessage(), "E001"));
    }
}
```

![img_42.png](img_42.png)

깔끔하게 에러코드와 에러메시지가 응답되는 것을 확인할 수 있습니다.

사실 서버에서 로그를 모아놓고 어떤 에러가 발생했을 때 알람이 되도록 운영을 하는데 Lombok을 사용한다는 가정하에 다음과 같이 로그를 사용할 수 있습니다.

## 로그

@Slf4j를 클래스에 적용하면 로그를 사용할 수 있습니다.

```java
@RestControllerAdvice
@Slf4j
public class GlobalExceptionHandler {
    @ExceptionHandler(MethodArgumentNotValidException.class)
    @ResponseStatus(value = HttpStatus.BAD_REQUEST)
    public ResponseEntity<ErrorResponse> methodArgumentNotValidException(final MethodArgumentNotValidException e) {
        log.error(e.getMessage());
        return ResponseEntity.badRequest()
                .body(new ErrorResponse(e.getBindingResult().getAllErrors().get(0).getDefaultMessage(), "E001"));
    }
}
```

만약 접속되지 않는 URL을 입력하면 어떻게 될까요 ? 

![img_43.png](img_43.png)

IllegalArgumentException이 발생합니다. 해당 예외를 처리하는 익셉션 핸들러는 존재하지 않습니다. 그래서 GlobalExceptionHandler에서 IllegalArgumentException을 처리하려고 고려하니 IllegalArgumentException을이 너무 범용적인 예외라는 문제가 있습니다. 

그래서 IllegalArgumentException을 커스텀 예외로 전환해보도록 하겠습니다.  

UrlConnectionException로 정의하겠습니다. 근데 속하는 것은 IllegalArgumentException에 속해야겠죠. IllegalArgumentException를 상속합니다. 상위 Exception의 생성자를 타도록 작성합니다.   

```java
    @ExceptionHandler(UrlConnectionException.class)
    @ResponseStatus(value = HttpStatus.BAD_REQUEST)
    public ResponseEntity<ErrorResponse> urlConnectionException(final UrlConnectionException e) {
        log.error(e.getMessage());
        return ResponseEntity.badRequest()
                .body(new ErrorResponse(e.getMessage(), "E002"));
    }
```

ExposureType도 다른게 들어 올수도 있겠죠. 

![img_45.png](img_45.png)

HttpMessageNotReadableException이 발생합니다. 

```java
    @ExceptionHandler(HttpMessageNotReadableException.class)
    @ResponseStatus(value = HttpStatus.BAD_REQUEST)
    public ResponseEntity<ErrorResponse> httpMessageNotReadableException(final UrlConnectionException e) {
        log.error(e.getMessage());
        return ResponseEntity.badRequest()
                .body(new ErrorResponse(e.getMessage(), "E003"));
    }
```

![img_46.png](img_46.png)

# README

README는 오픈소스라던가, 라이브러리라던가 많은 정리해놓은 글들이 많다. 참고해서 작성하자.

```
# string-handler

## 실행방법

```
git clone https://github.com/Jonny-Cho/data-processor.git
./gradlew bootRun
or
./gradlew build
java -jar ./build/libs/dataprocessor-0.0.1-SNAPSHOT.jar
```

## Swagger
http://localhost:8080/swagger-ui/index.html

## 개발환경
* Java8
* Spring Boot 2.6.4
* Junit5
* Jsoup
* Swagger

## 요구사항

1. URL 입력을 하면 그 링크 안에있는 모든 HTML코드를 불러온다.
2. 노출유형
  * 노출 유형이 HTML 일 경우는 태그를 제거한다
    * `<div>abc</div> -> abc`
  * 노출 유형이 TEXT 일 경우에는 모든 텍스트 포함
    * `<div>abc</div> -> <div>abc</div>`
3. 영어, 숫자만 출력
  * 결과 데이터는 영어와 숫자로만 구성된 데이터만 출력한다
4. 오름차순 정렬
  * 영어 : AaBbCcDd ... Zz
  * 숫자 : 0123456789
5. 교차출력
  * 영어숫자영어숫자
  * 더이상 출력될 숫자 또는 영어가 없을경우 남아있는 문자열 그대로 출력
6. 출력묶음단위
  * 사용자가 입력한 자연수의 묶음 단위를 기준으로 몫과 나머지를 구하여 노출
  * 예) ‘A0a1B2b3’, 출력 묶음 단위 3인경우
    * 몫 ‘A0a1B2’
    * 나머지 ‘b3’

## 고려사항

static component클래스 장단점, 왜 component를 사용했는지 대해서나.

globalexception을 작성했다.

swagger을 작성했따.

선택에 대한 이유라든가 구현시 어려웠던 점 주의했던점을 추가로 작성하면 된다.

README.md가 처음 보여지는 화면인데 열심히 성실히 작성했다 이런것을 보여주면 좋을 것 같습니다.

```

[개발가락/4년차] [오후 9:01] 아까 질문나왔던 static vs component를 좀 찾아봤습니다.

우선 static vs singleton (@Component 어노테이션의 기본 빈 스코프)
https://www.baeldung.com/java-static-class-vs-singleton

싱글톤

- 애플리케이션을 위한 완전한 객체 지향 솔루션 필요
- 주어진 시간에 클래스의 한 인스턴스만 필요하고 상태를 유지하기 위해
- 필요할 때만 로드되도록 클래스에 대해 느리게 로드된 솔루션을 원함

정적 클래스

- 입력 매개변수에서만 작동하고 내부 상태를 수정하지 않는 많은 정적 유틸리티 메소드를 저장하기만 하면 됩니다.
- 런타임 다형성 또는 객체 지향 솔루션이 필요하지 않습니다.

—
@component는 스프링 빈으로 등록해주는 어노테이션인데, 기본 스코프가 싱글톤이라 자주 쓰입니다.
DI를 할때 객체 생성을 직접하지 않고도 빈을 주입받을 수 있어서 작업이 편하다는 장점이 있어요.


[개발가락/4년차] [오후 9:05] https://stackoverflow.com/questions/7026507/why-are-static-variables-considered-evil

요 논의도 참고하기 좋은 것 같습니다!

https://stackoverflow.com/questions/7026507/why-are-static-variables-considered-evil
