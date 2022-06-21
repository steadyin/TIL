## position 

HTML을 작성하면 적어준 순서대로 화면에 보여줍니다. 위에서 아래로, 왼쪽에서 오른쪽으로.

그런데 가끔식 그걸 원하지 않을 때가 있습니다. HTML은 문서이기 때문에 항상 위에서 아래로, 왼쪽에서 오른쪽으로 가야되는데 디자인상으로 겹치거나, 바꿔주어야 할 때 position을 사용합니다.

```HTML
            <fieldset>
                <legend class="blind">검색</legend>
                <input/>
                <button>
                    <span class="blind">검색</span>
                    <span id="search-image"></span>
                </button>
            </fieldset>
```

\<fieldset\> 안에 \<input\> 안에 \<button\>이 붙어있는데 제대로 된 위치에 위치시키기 위해서는 position을 사용해야 합니다.

\<input\>은 기본적으로 display: inline-block; 이기 때문에 width, height를 적용하면 크기가 적용됩니다. 

네이버는 input의 크기를 405px, 23px로 지정했습니다.

```CSS
#header-search fieldset input {
    border: none;
    padding: 0;
    outline: none;
    vertical-align: top;
    width: 405px;
    height: 23px;
}
```

참고로 기본 CSS를 보기 위해서는 브라우저에서 user agent(Browser) sheet에서 살펴볼 수 있습니다.

![image](https://user-images.githubusercontent.com/79847020/174817713-c61423a6-a8c7-450c-b7e1-3dcef5901450.png)

\<input\>다음에 바로 \<button\>이 붙는데 조금 어긋납니다.

![image](https://user-images.githubusercontent.com/79847020/174817848-84693c31-fb19-429f-b3c9-adc662860840.png)

position을 자세히 살펴보는 것은 다음에 살펴보고 맛만 보도록 하겠습니다.

position은 여러가지 옵션이 있습니다.

![image](https://user-images.githubusercontent.com/79847020/174818202-0b6de631-e683-48da-9791-4545a17e2b03.png)

\<button\>에 position:absolute; 을 적용해봅니다.

```CSS
#header-search fieldset button {
    width: 49px;
    height: 49px;
    border: 0px;
    padding: 0px;
    background: #03CF5D;
    position: absolute;
    right: 0;
    top: 0;
}
```

![image](https://user-images.githubusercontent.com/79847020/174823574-31a61354-6ff7-4bb3-b7bb-bf6622d66509.png)

우리가 원하는 것은 \<fieldset\>에 붙여주는 것입니다. 이럴 때는 \<fieldset\>에 position: relative;를 적용합니다. 붙길 원하는 곳에다가 position: relative;를 적용합니다.

```CSS
#header-search fieldset button {
    width: 49px;
    height: 49px;
    border: 0px;
    padding: 0px;
    background: #03CF5D;
    position: relative;
}
```

![image](https://user-images.githubusercontent.com/79847020/174823933-75321f92-6e01-4f67-bf56-f6da48435f1f.png)

지금 \<fieldset\>이 border가 2이고 \<button\>이 그 안에 붙기 때문에 border 두께 2px 때문에 정확한 위치에 위치하지 않습니다.

![image](https://user-images.githubusercontent.com/79847020/174823996-3dcff212-1d87-4fbf-b4d7-6c5771a9bb36.png)

이럴 때는 다음과 같이 수정합니다. 

```
#header-search fieldset button {
    width: 49px;
    height: 49px;
    border: none;
    padding: 0;
    background: #03CF5D;
    position: absolute;
    right: -2px;
    top: -2px;
}
```

![image](https://user-images.githubusercontent.com/79847020/174824736-e6a9aae8-cb2b-41a7-968c-7f0099501888.png)

기본적으로 HTML, CSS 구조상 위치하는 곳이 정해져있는데 그 위치를 벗어나고 싶으면 position을 사용하고 absoulte, relative(렐레티브) 그 다음 right,top 이나 left,bottom을 사용해서 조정할 수 있습니다.

## 블록 서식 문맥 : Block Formatting Context. 쌓임 맥락 : Stacking Context

이런 것들이 뭐냐면 나중에 화면에 태그들이 어디에 위치하는지 또는 CSS 속성으로 어떻게 위치를 바꿀 수 있는지에 대한 규칙들입니다. 이런 것들을 알아두어야 자유자재로 배치할 수있습니다.

가장 많이 헷갈려하는 것이 쌓임 맥락을 이해하지 못해서 태그끼리 겹치게 되는데 어떤 것이 위에 보이고 아래 보이는지 그런 것에 대한 내용이 쌓임 맥락입니다.

블록 서식 문맥은 블록들간의 레이아웃들이 어떻게 배치되는지에 대한 내용입니다.

##

![image](https://user-images.githubusercontent.com/79847020/174826350-2fcd22c7-31f7-4492-8b9e-c9737a414908.png)

네이버가 조금 아래로 쳐져있습니다. HTML, CSS 상 이 위치에 위치해있는데 HTML을 수정하지 않고 위치를 수정하고 싶습니다. 

그러면 position: relative; 를 적용하고 top, right 등을 사용해서 위치를 적용할 수 있습니다. 

그래서 다음 시간에 relative, absolute 사용법에 대해서 살펴보도록 하겠습니다.

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
    top: -6px
}
```

![image](https://user-images.githubusercontent.com/79847020/174827304-a259f4d2-b397-4500-a6bc-bb967b9261a8.png)

사실 position, float만 하고나면 따라 만들기만 하면 되서 사실상 다 만들었습니다. 조금 황당해시겠지만 HTML/CSS하실 때 position, float, z-index 정도만 하면 나머지는 나머지는 가로세로 배치하고 개별에 디자인 주고가 다입니다.

네이버 코딩과 강의 내용이 많이 다른데 다른 이유가 모바일이나 테블릿 화면을 지원해야 한다고 하면 html 구조가 달라질 수 있고 cross-browsing 때문에 chrome에서는 잘 보이는데 explorer에서는 잘 안보이는 경우를 해결하다보니깐 달라질 수 있습니다. 

그래서 화면을 그릴 때 여러가지 브라우저에서 테스트를 해봐야합니다. 

  








