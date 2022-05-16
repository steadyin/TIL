![image](https://user-images.githubusercontent.com/79847020/168464875-3034c52a-f5fb-4f6c-8d69-1039b1640253.png)

다음과 같이 Validate 툴을 활용해서 페이지의 오류를 확인할 수 있습니다. Naver 페이지를 확인해봐도 에러가 뜨는데 완전히 지키는 것은 힘드니 참고용으로 활용하면 됩니다.

컨텐츠가 영역내에서 가운데 정렬되어 있으면 가운데 정렬될 컨텐츠를 묶는 것이 중요합니다. 가운데 정렬을 독특한 케이스라고 생각하며 되는데 가로 영역이 아니라 가운데에 위치할 영역을 먼저 묶어주어야 합니다. 네이버를 보면 가상의 선으로 페이지의 컨텐츠들이 가운데에 있는 것을 확인할 수 있습니다.

\<header\>태그 내에 가운데 정렬을 위한 \<div\>태그를 추가합니다. 가운데 정렬을 하려면 CSS를 사용해야 합니다. CSS는 디자인을 위한 것이고 가운데 정렬하는 것은 문서적으로는 의미가 없는데 말하자면 디자인용입니다. 그런 것들은 CSS가 담당한다고 생각하면 됩니다. 

가장 CSS를 쉽게 사용하는 방법은 style 속성을 사용하는 것입니다. 
```html
    <div style="margin: 0 auto;">
```

이렇게 작성하면 div태그가 부모 div태그(header)를 기준으로 가운데 정렬이 됩니다. 그런데 가운데 정렬이 안되는 이유는 div태그의 너비가 항상 100%를 차지하는데 100%를 차지하면 가운데 정렬을 해봤자 변화가 없습니다. 컨텐츠들이 가운데 정렬된다는 것이 아니라 div태그가 부모 div태그를 기준으로 가운데 정렬이 된다는 것을 이해하셔야 합니다. 컨텐츠가 아닌 구역이 가운데 정렬된다. 

![image](https://user-images.githubusercontent.com/79847020/168465755-43fc5b6b-b4d5-43b0-9dde-22917c003f33.png)

네이버 1080으로 너비를 정해두었습니다. CSS에 대한 것도 개발자도구에서 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/79847020/168465792-b4cca2cd-c3f6-40a6-9a3f-794ad71972cf.png)

그래서 네이버 페이지를 따라 만드는데 CSS를 어떻게 했는지 모르겠다 싶으면 개발자도구를 분석만해도 대부분 해결할 수 있습니다. 

```html
    <div style="margin: 0 auto; width: 1080px;">
```

![image](https://user-images.githubusercontent.com/79847020/168465839-a83e95d6-2866-42fa-b3e0-8a3b8cabef0c.png)

컨텐츠가 가운데 정렬이 되는 것이 아니라 영역, 구역의 배치가 가운데 정렬되는 것을 확인할 수 있습니다. 

CSS에 대해서 설명을 하면 문법 자체가 속성명 값으로 표현합니다. px는 픽셀입니다. 속성이 여러개일 경우에는 ;으로 구분합니다. 

```
 <!-- 속성명1: 값1;속성명2: 속성값2;-->
```

이렇게 HTML파일에 CSS를 쓰는 방법은 인라인 스타일이라고 합니다. HTML은 구조와 컨텐츠만 담당하고 CSS를 디자인을 담당하고 자바스크립트 파일은 동작을 담당하게 하는것이 더 좋은 프로그래밍 모델입니다.

naver.css파일을 생성합니다. css파일에 'margin:0 auto; width: 100px;'을 적어봐야 어떤 대상에 적용을 해야할지 알수 없기 때문에 여기서 선택자(Selector) 개념이 등장합니다. 
참고로 CSS에서는 주석을 /* 선택자(selector) */ 으로 작성합니다.

\<div\>를 구분하기 위해 id속성을 사용한다고 했는데 CSS를 적용할 태그의 id를 지정합니다. 

```html
...
<header>
    <div id="header-center">
        <!-- 속성명1: 값1;속성명2: 속성값2;-->
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
</header>
...

그리고 CSS를 다음과 같이 작성합니다.

CSS에서는 id가 #입니다. \{\}를 사용해서 CSS를 입력할 수 있습니다.
```css
/* 선택자(selector) */
div#header-center {
    margin: 0 auto;
    width: 100px;
}
```

,(콤마)를 사용해서 CSS를 여러 곳에 적용할 수도 있습니다.

```css
div#header-center, div#zerocho {
    margin: 0 auto;
    width: 100px;
}
```

div는 빈번히 사용해서 생략할 수 있습니다.
```css
#header-center {
    margin: 0 auto;
    width: 100px;
}
```

다만 div가 아닌 태그들을 꼭 명시해주어야 합니다.
```css
header#header-center {
    margin: 0 auto;
    width: 100px;
}
```

CSS파일을 만들었는데 HTML에 CSS파일 경로를 명시해주어야 합니다. \<link\>태그를 사용합니다. HTML이 로드 될 때 CSS파일도 로드됩니다. 기준이 HTML이라고 생각하면 됩니다. \<link\>태그의 rel속성으로 역할을 구분하시면 됩니다.

CSS가 적용된 것을 확인할 수 있습니다. 개발자 모드에서 CSS는 Styles 탭을 참고하면 됩니다.

![image](https://user-images.githubusercontent.com/79847020/168621448-a07b1300-f300-47eb-80a5-bf87e6e74643.png)

naver.css 밑에 user agent css가 있습니다. \<div\> 같은 태그들은 기본적으로 브라우저가 지정한 스타일이 있습니다. \<div\>같은 경우 display: block; 라는 기본 CSS가 적용되어 있습니다. 이것은 브라우저마다 다를 수 있습니다. 

브라우저간에 다른것을 잘 처리하는 것을 크로스 브라우징이라고 부릅니다. 

\<html\>태그에 적용한 CSS가 \<div\> 태그에 영향을 미칩니다. 이해가 다 안될 수도 있는데 CSS의 특징입니다. CSS는 Cascading Style Sheet의 약어인데 Cascading 뜻이 덮어씌워진다라는 의미를 갖고 있습니다. 조상에 적용 된 CSS가 있으면 자손 CSS값이 값이 덮어씁니다라고 이해하면 됩니다.
























