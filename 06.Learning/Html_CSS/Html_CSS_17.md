
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

가운데 정렬이 되었습니다.

![image](https://user-images.githubusercontent.com/79847020/178507175-2166541f-99b2-470e-a97c-be427dc15550.png)

UL태그에도 margin과 왼쪽 padding이 적용되어 있습니다. 

![image](https://user-images.githubusercontent.com/79847020/178507579-14afac5d-37f7-4df5-8e7d-5e1b9b850e7b.png)

그런데\<li\>가 한줄을 다 차지하고 있습니다. \<ul\>에 margin, padding을 0을 주고 list-style로 항목 모양을 지정할 수 있습니다.

```
margin: 0;
padding: 0;
list-style: none;
```

list-style은 none을 통해서 없애줄 수도 있고 disc, square 등과 같이 모양을 지정해줄수도 있습니다.

![image](https://user-images.githubusercontent.com/79847020/178518207-da758f52-732f-4292-9cc1-8bcb0c98092e.png)

그리고 \<li\>를 display: inline-block으로 지정해줍니다. 옆으로 메뉴들이 나열되도록 합니다.

```CSS
nav ul {
    margin: 0;
    padding: 0;
    list-style: none;
}

nav li {
    display: inline-block;
}
```

![image](https://user-images.githubusercontent.com/79847020/179348491-e4a54af9-418c-4069-9b2d-950ceb1e6492.png)

참고로 inline-block 말고도 float left 으로 대체할 수도 있습니다.

이미지를 네이버에서 다운로드 했습니다.
  
![image](https://user-images.githubusercontent.com/79847020/179348960-ba2d9631-ee04-45c4-8968-7b0255f8785c.png)

앞서 배운 이미지 스프라이트를 활용해야합니다. 

이미지 스프라이트에서 백그라운드 이미지로 넣어주고 백그라운드 포지션을 잡아주면 되겠죠.

위치를 잡기 위해서는 어쩔 수없이 노가다를 좀 해야합니다.

네이버 태그를 살펴보면 리스트 \<li\>태그 안에 링크 태그를 들어가있습니다. 그리고 \<span\> 태그를 넣어두고 백그라운드 이미지를 넣어주게됩니다.

![image](https://user-images.githubusercontent.com/79847020/179349028-7bc68ee7-d0c9-4fcf-b6a9-2f525603d986.png)

이미지를 넣어줄 때는 항상 이미지를 설명하는 글을 넣어주어야 시각장애인에게 메일 링크인 것을 알려줄 수 있죠.

```html
<li><a href=""><span></span><span class="blind">메일</span></a></li><!--list item-->
```

이제 이미지를 넣어주어야 하는데 span에다가 id="nav-mail-image" 로 id를 지정해주어서 CSS에다가 각각 넣어주던지 아니면 선택자를 통해서 선택을 하던지 선택해야 합니다.

여기서는 선택자를 배우도록 하겠습니다. 

구조가 다음과 같이 되어있습니다.

```html
<div>
  <ul>
    <li>
      <a href="">
        <span></span>
        <span class="blind">메일</span>
      </a>
    </li><!--list item-->
  </ul>
</div>
```

\<span\> 2개 중에 앞선 \<span\>에 적용을 해야하는데 선택자에 특정한 순서, 첫번째나 두번째과 같이 특정한 순서에 위치에 있는 태그를 선택할수 있습니다.

다음과 같이 작성하면 내부 안에 들어있는 리스트를 선택하는데 그 리스트 중에서 첫번째 자식인 것, UL의 첫번째 자식인 first-child를 다음과 같이 지정할 수 있습니다.

콜론을 하고 뒤에 first-child를 적어주시면 됩니다. 그리고 안에 span, span 중에서도 first-child에 CSS를 적용하겠습니다.

```CSS
nav li:first-child span:first-child {
    width: 25px;
    height: 16px;
    background-position: 0 -285px;
    background-image: url(./ch2/sp_nav.png);
    background-repeat: no-repeat;
}

```

그러면 첫번째 자식 \<li\>안에서 첫번째 자식 \<span\>을 선택하는 것입니다. span이기 때문에 display: inline-block를 지정합니다.

![image](https://user-images.githubusercontent.com/79847020/179351697-8c27206d-8fe1-4180-a41d-8458726801b8.png)

이미지 스프라이트인 경우 항상 이미지 포지션을 잘 적용해주셔야 합니다.

\<li\>에 이어서 적용해주어야 하는데 2번째 자식 태그는 second-child가 아니라 nth-child 입니다. 첫번째만 first-child로 적용하고 그 이후로는 nth-child 그리고 마지막에는 last-child로 선택할 수 있습니다.

```CSS
nav li:nth-child(2) span:first-child {
    width: 27px;
    height: 16px;
    background-position: -279px -52px;
    background-image: url(./sp_nav.png);
    background-repeat: no-repeat;
    display: inline-block;
}
```

![image](https://user-images.githubusercontent.com/79847020/179351937-48e639e2-5324-49f4-8e47-56ee028cc2b6.png)

이미지 스프라이트를 적용하기가 까다로운데 트레이드 오프라고 생각하시면 됩니다.

브라우저 성능을 향상시킬 수 있지만 개발자는 조금 귀찮게 합니다.

CSS를 보시면 공통적인 것이 있는 것을 볼 수 있습니다. 

공통적인 속성이 3개가 겹칠 때부터는 어느정도 겹친다고 생각하고 분리하는 것이 좋습니다.

3번째 child를 작성합니다.

```CSS
nav li:first-child span:first-child {
    width: 25px;
    /*height: 16px;*/
    background-position: 0 -285px;
    /*background-image: url(./sp_nav.png);*/
    /*background-repeat: no-repeat;*/
    /*display: inline-block;*/
}

nav li:nth-child(2) span:first-child {
    width: 27px;
    /*height: 16px;*/
    background-position: -279px -52px;
    /*background-image: url(./sp_nav.png);*/
    /*background-repeat: no-repeat;*/
    /*display: inline-block;*/
}

nav li:nth-child(3) span:first-child {
    width: 40px;
    /*height: 16px;*/
    background-position: -100px -182px;
    /*background-image: url(./sp_nav.png);*/
    /*background-repeat: no-repeat;*/
    /*display: inline-block;*/
}
```

![image](https://user-images.githubusercontent.com/79847020/179521389-187f0d55-2627-42ba-9e4e-25190839f6d8.png)


공통적인 것을 뽑아봅시다. 셋의 공통점은 nav li span 임을 인식합니다.

```CSS
nav li {
    display: inline-block;
}

nav li span {
    background-image: url(./sp_nav.png);
    background-repeat: no-repeat;
    display: inline-block;
    height: 16px;
}
```

![image](https://user-images.githubusercontent.com/79847020/179521376-a0f14725-b232-46bf-bf1c-b85da55e188f.png)

이어서 작성을 하면 다음과 같이 메뉴를 완성시킬 수 있습니다.

![image](https://user-images.githubusercontent.com/79847020/179523008-553e1644-989b-4ed9-9634-3bac9ec125be.png)

사실 이 예제는 nth-child를 알려드리기 위해서 억지로 끼워넣은 것이고 실무에서는 사용하지 않는 것을 권장드립니다.

지금 당장 처음 배우시는 분들은 구현하기 급급해서 nth-child를 사용할수도 있는데 유지보수적인 측면에서 배우 안좋습니다.

왜냐하면 순서가 달라질수도 있고 정확히 무엇을 위한 CSS인지 인식하기도 힘들기 때문입니다. 

그래서 nth-child를 사용하기보다는 클래스 이름을 하나씩 줘서 매칭하는 것이 더 낫다고 생각됩니다.
