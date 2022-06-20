## box-sizing: content-box;

분명히 네이버 메인의 CSS와 동일하게 해준 것 같은데 큰 차이가 있습니다.

![image](https://user-images.githubusercontent.com/79847020/174620355-53a9a811-e9c7-4926-b2e3-a09d469c2471.png)

![image](https://user-images.githubusercontent.com/79847020/174620378-80f29659-8ccf-4aba-8f8d-a16629a869cc.png)

네이버 메인의 검색창의 크기 521px x 49px 가 content 크기라고 생각했었는데 알고보닊나 보더까지 포함한 크기입니다. 

![image](https://user-images.githubusercontent.com/79847020/174620833-db0bfa29-6ba7-4b4b-a1ed-494ef14b53f5.png)

그런데 저희가 만든 것은 Content 크기만 512 x 49 입니다.

![image](https://user-images.githubusercontent.com/79847020/174621047-2b1e0c1d-d20f-49d0-96ac-c57816a8c88f.png)

이게 박스 모델이라는것인데 기본적으로 html에서는 Content, Pdading, Border, Width을 구분합니다. 이것을 Content-box라고 하는 것입니다. 

box-sizing의 기본 값은 content box입니다.

```
box-sizing: content-box;
```

이것의 단점이 border, padding, content 다 합쳐서 521이 돼야 하는데 width는 content만 가리키고 있습니다. 실무에서는 이것이 매우 불편합니다. 왜냐하면 우리가 생각하는 width는 border, padding, content를 다 합친 것인데 브라우저가 가리키는 width는 content만 가리키는 것이라서 우리가 일일이 계산을 해주어야 합니다.

그래서 브라우저에게 width를 content만 가리키지 말고 border, padding, content를 가리켜라라고 바꿔줄 수 있습니다. 

```
box-sizing: border-box;
```

border-box로 바꾸면 border, padding, content 다 합쳐서 512x49가 되는 것입니다.

![image](https://user-images.githubusercontent.com/79847020/174622554-916250d5-4bbb-4560-81e5-14daac7fa899.png)

![image](https://user-images.githubusercontent.com/79847020/174622528-95ab9b37-eecd-43a8-af95-65a3c835834f.png)

그래서 왠만하면 모든 태그에 border-box를 적용하는 것이 좋습니다. 안그러면 매번 content, padding, border를 숫자 계산해야 되기 때문에 아예 width를 content 기준에서 border + padding + content 기준으로 바꿔버리는 것이 좋습니다.

매번 box-sizing: border-box; 를 적용하는 것은 번고로운데 개발자는 중복된 게 있으면 줄이려는 노력을 해야합니다.

## \* {}

여기서 줄일 수 방법은 \*를 애스터리스크라고 부르는데 CSS에서 사용하면 모든 태그를 의미합니다.

```CSS 
...
* {
    box-sizing: border-box;
}
...
```

## CSS Reset

![image](https://user-images.githubusercontent.com/79847020/174625883-24dbbca2-7c14-4a00-8167-2a752c91f58d.png)

그리고 박스 모델을 할 때 알아두어야 하는 점이 기본 태그 input에 CSS를 아무것도 적용하지 않았는데 다들 기본적으로 CSS가 적용되어있습니다.

![image](https://user-images.githubusercontent.com/79847020/174626096-29a37ea1-9e65-4372-86bd-f72d8acee4f7.png)

왜냐하면 브라우저에서 CSS를 넣어주기 때문입니다. \<input\> 에도 아무 CSS를 적용하지 않았지만 border나 padding이 기본적으로 적용되어 있습니다. 이런 것들이 좋을 수도 있지만 브라우저마다 기본 CSS가 다르기 때문에 브라우저별로 다르게 보일 가능성이 있습니다.

그래서 기본 CSS를 없애주는 행위를 하는 것이 좋은데 이 행위를 CSS Reset라고 부릅니다.

이것은 나중에 파일을 소개해드리겠습니다. reset.css나 nomalize.css를 많이 사용합니다. 

## border: 2px solid #03cf5d; , border: none

\<input\>에 적용되어 있는 기본 CSS를 지우겠습니다.

만약 border CSS를 다음과 같이 적용하면 순서대로 굵기, 색상을 의미합니다.

```CSS
border: 2px solid #03cf5d;
```

#03cf5d를 hex 표기법이라고 합니다. 구글에 hex color라고 검색하면 다음과 같이 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/79847020/174627118-81583118-b295-4d7d-b478-42c655186abd.png)

색상 표기법은 HEX, RGB, CMYK, HSV, HSL이 있는데 웹에서는 보통 RGB나 HEX를 많이 사용합니다.

RGB 표기법으로 다음과 같이 표현할 수도 있습니다.

```CSS
border: 2px solid rgb(61, 133, 5);
```

## \<input\> 초기화

그래서 다음과 같이 CSS를 적용합니다.

```CSS
...
#header-search fieldset input {
    border: none;
    padding: 0;
}
...
```

border는 none을 하고 padding, margin은 0하면 top, right, bottom, left 모두 적용됩니다.

![image](https://user-images.githubusercontent.com/79847020/174628314-d680ae6f-fdeb-4289-ab8e-2027ed74e4e6.png)

그리고 선택하면 나오면 나오는 테두리가 있습니다.

![image](https://user-images.githubusercontent.com/79847020/174628498-9d462f7e-1dba-4701-a771-6168e99cc003.png)

이것을 아웃라인이라고 합니다. 이것마저도 지저분하다고 생각되면 다음과 같이 지울 수 있습니다.

```CSS
...
#header-search fieldset input {
    border: none;
    padding: 0;
    outline: none;
}
...
```

CSS는 기본 원리만 깨달으면 외우는게 다입니다.

## vertical-align: middle;

![image](https://user-images.githubusercontent.com/79847020/174628975-12a9e12d-4edc-4d37-9cda-916cda762965.png)

현재 위치가 맞지 않습니다.

형제 태그인데 높낮이가 안맞을 때가 있습니다. 이런 경우는 형제 태그 끼리 서로 height가 다르면 정렬이 달라집니다. 

정렬이 달라지기 때문에 세로 정렬를 하기 위해 vertical-align을 맞춰주어야 합니다.

여기서는 middle; 로 지정하겠습니다.

vertical-align: middle; 해도 맞지 않는 경우 수정하는 방법이 있는데 그건 포지션에서 다루겠습니다. 

```CSS
...
div#header-search h1 {
    width: 198px;
    height: 48px;
    display: inline-block;
    /*background-image: url(sp_search.png);*/
    /*background-position: -4px -10px;*/
    /*background-repeat: no-repeat;*/
    background: url(./sp_search.png) -4px -10px no-repeat;
    vertical-align: middle;
}
...
#header-search fieldset input {
    border: none;
    padding: 0;
    outline: none;
    vertical-align: middle;
}
```

![image](https://user-images.githubusercontent.com/79847020/174634989-68e852d1-e694-43bc-83a2-a794e4dabea9.png)























