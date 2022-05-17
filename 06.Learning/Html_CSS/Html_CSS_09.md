```css
/* 선택자(selector) */
div#header-center {
    margin: 0 auto;
    width: 1080px;
}
```

PX, 픽셀은 상대적인 길이입니다. 이 픽셀은 상대적인 개념이라서 모니터의 픽셀수를 알아야 합니다. 모니터의 가로 픽셀이 1920이라고 했을 때 1920등분 한 것 중에 1080을 차지한다라는 의미입니다. 여러분의 모니터 해상도에 따라서 이 픽셀이 달라지기 때문에 상대적인 개념이라고 이해하시면 됩니다.

* 크롬 개발자 도구 Console -> screen.width 가로 해상도

![image](https://user-images.githubusercontent.com/79847020/168804744-ec062557-62bb-46fa-8519-27ff0e094ea9.png)

* 크롬 개발자 도구 Console -> screen.height 세로 해상도

![image](https://user-images.githubusercontent.com/79847020/168804962-c327314b-60ca-4563-a512-b40407ec858f.png)

네이버의 \<header\> 영역은 상단 메뉴와 검색창으로 나눌 수 있습니다. 

```html 
...
<header>
    <div id="header-center"> <!-- 가운데 정렬용 div -->
        <!-- 속성명1: 값1;속성명2: 속성값2;-->
        <!-- 인라인 스타일 -->
        <div>
            네이버를 시작페이지로
        </div>
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
    </div>
</header>
...
```

![image](https://user-images.githubusercontent.com/79847020/168805782-02270189-9b81-4136-b466-5a4dceccd56d.png)

![image](https://user-images.githubusercontent.com/79847020/168805833-cd7963b5-b421-496b-ac2e-7ca93189d584.png)

네이버 대표 이미지와 네이버 검색창을 한 라인에 있습니다. \<div\>를 구분하기 위해 id속성을 넣어주겠습니다. id말고 class속성이 있는데 나중에 설명드리겠습니다.

검색창 div에 "header-search" id명을 지정해줍니다. 그리고 그 자식태그 \<h1\>에 css를 적용하기 위해 id를 추가해주어도 되지만 \<h1\>태그에 CSS를 적용할수도 있습니다.

```
...
        <h1> 형제(Sibling) </h1>
        <div id="header-search">
            <h1>
                <a href="https://www.naver.com">네이버</a>
                <!-- 주석(comment)입니다 -->s
                <!-- 속성(attribute)입니다 -->
            </h1>
            <h2>검색창</h2>
            <fieldset>
                <legend>검색</legend>
                <input/>
                <button>검색</button>
            </fieldset>
        </div>
...
```     

자식 태그에 CSS를 적용하기 위해서는 다음과 같이 작성합니다.

```css
div#header-search > h1 {
    width: 280px;
}
```

참고로 형제 태그를 시블링(Sibling)이라고 하는데 형제 태그 \<h1\>에는 당연히 적용되지 않습니다. 다시 말하지만 div#header-search는 생략해서 #header-search로 작성해도 무방합니다. 

자식태그, 자손태그 또 그 이하를 자손태그라고 합니다. 자식태그와 자손태그의 CSS 적용법은 차이가 있습니다.

자식 선택자
```css
div#header-search > h1 {
...
}
```

자손 선택자
```css
div#header-search h1 {
...
}
```

보통 자식 선택자보다는 자손 선택을 활용하게 됩니다. 

\<h2\>검색창\</h2\>는 화면에 보이지 않는데 스크린리더 전용입니다. 시각장애인이 검색창인 것을 인지할수도록 함입니다. 하지만 보여지면 안되는 애들입니다. 따라서 CSS를 적용합니다.

```css
div#header-search h2 {
    display: none
}
```

CSS를 작성할 때 중요한 것은 선택자와 속성을 외우셔야 됩니다. 외우는 방법 밖에 없습니다. 

* 오류 : display: none은 스크린 리더도 못 읽습니다. 다음 시간에 다른 방법을 사용하겠습니다.

검색창은 520 px입니다. 

```css
div#header-search fieldset {
    width: 520px
}
```

![image](https://user-images.githubusercontent.com/79847020/168809481-337251eb-7deb-4b51-bb9b-992fb0f0efcc.png)

여기서 의문이 들 수 있는게 header-center 영역이 1080px이고 \<h2\>는 0px 네이버 대표 이미지(\<h1\>)가 280px 검색 입력 창이 520px인데 나란히 한줄로 보여지지 않습니다.

![image](https://user-images.githubusercontent.com/79847020/168810089-bbbc6eb0-ae0f-4cf1-8835-dbf3fc25c06e.png)

\<div\>에 display: block CSS가 적용되어 있는데 기본적으로 전체 너비를 차지하게 됩니다. 280 px로 수정해도 공간은 전체를 다 차지하고 있습니다. 

![image](https://user-images.githubusercontent.com/79847020/168810484-16867f08-f51f-4d81-a776-43eaa5bb40a9.png)

실제 컨텐츠의 너비를 조정되었지만 차지하는 공간(주황색 영역)은 한 라인을 다 차지하고 있습니다. 이 주항색 부분을 경계, 여백 또는 Margin이라고 부릅니다. 저부분에는 다른게 들어갈 수 없습니다. display:block는 항상 경계를 갖고 있기 때문에 나란히 보여질 수 없습니다. 적용되어 있는 display:block을 취소하겠습니다.

```CSS
div#header-search > h1 {
    width: 280px;
    display: inline-block;
}
```

![image](https://user-images.githubusercontent.com/79847020/168811024-bcc5d03d-0ee7-4ce2-91fa-84c7525dab13.png)

```CSS
div#header-search fieldset {
    width: 520px;
    display: inline-block;
}
```

다음과 같이 같은 가로 라인에 보여지게 됩니다.

![image](https://user-images.githubusercontent.com/79847020/168811194-1774e475-972d-4376-b7f3-2f93b4068632.png)

그렇다면 margin 영역에는 컨텐츠를 올리지 못할까요 ? 강제적으로 올리는 방법이 있습니다.

컨텐츠를 한 라인에 보여주기 위해서는 \<div\>의 너비를 조절하고 기본속성을 display:block; 에서 display: inline-block; 으로 수정하면 된다는 것을 알아두시면 좋습니다. 

![image](https://user-images.githubusercontent.com/79847020/168820422-d9b791cd-0264-42ac-a51e-044a9cce5f70.png)

우측 상단에 메뉴를 그려보겠습니다.

```html
        <div>
            <div style="display:inline-block;">네이버를 시작페이지로</div>
            <div style="display:inline-block;">쥬니어네이버</div>
            <div style="display:inline-block;">해피빈</div>
        </div>
```

사실 이렇게 텍스트인 경우에는 \<div\>보다는 \<span\>을 활용합니다.

```html
        <div>
          <span>네이버를 시작페이지로</span>
          <span>쥬니어네이버</span>
          <span>해피빈</span>
        </div>
```
다만 \<span\>은 차이가 있다면 display 기본속성이 inline입니다. display 속성값으로는 inline, block, inline-block이 있습니다. 

* display:block은 너비 100%를 차지합니다.
* display:inline은 컨텐츠의 너비만 차지합니다. 
* display:inline-block은 width속성을 지정할 수 있는데 height속성을 지정할 수 있습니다. block이 크기를 지정할 수 있다는 것이 inline과의 차이점입니다.

  






