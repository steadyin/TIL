![image](https://user-images.githubusercontent.com/79847020/174434357-0a80adff-c7b7-4e8a-8187-6e8c70c1d347.png)

네이버에 접속해서 네이버 로고 부분은 이렇게 되어있습니다. \<span\>태그만 있고 이미지 주소라든가 그런것들을 없습니다. 

CSS에 background-image가 있습니다. 

![image](https://user-images.githubusercontent.com/79847020/174434367-ee8b1b94-fd31-41d6-8bbd-68d8d512f28d.png)

그리고 적용되어 있는 css에서 height와 width는 198px, 48px입니다. 아니 일단 width만 지정해보겠습니다.

```CSS
...
div#header-search > h1 {
    width: 198px;
    height: 48px
    display: inline-block;
    background-image: url(./sp_search.png);
}
...
```

naver.html에서 blind를 제외합니다. 

```html
...
        <div id="header-search">
            <h1>
                <a href="https://www.naver.com">네이버</a>
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
...
```

다음과 같이 백그라운드 이미지로 들어가고 NAVER 이미지가 조금 잘렸는데 그래서 height 속성을 지정해주어야 합니다.

![image](https://user-images.githubusercontent.com/79847020/174434906-f25036b7-b626-4bb4-8a2e-e4785a8180a7.png)

\<a\>에 높이를 주면 되죠.

```CSS
...
div#header-search > h1 {
    width: 198px;
    height: 48px
    display: inline-block;
    background-image: url(./sp_search.png);
}
...
```

![image](https://user-images.githubusercontent.com/79847020/174435017-8a3250b3-2f3a-49c7-977f-0d0951f4d9f9.png)


아직도 Naver 이미지가 잘 적용되지 않았습니다. 

background-image에는 background-position 속성이 있습니다.

```css
...
div#header-search > h1 {
    width: 198px;
    height: 48px;
    display: inline-block;
    background-image: url(./sp_search.png);
    background-position: -4px -10px;
}
...
```

![image](https://user-images.githubusercontent.com/79847020/174435216-b5c58bef-998e-48ae-9183-13dd8aa6a8bc.png)

네이버에서는 네이버 글자를 보여주지 않기 위해서 text-indent를 사용했습니다. display:none을 사용하지 않냐면 스크린 리더에 읽히지 않기 때문입니다.
온갖 꼼수를 사용하는 것입니다. blind도 그렇고 스크린리더에 안읽히는 현상을 해결하기 위해 여러 꼼수를 사용합니다.

text-indent는 글자 들여쓰기 입니다.

```CSS
...
div#header-search > h1 {
    width: 198px;
    height: 48px;
    display: inline-block;
    background-image: url(./sp_search.png);
    background-position: -4px -10px;
    text-indent: 50%;
}
...
```

![image](https://user-images.githubusercontent.com/79847020/174437710-6db94887-46fd-4bc1-8d22-200caab1c81a.png)

text-indent: 100%; 하면 이미지 끝에 붙어있기 때문에 h1에게 주어진 공간 밖으로 나갑니다. 

![image](https://user-images.githubusercontent.com/79847020/174437725-c0a16fc3-19db-4b23-ac13-5f134f02569d.png)

## overflow: hidden;

밖으로 나갈 때 보여주지 않게 하기 위해서는 overflow: hidden을 CSS를 사용합니다.

```CSS
...
div#header-search > h1 {
    width: 198px;
    height: 48px;
    display: inline-block;
    background-image: url(./sp_search.png);
    background-position: -4px -10px;
    text-indent: 100%;
    overflow: hidden;
}
...
```

![image](https://user-images.githubusercontent.com/79847020/174437815-82211b21-73c6-4caf-8c89-6c33a533ab0f.png)

사실 이런 꼼수들은 기본기를 익힌다음 익히면 좋은데 꼼수를 사용하지 않는다면 다음과 같이 할 수 있습니다.

네이버 텍스트에 span 태그를 적용하고 어쩔 수 없이 display: none을 줍니다.

```HTML
...
  <a href="https://www.naver.com">
      <span>네이버</span>
  </a>
...
```

```CSS
...
#header-search > h1 span {
    display: none;
}
...
```

h1 > span 중간에 a가 있기 때문에 자식선택자를 사용하면 안됩니다. 자손선택자를 사용합니다.


![image](https://user-images.githubusercontent.com/79847020/174437949-df107d72-31cc-46d1-a3a8-816ca254d6d3.png)

CSS에서 text-indent: 100%;와 overflow: hidden;를 제거합니다.

```CSS
...
div#header-search > h1 {
    width: 198px;
    height: 48px;
    display: inline-block;
    background-image: url(./sp_search.png);
    background-position: -4px -10px;
}
...
```

## background-repeat: no-repeat;

만약에 height를 너무 많이준 경우 다음과 같이 보여집니다. height: 108px;를 주었을 때.

![image](https://user-images.githubusercontent.com/79847020/174438297-acf3c5bf-01da-4319-82d1-c0f7794ace91.png)

그래서 background=image를 적용할 때는 width와 height를 잘 사용해야합니다. 특히 이미지 스프라이트를 할 경우에는 더욱더.

디폴트값은 background-repeat: repeat; 으로 적용되어 있습니다. 그래서 background의 공간보다 h1이나 div의 공간이 넓다면 공간을 채우기 위해서 반복됩니다.

```CSS
...
div#header-search > h1 {
    width: 198px;
    height: 108px;
    display: inline-block;
    background-image: url(./sp_search.png);
    background-position: -4px -10px;
    background-repeat: no-repeat;
}
...
```

![image](https://user-images.githubusercontent.com/79847020/174438501-cc92fca1-c8f1-4cb0-bd77-9dda4c978bd6.png)

참고로 background CSS를 함축할 수 있습니다

```CSS
...
div#header-search > h1 {
    width: 198px;
    height: 48px;
    display: inline-block;
    background: url(./sp_search.png) -4px -10px no-repeat;
}
...
```

이렇게 한방에 줄일 수 있습니다. height도 다시 48px로 적용합니다.

![image](https://user-images.githubusercontent.com/79847020/174438744-5576bff4-ea14-40d7-958f-f5aaf7eb5d05.png)

## \<a\> 태그 수정

\<a\> 태그안에 \<span\>이 들어있는데 \<span\>이 지금 display:none입니다. 화면에 없어요. 그럼 \<span\>을 감싸고 있는 \<a\>도 0x0 입니다. 

![image](https://user-images.githubusercontent.com/79847020/174439270-0c94aa28-cceb-4990-affc-c92f9ed13fff.png)

\<a\>의 공간이 0x0이라는 것은 동작하지 않는 \<a\>라는 것을 의미합니다. 네이버로 이동하는 기능이 동작하지 않습니다. 그래서 \<a\>를 사용할 때는 공간이 할당되어 있는지 확인을 해주어야 합니다.

간단한 해결법은 \<a\>를 \<h1\> 밖으로 빼주면 됩니다.

```HTML
...
        <div id="header-search">
            <a href="https://www.naver.com">
                <h1>
                    <span>네이버</span>
                    <!-- 주석(comment)입니다 -->
                    <!-- 속성(attribute)입니다 -->
                </h1>
            </a>
            <h2>검색창</h2>
            <fieldset>
                <legend class="blind">검색</legend>
                <input/>
                <button class="blind">검색</button>
            </fieldset>
        </div>
...
```

CSS를 맞게 바꿉니다. 자식식별자가 아니라 자손식별자를 사용합니다.

```CSS
div#header-search h1 {
    width: 198px;
    height: 48px;
    display: inline-block;
    /*background-image: url(sp_search.png);*/
    /*background-position: -4px -10px;*/
    /*background-repeat: no-repeat;*/
    background: url(./sp_search.png) -4px -10px no-repeat;
}

#header-search h1 span {
    display: none;
}
```

![image](https://user-images.githubusercontent.com/79847020/174439929-7945ef83-9477-4590-91ac-718bb65c5206.png)

\<h1\> 태그를 \<a\> 태그로 감싸고 있으니까 자식 태그를 누르면 네이버로 이동할 수가 있습니다.

```html
  <a href="https://www.naver.com">
      <h1>
          <span>네이버</span>
          <!-- 주석(comment)입니다 -->
          <!-- 속성(attribute)입니다 -->
      </h1>
  </a>
```

\<a\>태그가 \<h1\>, \<span\>에 적용된거죠.

## 개발자 도구 활용 팁

CSS를 수정할 때 계속 새로고침하기 번거롭다면 개발자도구를 활용할 수 있습니다. 

개발자도구에서 다음과 같이 CSS를 변경해가며 결과를 확인할 수도 있습니다.

![image](https://user-images.githubusercontent.com/79847020/174440156-6c1af781-b6f0-40f9-b752-d483dd9c2b4f.png)

## \<a\> 태그 밑줄 제거 : text-decoration: none;

![image](https://user-images.githubusercontent.com/79847020/174440194-ef2e49f1-f820-47c8-b95c-91f52a0727bb.png)

\<a\>태그는 캡처와 같이 밑줄이 생길 수 있습니다. 링크들을 디폴트로 밑줄이 설정되어 있습니다. 

text-decoration: none;를 적용하면 됩니다.

![image](https://user-images.githubusercontent.com/79847020/174440256-7998593a-6f18-49db-a34d-56272a324953.png)

개발자 도구에서 먼저 확인을 할 수도 있습니다.

```CSS
#header-search a {
    text-decoration: none;
}
```

![image](https://user-images.githubusercontent.com/79847020/174440292-d5ac29b2-15e5-4aae-8af5-60bdc0ba24c5.png)

## Margin, Border, Padding, Content

그 다음 검색창을 만들면서 박스 모델에 대해서 알아볼건데요. CSS에서 width와 height를 배웠죠. 

![image](https://user-images.githubusercontent.com/79847020/174440361-bd5ccb2c-1a36-4d2d-966c-8c35747b5515.png)

네이버 검색창은 521x49 입니다. \<fieldset\>이 CSS를 설정하겠습니다.

```CSS
div#header-search fieldset {
    width: 512px;
    height: 49px;
    display: inline-block;
}
```

![image](https://user-images.githubusercontent.com/79847020/174441394-5e271cd0-4839-452a-9b9c-dcd1c5299650.png)

하나의 컨텐트는 margin, border, padding, content로 구성됩니다. 

![image](https://user-images.githubusercontent.com/79847020/174441427-3a18df78-47dd-471f-a083-871f436b769d.png)

초록색 테두리는 border, 그 안에 padding이 있습니다. 

![image](https://user-images.githubusercontent.com/79847020/174441551-5b5a7f2c-c398-4a75-b247-7a39ed6610d5.png)

그 안에 contet가 존재합니다. content와 border사이의 공간을 padding이라고 합니다. margin은 border와 NAVER의 사이를 말합니다.

그래서 border, padding, content 세 영역이 실제 tag의 영역이고 margin은 태그 바깥쪽입니다. 

![image](https://user-images.githubusercontent.com/79847020/174443836-28d58d65-fb36-47ee-8df7-e2d940acd118.png)

이렇게 주황색으로 표시되는 것, 이 부분이 margin입니다. 파란 부분이 content 부분입니다. 물론, 각 영역은 가로 세로 높이가 없을 수도 있습니다.

margin이 21.44px 인 것을 알 수 있습니다. 개발자 도구에서도 다음과 같이 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/79847020/174443892-da27d460-adde-4fb0-8d77-fa7282e4cdcf.png)

각 영역은 4방향이 존재합니다. 다음과 같이 4방향이 있습니다.

```CSS
div#header-search fieldset {
    margin-left: 20px;
    /*padding-top: 12px;*/
    /*padding-bottom: 12px;*/
    /*padding-left: 10px;*/
    /*padding-right: 0px;*/
    padding: 12px 0px 12px 10px;
     
    width: 512px;
    height: 49px;
    display: inline-block;
}
```
padding과 같이 한줄로 표현할 수 있는데 이 때 시계방향으로 값을 입력하면 됩니다. 위 -> 오른쪽 -> 아래쪽 -> 왼쪽 순입니다.

![image](https://user-images.githubusercontent.com/79847020/174445243-65c9af47-6180-4405-8cc0-1b39245ef6d3.png)


















