## Html_CSS_15.md

오늘은 컨테이닝 블록이랑 포지션에 대해서 알아보도록 하겠습니다. mdn사이트에서 3개의 글을 소개하겠습니다. 

w3schools.com 같은 곳에서 배워도 되지만 부정확합니다. mdn 정도면 신뢰성 있는 사이트로 평가됩니다. Firefox를 만든 Mozilar에서 만든 사이트 입니다.

## 컨테이닝 블록(Containing Block) 의 모든 것 

https://developer.mozilla.org/ko/docs/Web/CSS/Containing_block

\<span\> 태그를 \<div\>가 감싸고 있다면 \<span\> 태그의 컨테이닝 블록은 \<div\>가 되는 것입니다. 

```HTML
        <div>
            <span>네이버를 시작페이지로</span>
            <span>쥬니어네이버</span>
            <span>해피빈</span>
        </div>
```

감싸고 있다고 해서 무조건 컨테이닝 블록이 되는 것은 아닙니다. 컨테이닝 블록이 될 수 있는 조건이 있습니다. 때문에 식별방법을 알고 있어야 합니다. 

컨테이닝 블록이 왜 중요하냐면 부모 자식 관계에서 부모의 가로 세로 너비 높이에도 영향을 미치고 자식의 가로 세로 너비 높이에도 영향을 주기 때문입니다.

Margin, Padding, Border 에도 영향을 주기 때문에 누가 누구를 감싸고 있는지 파악을 하셔야 합니다.

그전에 먼저 포지션 개념에 대해서 알아야 합니다.

### position: satatic;

모든 태그들은 포지션이 static이라는 포지션을 사용합니다. static은 정적이라는 의미입니다. 기본적으로 html은 위에서 아래로, 왼쪽에서 오른쪽으로 순차적으로 보여지는데 기본적으로 그 구조를 유지하는 것을 말합니다. 

### position: relative;

relative는 상대적인이라는 의미입니다. 상대적이란 말은 비교대상이 있어야 합니다. 비교대상은 현재 static인 경우의 위치입니다. static 위치에 대비해서 상대적으로 위치를 바꾸는 것입니다.

position: relative; top: 0px;

![image](https://user-images.githubusercontent.com/79847020/175034037-aea3a146-8d5a-4067-ad5a-a3572b49e521.png)

position: relative; top: -20px;

![image](https://user-images.githubusercontent.com/79847020/175034192-fce2ff8d-7704-4129-b1a5-8ef6ed3d638c.png)

position: relative; top: +20px;

![image](https://user-images.githubusercontent.com/79847020/175034390-ea9e26d7-fd59-4b05-8505-e440af47badd.png)

position: relative; 는 position: static; 일 때 위치를 기반으로 움직인다고 기억하시면 됩니다.

### position: absolute;

absolute를 절대적이라는 것인데 화면을 기준으로 위치를 지정하는 것입니다. 

항상 전체적인 화면을 기반해서 움직이는 것이 아니라 absolute는 한 가지 규칙이 있는데 absolute인 경우 컨테이닝 블록은 position 속성 값이 static이 아니고 (fixed, absolute, relative, sticky 이고 ) 가장 가까운 조상의 내부 영역입니다. 이 말은 태그가 absolute면 부모들을 계속 찾아가면서 static이 아닌 영역을 기준으로 삼습니다.

sticky는 최근에 나온 것인데 익스플로러에서 지원하지 않기 때문에 잘 사용하진 않습니다. 잘 사용하지 않습니다. 

예를 들어 






## 블록 포맷 컨텍스트

## 쌓인 맥락 (Stacking Context)
