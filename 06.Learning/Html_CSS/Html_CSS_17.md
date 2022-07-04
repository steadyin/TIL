
이제 메뉴 부분을 만들어보도록 하겠습니다.

![image](https://user-images.githubusercontent.com/79847020/177178813-51bd7f9d-750e-455c-8188-96226263d55f.png)

![image](https://user-images.githubusercontent.com/79847020/177179470-d8eb9977-6015-4158-a663-601a0cfe6df2.png)

\<nav\> 안에서 \<div\>로 감싸고 가운데 정렬을 합니다. id는 가운데 정렬을 위한 center-align으로 지정합니다.

```HTML
<nav>
  <div id="center-align">
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
</nav>
```

그런데 header-center CSS를 보면 너비가 1080px 이므로 재활용할 수 있을 것으로 보입니다.

```
#header-center {
    margin: 0 auto;
    width: 1080px;
}
```

여러번 사용되는 범용적인 CSS는 Class로 바꿔줍니다.

```
.center-align {
    margin: 0 auto;
    width: 1080px;
}
```

```HTML

...
<header>
    <div class="center-align">
    <!--<div id="header-center">--> <!-- 가운데 정렬용 div -->
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
...생략
</header>
<nav>
    <div class="center-align">
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
</nav>
```











