# 7-3. 실시간 랭킹 hover로 보여주기

그 다음 정리할게 실시간 검색어 부분입니다. 마우스 포커스를 얻게 되면 더 큰 영역이 보이게 됩니다. Z-index 입니다. 기본적으로 위에서 아래로, 왼쪽에서 오른쪽으로 배치되므로 영역이 겹치는 상황이 만들어지지 않습니다. 그런데 위에서 아래, 왼쪽에서 오른쪽 같은 일반적인 배치에서 일반적인 흐름에서 벗어날 수 있는 것이 position이었습니다. position: relative; position: absolute; position; fixed; 를 하면 보통에서 배치에서 벗어나고 그러다보면 기존 영역을 침범해서 겹치는 상황이 만들어지기도 합니다. 그래서 겹쳤을 때 누가 더 상위에 위치하는지를 판단할 수 있는 기준이 있어야 하는데 그것이 Z인덱스 입니다. 

Z-Index 수치가 높다고 무조건 높은 것이 아니라 규칙이 존재합니다. 일단 상위에 위치할 영역을 먼저 구현하겠습니다.

![image](https://user-images.githubusercontent.com/79847020/185783214-0756da02-1235-4393-b8cc-4350e1b1fad5.png)


CSS에 hover라는 것이 있습니다. Naver는 hover를 사용하지 않고 javascript를 사용했는데 저희는 CSS로 구현하는 하기 위해 hover를 사용하겠습니다.

```HTML
    <nav>
        <div class="center-align relative">
            <div id="search-ranking">
                <h2 class="blind">실시간 검색어</h2>
                <span id="rank-number">1</span>
                <span id="rank-title">제로초</span>
                <ul>
                    <li> 
                        <span class="rank-number">1</span>
                        <span class="rank-title">제로초</span>
                    </li>
                    <li>
                        <span class="rank-number">2</span>
                        <span class="rank-title">제로초</span>
                    </li>
                    <li>
                        <span class="rank-number">3</span>
                        <span class="rank-title">제로초</span>
                    </li>
                    <li>
                        <span class="rank-number">4</span>
                        <span class="rank-title">제로초</span>
                    </li>
                    <li>
                        <span class="rank-number">5</span>
                        <span class="rank-title">제로초</span>
                    </li>
                    <li>
                        <span class="rank-number">6</span>
                        <span class="rank-title">제로초</span>
                    </li>
                </ul>
            </div>
	...		
```		

id와 class는 이름이 같아도 됩니다. id, class는 다른 개념이기 때문에 다르게 인식합니다. 

```CSS#search-ranking {
    position: absolute;

    top: 14px;
    right: 0px;
    width: 198px;
    height: 20px;
}

#search-ranking ul {
    display: inline-block;
    list-style: none;
    padding: 0;
    margin: 0;
}
```

UL에 기본 CSS를 제거하고 display: inline-block;을 적용합니다. 

![image](https://user-images.githubusercontent.com/79847020/185798154-afeb5fe1-aeb8-4651-95d4-9a0663d5ee5f.png)

선택자를 잘 못 설계하면 의도치 않게 CSS가 적용됩니다. 선택자를 nav li:nth-child(3) span:first-child 으로 적용해두었습니다. 해당 선택자는 \<nav\>의 메뉴 영역을 지정하기 위해서 사용했습니다. 그런데 \<ul\>을 영역안에서 사용했떠니 의도치 않게 \<ul\>에도 CSS가 적용되고 있습니다.

![image](https://user-images.githubusercontent.com/79847020/185798252-3ea1c25f-0674-43fb-86c5-31331d1a179d.png)

이럴 때는 모호한 선택자를 우선순위를 높여주면 되겠죠. 그래서 메뉴의 \<ul\>태그에 id를 추가하겠습니다. 

```HTML
            <ul id="nav-menu"><!--unordered list--><!-- ol 태그도 있어요 ordered list -->
                <li>
                    <a href="">
                        <span class="nav-item-mail"></span>
                        <span class="blind">메일</span>
                    </a>
                </li><!--list item-->
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
```

```CSS
#nav-menu li:first-child span:first-child {
    width: 25px;
    /*height: 16px;*/
    background-position: 0 -285px;
    /*background-image: url(./sp_nav.png);*/
    /*background-repeat: no-repeat;*/
    /*display: inline-block;*/
    margin-left: 0;
}

#nav-menu li span:first-child {
    background-image: url(./sp_nav.png);
    background-repeat: no-repeat;
    display: inline-block;
    height: 16px;
    margin-left: 10px;
}

#nav-menu li:nth-child(2) span:first-child {
    width: 27px;
    /*height: 16px;*/
    background-position: -279px -52px;
    /*background-image: url(./sp_nav.png);*/
    /*background-repeat: no-repeat;*/
    /*display: inline-block;*/
}

#nav-menu li:nth-child(3) span:first-child {
    width: 40px;
    /*height: 16px;*/
    background-position: -100px -182px;
    /*background-image: url(./sp_nav.png);*/
    /*background-repeat: no-repeat;*/
    /*display: inline-block;*/
}

#nav-menu li:nth-child(4) span:first-child {
    width: 40px;
    background-position: -101px -156px;
}

#nav-menu li:nth-child(5) span:first-child {
    width: 26px;
    background-position: -279px -156px;
}

#nav-menu li:nth-child(6) span:first-child {
    width: 25px;
    background-position: -70px -285px;
}

#nav-menu li:nth-child(7) span:first-child {
    width: 35px;
    background-position: -125px -130px;
}

#nav-menu li:nth-child(8) span:first-child {
    width: 26px;
    background-position: -279px -208px;
}

#nav-menu li:nth-child(9) span:first-child {
    width: 26px;
    background-position: -128px -104px;
}

#nav-menu li:nth-child(10) span:first-child {
    width: 26px;
    background-position: -36px -259px;
}

#nav-menu li:nth-child(11) span:first-child {
    width: 39px;
    background-position: -151px -156px;
}

#nav-menu li:nth-child(12) span:first-child {
    width: 26px;
    background-position: -279px -130px;
}

#nav-menu li:nth-child(13) span:first-child {
    width: 26px;
    background-position: -234px -233px;
}

#nav-menu li:nth-child(14) span:first-child {
    width: 26px;
    background-position: -72px -259px;
}

#nav-menu li:nth-child(15) span:first-child {
    width: 13px;
    background-position: -140px -78px;
}

#nav-menu li:nth-child(16) span:first-child {
    width: 26px;
    background-position: -279px 0;
}
```

nav가 동메달이었는데 id는 금메달이 되고 nav-menu 안에 있는 \<li\>만 해당되기 때문에 겹칠일이 줄어들게되죠. 

다음과 같은 nav li도 겹칠일이 없어도록 수정해야 합니다.


![image](https://user-images.githubusercontent.com/79847020/186420941-34bc6585-e306-48a1-89a0-c7f5b16e017e.png)

```CSS
#nav-menu li {
    display: inline-block;
}
```

검색어 랭킹이 노출되는 검색어와 랭킹 전체가 나란히 있을 필요는 없으므로 \<div\>으로 구분해줍니다.

![image](https://user-images.githubusercontent.com/79847020/186421329-9c849fec-d6ee-4f29-a082-c27746399ae2.png)

```HTML
        <div class="center-align relative">
            <div id="search-ranking">
                <h2 class="blind">실시간 검색어</h2>
                <div>
                    <span id="rank-number">1</span>
                    <span id="rank-title">제로초</span>
                </div>
                <div>
                    <ul>
                        <li>
                            <span class="rank-number">1</span>
                            <span class="rank-title">제로초</span>
                        </li>
                        <li>
                            <span class="rank-number">2</span>
                            <span class="rank-title">제로초</span>
                        </li>
                        <li>
                            <span class="rank-number">3</span>
                            <span class="rank-title">제로초</span>
                        </li>
                        <li>
                            <span class="rank-number">4</span>
                            <span class="rank-title">제로초</span>
                        </li>
                        <li>
                            <span class="rank-number">5</span>
                            <span class="rank-title">제로초</span>
                        </li>
                        <li>
                            <span class="rank-number">6</span>
                            <span class="rank-title">제로초</span>
                        </li>
                    </ul>
                </div>
            </div>
```

![image](https://user-images.githubusercontent.com/79847020/186421670-4d325f67-d260-40a3-933a-e10a13a336cd.png)

목록들을 노출되는 랭킹에 마우스를 올렸을 때 보여져야 합니다. 

```CSS

#search-ranking ul {
    display: inline-block;
    list-style: none;
    padding: 17px
    margin: 0;

    width: 332px;
    height: 334px;
    border: 1px solid #aaa;
    background-color: white;
}
```

![image](https://user-images.githubusercontent.com/79847020/186422271-4868f036-7cd7-42cb-aa95-fe3f1b859501.png)

search-ranking 영역의 위치를 조절하겠습니다.

이것이 가능한 이유가 \<ul\> containing block을 찾아야되는데 왜냐하면 position: absolute;면 containing block이 달라지기 때문입니다. 부모인 search-ranking이 position: absolute;라서 \<ul\>의 containg block이 될 수 있고 search-ranking을 기준으로 top, right을 적용합니다.

평소에는 보이지 않다가 search-ranking에 마우스를 올렸을 때 보여야죠. 그러면 평소에는 display: none;을 해주고 손을 올렸을 때 보이게 해야합니다.

```CSS
#search-ranking ul {
    display: inline-block;
    list-style: none;
    padding: 17px;
    margin: 0;

    width: 332px;
    height: 334px;
    border: 1px solid #aaa;
    background-color: white;
    position: absolute;
    top: -15px;
    right: 0;
	
	display: none;
}
```

hover가 있습니다. hover는 sudo class, 의사 클래스라고 하는데 first-child처럼 은메달 역할을 합니다. hover는 마우스를 올렸을 때 CSS를 바꿔줄 수 있습니다. 

여기서는 마우스를 올렸을 때 search-ranking의 CSS를 수정하는 것이 아니라 search-ranking 안에 ul을 바꿔주는 것입니다.

```CSS
#search-ranking:hover ul {
    display: inline-block;
}
```
![image](https://user-images.githubusercontent.com/79847020/186428774-6f9378e7-9791-4885-af8d-894d5e40f453.png)




