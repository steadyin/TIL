![image](https://user-images.githubusercontent.com/79847020/183595793-d61bfcf8-2819-4249-b10c-d0abc18366fb.png)

검색창과 메뉴에 공백이 크게 존재합니다. 

<header>의 하이트를 170으로 설정합니다.

```CSS
header {
    height: 170px;
}
```

![image](https://user-images.githubusercontent.com/79847020/183597687-8cf18c11-0d82-4dc0-a60d-41119a04fef2.png)

네이버를 시작페이지로와 검색창 사이에도 공간이 있습니다.

![image](https://user-images.githubusercontent.com/79847020/183598511-e9a8adfe-8b68-4d16-bd42-966c580d672f.png)

header-top, header-search 사이에 공간을 띄우면 되는데 현재는 margin collapsing(마진 상쇄 현상) 때문에 적용되지 않습니다. 다음시간에 언급하겠습니다.

![image](https://user-images.githubusercontent.com/79847020/183599808-dbc18759-3bd4-47cb-a197-97255cc74951.png)

margin도 공간이고 padding도 공간인데, 예를 들어 fieldset을 검색창으로 삼는다고 치면 검색창의 밖은 margin이 되는 것입니다. 바깥공간은 margin이라고 생각을 해야되고 검색창 테두리 안의 여유공간은 padding이 되는 것입니다. 테두리가 있으면 구별하기 쉬운데 테두리가 없는 경우에는 margin으로 할지 padding으로 할지 애매합니다. 둘다해도 크게 문제는 없는데 안전한 것은 padding이 안전합니다. padding은 특히 \<a\> 태그인 경우에 padding공간도 클릭하면 동작하게 됩니다. padding을 지정해서 클릭할 수 있는 공간을 확보해주는게 일반적으로 좋습니다. margin을 위험하다고 하는 이유는 margin-collapsing이라고 해서 상하에 마진을 둬서 마진이 겹치는 경우에 없어지는 현상이 있습니다. 그 현상을 제대로 인식하지 않고 margin을 사용하면 margin을 지정했는데 적용되지 않는 현상이 발생합니다. 

```
#header-search {
    clear: both;
    padding-top: 20px;
}
```

nav영역에 다음과 같이 적용합니다.

```CSS
nav {
    height: 46px;
    border-top: 1px solid #f1f3f6;
}
```

참고로 solid 외에도 dashed, dotted도 있는데 대부분 solid를 사용합니다.

![image](https://user-images.githubusercontent.com/79847020/183616356-ec529931-4fd1-4eb2-b48c-c01e306bf7fb.png)

구분선이 하나 들어가있는데 네이버의 경우에는 CSS를 활용해서 표현했습니다.

![image](https://user-images.githubusercontent.com/79847020/183616589-6db9170b-110f-4b57-ae34-cd2baae59bce.png)

width:1px; height:14px; background: #ebeef3; 인 \<div>

* HTML Emmet

자동완성 기능으로 HTML을 작성할 때 사용할 수 있는 플러그인입니다.

WebStorm에서 nav-divider를 입력하고 TAB을 누르면 다음과 같이 자동완성됩니다.

```HTML
<nav>
    <div class="center-align">
        <ul><!--unordered list-->
            <li><a href=""><span></span><span class="blind">메일</span></a></li><!--list item-->
            <li><a href=""><span></span><span class="blind">카페</span></a></li><!--list item-->
            <li><a href=""><span></span><span class="blind">블로그</span></a></li><!--list item-->
            <li><a href=""><span></span><span class="blind">지식인</span></a></li><!--list item-->
            <li><a href=""><span></span><span class="blind">네이버쇼핑</span></a></li><!--list item-->
            <li><a href=""><span></span><span class="blind">네이버페이</span></a></li><!--list item-->
            <li><a href=""><span></span><span class="blind">네이버TV</span></a></li> <!-- list item -->
            <li><a href=""><span></span><span class="blind">사전</span></a></li> <!-- list item -->
            <li><a href=""><span></span><span class="blind">뉴스</span></a></li> <!-- list item -->
            <li><a href=""><span></span><span class="blind">증권</span></a></li> <!-- list item -->
            <li><a href=""><span></span><span class="blind">부동산</span></a></li> <!-- list item -->
            <li><a href=""><span></span><span class="blind">지도</span></a></li> <!-- list item -->
            <li><a href=""><span></span><span class="blind">영화</span></a></li> <!-- list item -->
            <li><a href=""><span></span><span class="blind">뮤직</span></a></li> <!-- list item -->
            <li><a href=""><span></span><span class="blind">책</span></a></li> <!-- list item -->
            <li><a href=""><span></span><span class="blind">웹툰</span></a></li> <!-- list item -->
        </ul>
        <div class="nav-divider"></div>
    </div>
</nav>
```

```CSS
.nav-divider {
    float: left;
    margin: 18px 16px 0;
    width: 1px;
    height: 14px;
    background: #ebeef3;
}
```
다음과 같이 아래쪽에 위치하는데 

![image](https://user-images.githubusercontent.com/79847020/183621964-48d32e82-b10d-4b13-a5b8-e34d6e8e9ca2.png)

\<ul\>태그에 display: inline-block;을 적용합니다. 

```CSS
nav ul {
    margin: 0;
    padding: 16px 0 0 4px;
    list-style: none;
    display: inline-block;
}
```

inline-block을 적용하면 공간은 ul만 차지하는데 float: left;에 의해 왼쪽에 위치하게 됩니다.

![image](https://user-images.githubusercontent.com/79847020/183622422-e499119e-a7a5-4a4a-a955-3896a46dc480.png)

float: left;를 하면 기존에 있떤 것들을 무시하고 바로 왼쪽으로 붙습니다. 그럴때는 감싸고 있는 부모태그에도 float: left;를 적용합니다.

![image](https://user-images.githubusercontent.com/79847020/184305886-0c5475d2-93f7-43f3-ad31-8bc741f7f157.png)

float: left; 와 float: left; 가 연속해서 있다면 순서대로 렌더링되고 display: inline-block;와 float: left; 가 연속해서 있다면 float: left; 가 렌더링 됩니다.

그래서 혼동이 오기 때문에 float을 사용하지 않겠다고 하면 둘 다 display: inline-block;만 사용하시면 됩니다.

모두 display: inline-block; 적용

![image](https://user-images.githubusercontent.com/79847020/184307100-cee82e0f-980d-4188-9cab-c4b73f4d108d.png)

저자는 float은 혼동을 가져오기 때문에 사용을 지양하는 편입니다. 

## 실시간 검색어 

실시간 검색어 부분을 추가합니다. (강의 누락 부분 추정)

```HTML
<nav>
    <div class="center-align relative">
        <div id="search-ranking">
            <h2 class="blind">실시간 검색어</h2>
            <ul>
                <li>제로초</li>
            </ul>
        </div>
...
```

```CSS
#search-ranking {
    position: absolute;
    top: 0;
    right: 0;
}

.relative {
    position: relative;
}
```

실시간 검색어 항목을 작성합니다.

```HTML
....
<nav>
    <div class="center-align relative">
        <div id="search-ranking">
            <h2 class="blind">실시간 검색어</h2>
            <span id="rank-number">1</span>
            <span>제로초</span>
        </div>
...
```

```CSS
#rank-number {
    margin-top: -9px;
    color: #00ab33;
    font-size: 16px;
}		
```

![image](https://user-images.githubusercontent.com/79847020/184313283-78a146f9-355e-4868-b435-817a64e569a5.png)

```CSS
#search-ranking {
    position: absolute;
    top: 16px;
    right: 100px;
}
```

![image](https://user-images.githubusercontent.com/79847020/184313880-37b76157-b9ed-4dad-81e2-5869500ac16f.png)


```CSS
nav {
    height: 46px;
    border-top: 1px solid #f1f3f6;
    border-bottom: 1px solid #d1d8e4;
}
```

![image](https://user-images.githubusercontent.com/79847020/184314055-595ab531-ca64-4762-928c-d7b0774986a2.png)

미세하게 다른 부분들을 조정해줍니다.

```CSS
div#header-search h1 {
    width: 198px;
    height: 48px;
    display: inline-block;
    /*background-image: url(sp_search.png);*/
    /*background-position: -4px -10px;*/
    /*background-repeat: no-repeat;*/
    background: url(./sp_search.png) -4px -4px no-repeat;
    vertical-align: middle;
    position: relative;
    top: -5px;
	left: 1px;
}

#header-top {
    float: right;
    margin-top: 8px;
}

#header-search {
    clear: both;
    padding-top: 12px;
}

#search-image {
    background-image: url(./sp_search.png);
    background-position: -3px -60px;
    background-repeat: no-repeat;
    width: 21px;
    height: 21px;
    display: inline-block;
    margin: 14px;
}



```

개발자모드에서 Computed 탭에서 폰트를 확인할 수 있습니다. 

![image](https://user-images.githubusercontent.com/79847020/184316309-ddca1026-25b5-402d-9282-167010b5a330.png)

문서 자체의 폰트가 돋움으로 설정되어 있습니다. font-family를 활용합니다. 폰트 여러개를 예비로 넣었습니다. 순차적으로 폰트를 적용후 폰트가 없으면 다음 폰트가 적용되는 식으로 동작합니다.

```CSS
html, body {
    margin: 0;
    padding: 0;
    font-family: Dotum, '돋움', Helvetica, "Apple SD Gothic Neo", sans-serif;
}
```

참고로 serif와 sans-serif 폰트는 모든 컴퓨터에 다 설치되어있다고 생각하시면 딥니다.


