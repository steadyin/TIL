## FIXED

그리고 fixed는 화면에 고정되어 있는 것을 의미합니다. 스크롤을 내려도 그 컨텐츠는 계속해서 같은 위치에 존재합니다. 

```HTML
...
<div id="fixed">고정</div>
...
````

```CSS
#fixed {
    position: fixed;
    top: 50px;
    right: 50px;
    display: inline-block;
    width: 100px;
    height: 100px;
    border: 1px solid black;
}
```

![image](https://user-images.githubusercontent.com/79847020/176994341-79dbfc63-cc49-4db3-a31e-9d99d09b3e7c.png)

![image](https://user-images.githubusercontent.com/79847020/176994397-5f76a7c3-39d0-435c-98fb-1453e18de880.png)

* 위치를 움직여도 fixed가 적용된 컨텐츠는 그위치에 존재합니다.
* border-radius는 테두리는 둥그렇게 합니다.
* fixed이기 때문에 위치를 지정해주어야 합니다. top, bottom, left, right를 사용합니다.
* fixed도 containing block 기준으로 위치를 지정합니다. fixed도 absolute와 유사하다고 생각하면 됩니다. 

```HTML
...
<body>
<header>
    <div id="fixed">고정</div>
    <div id="header-center"> <!-- 가운데 정렬용 div -->
        <!-- 속성명1: 값1;속성명2: 속성값2;-->
        <!-- 인라인 스타일 -->
        <div>
            <!--<div style="display:inline-block;">네이버를 시작페이지로</div>-->
            <!--<div style="display:inline-block;">쥬니어네이버</div>-->
            <!--<div style="display:inline-block;">해피빈</div>-->
            <span>네이버를 시작페이지로</span>
            <span>쥬니어네이버</span>
            <span>해피빈</span>
...
```

* fixed도 containing block이 header가 아닙니다. header은 positiong: static; 이기 때문입니다.


![image](https://user-images.githubusercontent.com/79847020/176994483-752cf673-92f2-41fe-9555-aade654c09d0.png)

* 따라서 containing block을 파악하는 것인 position에서 중요합니다.

문서의 예제를 살펴보겠습니다.

다음 예제에서 문단은 정적 위치를 가지고, 가장 가까운 블록 컨테이너는 \<section\>이므로 문단의 컨테이닝 블록도 \<section\>입니다.

```CSS
body {
  background: beige;
}

section {
  display: block;
  width: 400px;
  height: 160px;
  background: lightgray;
}

p {
  width: 50%;   /* == 400px * .5 = 200px */
  height: 25%;  /* == 160px * .25 = 40px */
  margin: 5%;   /* == 400px * .05 = 20px */
  padding: 5%;  /* == 400px * .05 = 20px */
  background: cyan;
}
```

![image](https://user-images.githubusercontent.com/79847020/176994556-bb8ae518-6f48-46b1-b9e5-bb8b40bd8d30.png)

* width를 %로 줘서 containing block의 수치를 %로 활용할 수 있습니다.
* containing block이 항상 부모가 아닐 수 있음을 이해해야 합니다.
* 예를 들어 이 예제에서는 \<body\> 안에 \<section\>안에 \<p\>가 들어있는데 \<p\>의 containing block이 \<section\>인지 생각해보셔야 합니다. 
* 예제1에서 \<p\>의 containing block은 \<section\>입니다. width는 containing block의 50%가 되어 200px가 됩니다.

```CSS
body {
  background: beige;
}

section {
  display: inline;
  background: lightgray;
}

p {
  width: 50%;     /* == body 너비의 절반 */
  height: 200px;  /* 참고: 백분율 값이었으면 0 */
  background: cyan;
}
```

![image](https://user-images.githubusercontent.com/79847020/176994690-f6d17b87-1b00-422d-9836-aa665489cbea.png)

이번에는 \<section\>의 display:inline;입니다. display:inline;인 경우 containing block이 될 수 없습니다. 그런 경우 \<p\>의 containing block은 \<section\>을 건너뛰고 \<section\>의 부모인 \<body\>까지 올라가서 \<body\>의 width 50%를 적용합니다.

## 컨테이닝 블록 식별

컨테이닝 블록의 식별 과정은 position 속성에 따라 완전히 달라집니다.

1. position 속성이 static, relative, sticky 중 하나이면, 컨테이닝 블록은 가장 가까운 조상 블록 컨테이너(inline-block, block, list-item 등의 요소), 또는 가장 가까우면서 서식 맥락을 형성하는 조상 요소(table, flex, grid, 아니면 블록 컨테이너 자기 자신)의 콘텐츠 영역 경계를 따라 형성됩니다.

2. position 속성이 absolute인 경우, 컨테이닝 블록은 position 속성 값이 static이 아닌(fixed, absolute, relative, sticky) 가장 가까운 조상의 내부 여백 영역입니다.

3. position 속성이 fixed인 경우, 컨테이닝 블록은 뷰포트나 페이지 영역(페이지로 나뉘는 매체인 경우)입니다.

4. position 속성이 absolute나 fixed인 경우, 다음 조건 중 하나를 만족하는 가장 가까운 조상의 내부 여백 영역이 컨테이닝 블록이 될 수도 있습니다.
    1. transform이나 perspective (en-US) 속성이 none이 아님.
    2. will-change 속성이 transform이나 perspective임.
    3. filter 속성이 none임. (Firefox에선 will-change가 filter일 때도 적용)
    4. contain 속성이 paint임.

첫번째 규칙은 static, relative, sticky 중 하나이면 containing block은 inline이 될 수 없다고 외우시면 됩니다. 네번째 규칙은 ii, iii, iv보다는 i규칙이 중요합니다. \<transform\>이 빈번이 사용되기 때문에 2번 규칙만 외우고 있다가 absolute와 transform을 같이 사용하는 경우 혼란이 올 수 있습니다. 이 문서를 항상 즐겨찾기 해두고 틈틈히 보는 것을 추천합니다. 

\<body\>가 아니라 \<html\>이 루트 요소입니다. 강의 오류 !!

하나 생각하셔야 될게 \<body\>에 대부분의 브라우저가 기본적으로 margin이 적용되어 있습니다.

![image](https://user-images.githubusercontent.com/79847020/177176723-a29f53b5-fd7f-4e95-bba4-88970084503e.png)

그래서 약간의 공간이 있습니다. 

![image](https://user-images.githubusercontent.com/79847020/177176799-fc92c013-bc58-43c7-a92a-a012aea98081.png)

이런 것들을 왠만하면 없애주고 시작합니다. body 뿐만 아니라 html도 다음과 같이 마찬가지로 적용합니다.

```CSS
html, body {
    margin: 0;
    padding: 0;
}
```

돋보기버튼을 이전에 margin을 사용해서 가운데 정렬을 했는데 positiong을 사용해서 정렬할수도 있습니다. 가운데 정렬하는 방법을 여러가지가 있는데 한 가지를 제대로 알아두시고 활용하시면 됩니다.

https://www.zerocho.com/category/CSS/post/5881edef636a7f0b8e8507d8

flex를 사용해도 되고, 단 익스플로러에서는 동작하지 않습니다.
```
@css
display: flex;
align-items: center;
```

그 외 display: table-cell, display: table를 transform을 이용하는 방법 여러가지가 있습니다. 



