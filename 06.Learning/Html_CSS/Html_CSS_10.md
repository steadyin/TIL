이번 시간에는 박스 모델과 플르토, 포지션에 대해서 알아보겠습니다. 지금까지의 소스 입니다.

```html
<!doctype html>
<html>
<head>
    <meta charset="utf-8">
    <title>네이버</title>
    <link rel="shortcut icon" type="image/x-icon" href="./favicon.ico" />
    <link rel="stylesheet" href="./naver.css"/>
</head>
<body>
<header>
    <div id="header-center"> <!-- 가운데 정렬용 div -->
        <!-- 속성명1: 값1;속성명2: 속성값2;-->
        <!-- 인라인 스타일 -->
        <div>
            <!--<div style="display:inline-block;">네이버를 시작페이지로</div>-->
            <!--<div style="display:inline-block;">쥬니어네이버</div>-->
            <!--<div style="display:inline-block;">해피빈</div>-->
            <span>네이버를 시작페이지로</span>
            <span>쥬니어네이버</span>
            <span>해피빈</span>
        </div>
        <div id="header-search">
            <h1>
                <a href="https://www.naver.com">네이버</a>
                <!-- 주석(comment)입니다 -->
                <!-- 속성(attribute)입니다 -->
            </h1>
            <h2>검색창</h2>
            <fieldset>
                <legend>검색</legend>
                <input/>
                <button>검색</button>
            </fieldset>
        </div>
    </div>
</header>
<nav>
    <ul><!--unordered list-->
        <li>메일</li><!--list item-->
        <li>카페</li><!--list item-->
        <li>블로그</li><!--list item-->
        <li>지식인</li><!--list item-->
        <li>쇼핑</li><!--list item-->
        <li>네이버페이</li><!--list item-->
        <li>네이버쇼핑</li><!--list item-->
    </ul>
</nav>
<main>
    <h2>실시간 검색어</h2>
    <h3>연합뉴스</h3>
    <ol>
        <li>충격) 테스트 지금 목아파..</li>
        <li>속보) 방송 종료</li>
        <li>중대발표) 내일 출근</li>
    </ol>
    <h3>언론사 목록</h3>
    <ul>
        <li><img src="./한국일보.png" alt="한국일보"></li>
        <li><img src="./전자신문.png" alt="전자신문"></li>
    </ul>
    <h3>로그인</h3>
    <h3>뉴스</h3>
    <h3>법률</h3>
    <h3>쇼핑</h3>
</main>
<footer>
    <div>공지사항</div>
    <div>Creators</div>
    <div>회사소개</div>
</footer>
</body>
</html>
```

```CSS
/* 선택자(selector) */
div#header-center {
    margin: 0 auto;
    width: 1080px;
}

div#header-search > h1 {
    width: 280px;
    display: inline-block;
}

/*
div#header-search h1 {
    자손 선택자
}
*/

div#header-search h2 {
    display: none
}

div#header-search fieldset {
    width: 520px;
    display: inline-block;
}
```

저번 시간에 display:none에 대해서 설명을 했는데 정정할게 있습니다. 사람 눈에는 보이지 않지만 스크린 리더에 읽힌다고 설명을 드렸었는데 시각장애인이 보는 스크린 리더에도 읽히지 않습니다. \<h2\>가 있는 이유는 HTML이 문서의 구조를 잡는 역할을 수행하기 때문에 문서에서 두번째로 중요한 컨텐츠라는 구조를 잡기 위해서 사용되었습니다.

눈에 보이지는 않지만 검색 엔진이 \<h2\>태그를 읽고 컨텐츠의 키워드를 인식하게 됩니다.

그래서 일반 사람 눈에는 보이지 않지만 스크린 리더에서 인식되게 하는 방법을 알아야 합니다. 네이버의 소스를 살펴보면 다음과 같이 되어있습니다.

