다시 네이버에서 들어가서 document.head.parentNode.removeChild(document.head); JavaScript 명령어를 적용합니다. 명령어를 적용하면 \<head\> 부분이 사라지는 것을 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/79847020/165555093-6b268f79-3028-4567-8a19-99765ed4c813.png)

![image](https://user-images.githubusercontent.com/79847020/165555169-5c95a6a2-45ef-496e-af31-295b840522e5.png)

\<head\> 태그가 사라지면 CSS가 사라지는 것을 알 수 있습니다. 그럼 CSS가 \<head\> 태그안에 포함되어 있을 것이라는 것을 추측해볼 수 있습니다.

이제 컨텐츠를 만들어보겠습니다. 네이버 화면에서 가장 중요한 것을 생각해봅시다. 네이버임을 보여주는 NAVER이라는 회사명, 사이트명이 가장 중요합니다. 

그런애들을 HTML Headings라고 말하면 \<h\> 태그를 사용합니다. 중요도별로 숫자를 붙여 표현합니다.

```
<!doctype html>
<html>
    <head>
        <meta charset="utf-8">
        <title>네이버</title>
        <link rel="shortcut icon" href="./favicon.ico" type="image/x-icon"/>
    </head>
    <body>
        <h1>네이버</h1>
        <h2>검색창</h2>
        <h2>실시간 검색어</h2>
        <h3>로그인</h3>
        <h3>뉴스</h3>
        <h3>법률</h3>
        <h3>쇼핑</h3>
    </body>
</html>
```

Heading 태그로 현재 페이지의 주요 키워드를 중요한 순으로 표현할 수 있습니다. 

크롬의 확장도구를 설치합니다.

![image](https://user-images.githubusercontent.com/79847020/166475943-bd90af6b-f421-4c6e-9ab7-2bc8e0ba770a.png)

HeadingsMap으로 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/79847020/166476106-a96d8ace-7eff-4831-bc26-c86646cec4bd.png)

\<h\>를 잘 작성하면 검색엔진에서 해당 웹 페이지의 키워드로 사용합니다.

인텔리J는 Ctrl+D로 라인을 복사할 수 있습니다. 이런것때문에 IDE를 사용하고 속도가 달라집니다. 속도는 실력으로 평가받기 때문에 잘 알아두셔야 합니다.







