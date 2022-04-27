Html은 \<head\>에 들어갈 태그가 \<body\>에 들어가도 동작하고 \<body\>에 들어갈 태그가 \<head\>에 들어가도 동작합니다. 엄격하지 않은 편이기 때문에. 하지만 표준에 맞게 작성하는게 좋습니다.

\<head\>에는 만약 웹 사이트의 인코딩, 웹 사이트의 설명, 웹 사이트의 제목, Favicon(페비콘)과 같은 웹사이트의 부가정보를 포함합니다. 

![image](https://user-images.githubusercontent.com/79847020/165549242-af5d30d2-f9da-4106-a447-1aef52d82d27.png)

\<title\>은 웹사이트의 제목, \<meta\>는 웹사이트의 정보를 표현하는 태그입니다. 보통은 \<meta\>의 charset속성을 지정하는데 한글이 깨지기않기 위한 인코딩 정보입니다. 

```html
<!doctype html>
<html>
    <head>
        <meta charset="utf-8">
        <title>네이버</title>
    </head>
    <body>

    </body>
</html>
```

\<meta\> 또한 닫는 \</meta\>태그가 있어야 하는 것이 원칙인데 아래와 같이 표현해도 동작합니다.

```html
<meta charset="utf-8"></meta>

<meta charset="utf-8"/>

<meta charset="utf-8">
```

React나 Vue를 할 때 닫는 태그는 중요하기 때문에 항상 닫는 태그를 붙이는 습관을 기르시는것을 추천합니다. 

favicon을 추가할 수 있습니다.

```html
<link rel="shortcut icon" href="/favicon.ico" type="image/x-icon"/>
```

rel="shortcut icon", type="image/x-icon"은 favicon이라는 것을 알려주는 것이고 href가 hypertext reference의 약어입니다. \/로 시작하는 경우 현재 페이지의 절대경로를 의미합니다. 

경로에 관한 것은 나중에 다시 정리하겠습니다.

크롬의 개발자도구에서 Sources 탭을 가면 각종 웹페이지 관련 파일들이 나열되어있습니다. 

https://www.naver.com/favicon.ico에서 favicon.ico를 받을 수 있습니다. 

하지만 반영되지 않습니다. 다음과 같이 경로를 수정합니다.

```html
<link rel="shortcut icon" href="./favicon.ico" type="image/x-icon"/>
```

다시 파일을 열면 favicon이 적용된 것을 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/79847020/165553478-578a4e28-3739-4891-b8b3-86228db3a5ee.png)



