![image](https://user-images.githubusercontent.com/79847020/168823501-dc9665f9-1927-4a9a-9466-64a2d0623bb4.png)

blind CSS가 적용되어 있는 것을 알 수 있습니다. 

```CSS
.blind {
    position: absolute;
    clip: rect(0 0 0 0);
    width: 1px;
    height: 1px;
    margin: -1px;
    overflow: hidden;
}
```
눈에 보이지는 않지만 Display:None은 아닙니다. 그래서 스크린리더에는 읽힙니다. 이런 것을 웹 접근성을 말합니다. 

웹 접근성을 고려하고 있으면 이 blind CSS를 활용하면 됩니다. CSS에서 .으로 지정되있으면 class속성을 의미합니다. 다음과 같이 CSS를 적용할 수 있습니다.

```html
<h2 class="blind">~~</h2>
```

\<div\>가 빈번히 사용되기 때문에 구분하기 위해서 id 속성을 활용한다고 했습니다. class도 구별하기 위해 사용되지만 id는 고유한 값을 지정할 때 사용하고 class는 속성을 지정할 때 사용됩니다. 같은 class 속성을 여러 곳에서 사용할 수 있다고 생각하면 됩니다. 참고로 하나의 태그에 여러 class를 적용할 수 있습니다. 

여러 class를 지정할 때는 뛰어쓰기로 구분합니다.
```html
<h2 class="blind newStyle"></h2>
```

id 속성 값은 중복될 수 없는 고유한 속성 값이다. class 속성 값은 중복되서 사용할 수 있꼬 여러 class를 뛰어쓰기로 지정할 수 있다. 참고로 id를 중복 사용한다고 해서 에러가 나는 것은 아닙니다. 하지만 유효하지 않은 html이기 때문에 안하는 것입니다. html은 워낙 관대해서 예를 들어 닫는 태그가 없어도 에러가 발생하지 않는데 그만큼 스스로 룰을 정해서 엄격하게 작성할 필요가 있습니다. 

```
...
<header>
    <div id="header-center"> <!-- 가운데 정렬용 div -->
        <!-- 속성명1: 값1;속성명2: 속성값2;-->
        <!-- 인라인 스타일 -->
        <div>
            <!--<div style="display:inline-block;">네이버를 시작페이지로</div>-->
            <!--<div style="display:inline-block;">쥬니어네이버</div>-->
            <!--<div style="display:inline-block;">해피빈</div>-->
            <span>네이버를 시작페이지로</span>
            <span>쥬니어네이버</span>
            <span>해피빈</span>
        </div>
        <div id="header-search">
            <h1>
                <a href="https://www.naver.com" class="blind">네이버</a>
                <!-- 주석(comment)입니다 -->
                <!-- 속성(attribute)입니다 -->
            </h1>
            <h2>검색창</h2>
            <fieldset>
                <legend class="blind">검색</legend>
                <input/>
                <button class="blind">검색</button>
            </fieldset>
        </div>
    </div>
</header>
...
```

![image](https://user-images.githubusercontent.com/79847020/168825679-4833abff-87c5-4ec0-b6af-a819c4a121fc.png)

네이버 이미지를 다운로드 합니다.

![sp_search](https://user-images.githubusercontent.com/79847020/168826201-6a02c52d-6ecb-4089-919b-788cbe7ed724.png)

이미지를 별도의 이미지로 분리해놓은 것이 아니라 하나로 합쳐놓았는데 이것을 이미지 스프라이트라고 부릅니다. 요즘에는 사실 http2라는 프로토콜이 나오면서 네트워크 환경이 개선되었다고 보시면 됩니다. 옛날에는 네트워크 환경이 안 좋을 때는 네이버 서버에서 이미지를 가져왔습니다. 그런데 다른 곳에서 가져오는 행위를 동시에 여러개를 할 수 없습니다. 동시에 가져오면 매우 느리게 로딩이 되었는데 요즘에는 http2가 나오면서 그런 부담이 줄어들었습니다. 

그래도 http2를 지원하지 않는 환경, 브라우저도 있기 때문에 이런식으로 관련 있는 이미지를 이미지 스프라이트로 묶어서 한번에 요청합니다. 만약 이미지 8개가 하나의 이미지 스프라이트로 되어 있다면 8번 요청해야 할 것을 1번 요청으로 처리하게 되는 것입니다. 

![image](https://user-images.githubusercontent.com/79847020/168827162-10d8dbf8-972b-41bb-b1c3-5fcfa9c4bb42.png)

프론트엔드, html/css개발을 할 때는 코딩하는 것만 집중하는데 디테일하게 들어가면 로딩이 너무 오래걸린다. 로딩 시간을 어떻게 줄여야할까 같은 고민들이 이루어져야 합니다. 요즘에는 필요성이 많이 줄어들었지만 이미지 스프라이트라는 기법을 통해서 처리를 개선했다는 것을 알아두시면 됩니다.  



 













