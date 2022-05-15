HTML 코드가 잘 작성되어있어야 하지 CSS는 부가적인 요소라는 것을 아셔야 합니다. 세상에는 장애인들도 있고 이 분들은 CSS에 의존하고 있지 않기 때문에 HTML만으로 웹 사이트를 이해하고 있기 때문에 HTML을 제대로 만들 의무가 있습니다.

레이아웃 구조 잡는 것을 알아보도록 하겠습니다. 레이아웃은 웹 페이지의 배치라고 생각하면 됩니다. 네이버를 살펴보면 검색창, 컨테츠들이 있고 맨 상단에는 header, 마지막 하단에는 footer가 존재합니다. 

배치를 CSS를 우겨 넣어서 억지로 맞추는 분들이 있는데 유지보수가 힘들고 반응형이라고 모바일에서부터 화면사이즈를 넓히면 자동으로 배치를 변경해주는데, 그런 경우에 배치를 잘 못 잡았다면 고통을 받을 수 있으니 레이아웃 잡는것을 중요시 해야합니다. 먼저, html 구조 먼저 잘 잡아놓고 그 다음에 레이아웃 잡는 것 나머지 디자인적 요소는 뒤로 미루어 작업하셔도 좋습니다.

레이아웃 구조 잡는 것을 가장 쉽게하는 방법이 무얼까 생각해보았는데 가로선을 생각해보면 좋습니다. 웹 페이지에 구분선으로 가로선이 있습니다. 구역을 가로로 먼저 나누고 그 안에서 세로로 나눕니다. 항상 먼저 가로로 나누고 나서 가로로 나눈 구역안에서도 세부적으로 구분이 필요하다면 가로로 나누고 더 이상 가로로 나눌 필요가 없다면 세로로 나눕니다. 그 이유는 코드를 작성하면서 살펴보겠습니다.

![image](https://user-images.githubusercontent.com/79847020/168458503-08b24371-d2b3-40fb-a581-a902b8d1587c.png)

네이버이 컨텐츠 부분을 살펴보겠습니다. 가로로 먼저 나눈다고 생각하면 아래와 같이 그을 수 있겠지만 오른쪽 로그인 부분과 환율 정보가 나오는 레이아웃에서 가로선이 잘리기 때문에 이 부분에서 가로선을 긋기에는 애매합니다. 
 
![image](https://user-images.githubusercontent.com/79847020/168458800-6922dff2-0445-4511-8fb7-6e6a41f33c0e.png)

그럴 때는 다음으로 넘어갑니다. 다음가 같이 구역을 가로로 레이아웃을 나누기에 합당한 곳들이 있습니다.

![image](https://user-images.githubusercontent.com/79847020/168458868-f9c101e9-4589-4a49-ad8e-c0e28527ac77.png)

![image](https://user-images.githubusercontent.com/79847020/168458916-b9431859-3631-4c02-b467-06d8ecee6289.png)

그럼 콘텐츠 부분에서 3개의 가로 구역으로 나눌 수 있습니다. 그 다음 세로로 레이아웃선을 그어봅니다.

![image](https://user-images.githubusercontent.com/79847020/168458995-ffe52330-2e51-4cda-94fd-e34da29620fe.png)

그리고 구분된 뉴스/언론사 영역에서 다시 가로 구역을 생각해봅시다.

![image](https://user-images.githubusercontent.com/79847020/168459025-fd8d2541-a769-4332-9a0a-4753d35a86d8.png)

더 이상 가로로 나눌 수 없는 영역에서 다시 세로로 영역을 고민해봅니다.

![image](https://user-images.githubusercontent.com/79847020/168459117-2537743b-d92c-42e8-855e-5d659e641074.png)

다른 부분도 이런 단계를 반복해서 레이아웃을 나눌 수 있습니다.

네이버 소스를 살펴보면 \<div\>태그가 많이 보이는데 \<div\>가 구역을 나눈다고 생각하면 됩니다. division의 약자입니다.

![image](https://user-images.githubusercontent.com/79847020/168459637-2e976553-aea5-4c99-9c89-19c2625453d7.png)

가로 6개의 구역을 6개의 \<div\> 태그로 만들겠습니다.

```html
<!doctype html>
<html>
<head>
    <meta charset="utf-8">
    <title>네이버</title>
    <link rel="shortcut icon" href="favicon.ico" type="image/x-icon"/>
</head>
<body>
<div>
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
<div>
    <ul><!--unordered list-->
        <li>메일</li><!--list item-->
        <li>카페</li><!--list item-->
        <li>블로그</li><!--list item-->
        <li>지식인</li><!--list item-->
        <li>쇼핑</li><!--list item-->
        <li>네이버페이</li><!--list item-->
        <li>네이버쇼핑</li><!--list item-->
    </ul>
</div>
<div>
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
</div>
<div>공지사항</div>
<div>Creators</div>
<div>회사소개</div>
</body>
</html>
```

다음과 같이 크롬 개발자모드에서 구역을 깔끔하게 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/79847020/168459870-90662ef5-fc56-4a25-a973-aa550c20b1eb.png)

\<div\>태그의 특징은 가로줄 전체를 차지합니다. \<div\>는 한 페이지의 넓이 100%를 차지하고 있습니다. 

한 페이지에 빈번하게 사용되는 \<div\>를 구분하기 위해 id 속성을 활용했습니다.

```html
<div id="header">....</div>
<div id="nav">....</div>
<div id="main">....</div>
<div id="footer">....</div>
```

그런데 html5가 도입되면서 자주 사용되는 \<div id=""\> 태그를 대체할 수 있는 태그들이 추가되었습니다. 

\<header\>,\<nav\>,\<main\>,\<footer\>,\<article\>,\<aside\>, \<figure\>

\<div\>태그에 자주사용되서 이름이 붙은 태그라고 생각하면 됩니다.

지금보니 공지사항, Creators, 회사소개는 footer역할로 봐도 무방해서 하나의 영역으로 묶어주겠습니다.

```html
<!doctype html>
<html>
<head>
    <meta charset="utf-8">
    <title>네이버</title>
    <link rel="shortcut icon" href="favicon.ico" type="image/x-icon"/>
</head>
<body>
<header>
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

이런 것들이 무슨 의미가 있냐. 아직까지 검색엔진들이 멍청해서 header, main, nav, footer든 잘 못 알아차리는데 앞으로는 점점 더 똑똑해질 것이기 때문에 이런 시멘틱 태그(Sementic Tag)들이 중요성이 올라갈것입니다. 그래서 \<div\>태그를 남용하기 보다는 시멘틱 태그를 사용하는 것을 추천드립니다. 

시멘틱 태그는 의미가 있기 때문에 제한이 있는 경우가 있습니다. main은 주 내용이기 때문에 웹 페이지에서 1번만 사용해야 한다. 그런 제약들은 html 작성하면서 개발자가 신경쓰기 힘들기 때문에 Validity 라는 크롬 확장 프로그램을 사용하는 것이 좋습니다. 





















