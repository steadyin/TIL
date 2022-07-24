

이번시간에는 float과 블롯 포맷 컨테스트 또는 블록 서식 문맥이라고 번역하는 개념에 대해서도 알아보겠습니다.

컨테이닝 블록은 자식 입장에서 포지션을 가졌을 때 누구를 기준으로 위치해야하는가, 위쪽 태그를 찾아가는 규칙입니다.

그 다음 즐겨찾기를 하고 두고 보셔야 할 것이 Block Formatting Context입니다.

https://developer.mozilla.org/ko/docs/Web/Guide/CSS/Block_formatting_context

Block Format Context는 부모 입장에서 자식이 누구 까지를 내 블록에 포함해야되나를 찾아보는 개념입니다.

참고로 이 블록은 display: inline-block;의 블록과는 다른 의미입니다. 헷갈리면 안됩니다.

블록 서식 문맥은 display: block;일 때는 안생기고 display: inline-block; 일 때 블록 서식 문맥이 생깁니다. 

그래서 블록 서식 문맥의 블록과 display: block;의 블록과 구분을 하셔야 합니다.

```
* 문서의 루트 요소(<html>).
* 플로팅 요소(float이 none이 아님).
* 절대 위치를 지정한 요소(position이 absolute 또는 fixed).
* 인라인 블록(display가 inline-block).
* 표 칸(display가 table-cell, HTML 표 칸의 기본값).
* 표 주석(display가 table-caption, HTML 표 주석의 기본값).
* display가 table, table-row, table-row-group, table-header-group, table-footer-group (HTML 표에서, 각각 표 전체, 행, 본문, 헤더, 푸터의 기본값) 또는 inline-table인 요소가 암시적으로 생성한 무명 칸.
* overflow가 visible이 아닌 블록 요소.
* display가 flow-root.
* contain이 layout, content, paint.
* 스스로 플렉스, 그리드, 테이블 컨테이너가 아닌 경우의 플렉스 항목(display가 flex 또는 inline-flex인 요소의 바로 아래 자식)
* 스스로 플렉스, 그리드, 테이블 컨테이너가 아닌 경우의 그리드 항목(display가 grid 또는 inline-grid인 요소의 바로 아래 자식)
* 다열 컨테이너(column-count (en-US) 또는 (column-width (en-US)가 auto가 아닌 경우. column-count: 1 포함).
* column-span (en-US)이 all인 경우. 해당하는 요소가 다열 컨테이너 안에 위치하지 않아도 항상 새로운 블록 서식 맥락을 생성해야 합니다. (명세 변경, Chrome 버그)
```

그리고 overflow가 visible이 아닌 블록 요소를 잘 기억해두셔야 합니다. 아직 overflow를 배우지는 않았지만 블록 서식 문맥을 만드는 방법 중에 인라인 블록과 float과 overflow를 기억해두셔야 합니다.

* 인라인 블록(display가 inline-block).
* 플로팅 요소(float이 none이 아님).
* overflow가 visible이 아닌 블록 요소.

하나 더 기억하실만한게 

* 절대 위치를 지정한 요소(position이 absolute 또는 fixed)

이것들이 중요하니깐 기억하시면 좋을 것 같습니다. 

![image](https://user-images.githubusercontent.com/79847020/180631093-8c4bdc2e-c599-4719-8a11-34152819cca7.png)

상단 메뉴 네이버를 시작페이지로 쥬니어네이버 해피빈을 오른쪽 구석으로 옮기는 방법에는 무엇이 있을까요 ?

방법은 여러가지가 있습니다. CSS랑 HTMl은 방법이 여러가지이기 때문에..

text-align: right; 를 적용하는 방법이 있구요.

position: absolute; right: 0; top: 0; 을 하고 감싸주는 div를 하나 더 만들어서 div의 position: relative; 로 해야지 가운데 정렬되면서 position: absolute가 되겠죠;

이게 지난 시간에 배웠던 Containing Block의 개념이죠. 

position: absolute; 인 경우에는 컨테이닝 블록이 부모 태그가 아니라 더 올라가서 position: relative; 나 absolute; 가 있는지 없다면 html 태그가 컨테이닝 블록이 되는 거죠.

만약 부모 태그가 컨테이닝 블록으로 만들고 싶다면 position: relative; 줘야되겠죠.

그리고 한가지 다른 방법으로.

![image](https://user-images.githubusercontent.com/79847020/180634817-dcf773c5-1a58-4f78-ab1f-4c3865e4a979.png)

일단 높이를 잘보셔야 합니다. '네이버를 시작페이지로 쥬니어네이버 해피빈'이 일정 높이를 차지하고 있고 그 아래 네이버 검색창 구역이 있습니다.

이 상태에서 float: right;을 적용하면 다음과 같이 오른쪽으로 위치시킬 수 있습니다.


* float: right; 적용

![image](https://user-images.githubusercontent.com/79847020/180634877-41b35d7f-19d2-42e6-8f18-828b4c795ed6.png)

* text-align: right; 적용

![image](https://user-images.githubusercontent.com/79847020/180635029-9d5e0719-1002-46ca-bfc6-76aa490ef194.png)

text-algin:right 과 float:right과 차이가 있는 것을 확인할 수 있는데 float:right를 적용하면 Naver 검색 영역이 올라가는 것을 확인할 수 있습니다.

실제 html 소스는 다음과 같습니다.

```html
...생략	
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
        <div id="header-search">
            <a href="https://www.naver.com">
                <h1>
                    <span>네이버</span>
                    <!-- 주석(comment)입니다 -->
                    <!-- 속성(attribute)입니다 -->
                </h1>
            </a>
            <h2 class="blind">검색창</h2>
            <fieldset>
                <legend class="blind">검색</legend>
                <input/>
                <button>
                    <span class="blind">검색</span>
                    <span id="search-image"></span>
                </button>
            </fieldset>
        </div>
    </div>
...생략	
```	

![image](https://user-images.githubusercontent.com/79847020/180639463-e9138a74-7bcc-4e5e-ad78-abde7c36f75b.png)

영역을 확인해보니 상단 메뉴 div와 검색 영역 div는 형제 태그임에도 불구하고 검색 영역 div가 감싼것처럼 표시됩니다. 실제로 감싼 것은 아니고 float의 의미가 둥둥 떠있다 라고 생각하면 됩니다. float이 적용되지 않았을 때는 div가 일정 영역을 차지하므로 검색 영역과 위 아래 영역을 각각 점유하는데 float이 적용되는 순간 떠버립니다. 따라서 검색 영역이 올라오게 됩니다. 

float의 특징에 대해서 살펴보아야 하는데요. float영역은 떠있다고 생각하면 됩니다. 그래서 아래 영역이 올라오게 되는데 그렇다고 겹치지는 않습니다. 

예시를 한번 보겠습니다. 

참고로 background-color: red;와 같이 영어로 색상을 지정할 수도 있습니다. #121212와 같이 Hex값으로 지정하는 방법을 이전에 언급했었는데 이름있는 색상들은 단어로 지정할수도 있습니다. 

```HTML
    <div>
        <div style="float: right; background-color: green;">둥둥둥둥</div>
        <div>가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하</div>
    </div>
```	

![image](https://user-images.githubusercontent.com/79847020/180639484-0b522423-5255-4d6f-9ca5-bc285e1930d9.png)

![image](https://user-images.githubusercontent.com/79847020/180639522-c7f86f3f-1afd-49b9-9a0c-f076fd3a62e0.png)

떠있다고 해서 둥둥둥둥 Content 밑으로 다른 내용들이 들어가는 것이 아니라 둘러 싼 형태가 되어서 보이게 됩니다. 

여기서 float에 height 옵션이 추가된 경우를 봅시다.

```HTML
    <div>
        <div style="float: right; background-color: green; height: 500px">둥둥둥둥</div>
        <div>가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하</div>
    </div>
```	

![image](https://user-images.githubusercontent.com/79847020/180639592-e0bcf416-8899-401b-8791-b48a87a020e2.png)

그리고 이렇게 했을 때는 float의 부모 영역이 float 영역을 포함하고 있지 않은 것을 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/79847020/180639670-dce5edfc-bf35-48f5-8138-ccc12db332a7.png)

```HTML
    <div style="background-color: yellow;">
        <div style="float: right; background-color: green; height: 500px">둥둥둥둥</div>
        <div>가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하</div>
    </div>
```	

![image](https://user-images.githubusercontent.com/79847020/180639708-ee9bf013-9787-4d02-aa3d-64bbfb20a9de.png)

부모가 자식 영역을 포함하고 있지 않습니다.

여기서 block-foramt-context 개념이 나오게 됩니다. 블록 서식 문맥이란 것은 부모 입장에서 어떤 자식 태그를 포함할 수 있나 입니다. 무조건 부모 태그라고 해서 자식을 감싸는 것이 아닙니다. 위에서 언급
한 것들입니다.

블록 서식 문맥은 다음 중 하나에 의해 생서된다.

* 인라인 블록(display가 inline-block).
* 플로팅 요소(float이 none이 아님).
* overflow가 visible이 아닌 블록 요소.
* 절대 위치를 지정한 요소(position이 absolute 또는 fixed)

외울 필요는 없고 즐겨찾기 해놓고 계속 보다 보면 자연스럽게 외워집니다. 

\<div style="backgroun-color: yellow;"\> 블록 포맷 컨텍스트가 아니므로 감쌓을 수가 없습니다. 그래서 overflow: visible;이 기본값인데 auto;로 바꿔줍니다.

```HTML
    <div id="fixed">고정</div>
    <div style="background-color: yellow; overflow: auto;">
        <div style="float: right; background-color: green; height: 500px">둥둥둥둥</div>
        <div>가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하</div>
    </div>
```	

![image](https://user-images.githubusercontent.com/79847020/180639864-7c49fbcd-c6c8-4deb-9fea-a771a3b4cfa3.png)

overflow: visible;이 아니게 되었으므로 \<div\>가 블록 포맷 컨텍스트가 설정되고 float 태그를 자식 태그를 포함하게 됩니다. overflow: hidden; 도 마찬가지인데 auto와 hidden의 차이는 부모의 높이가 있을 때 자식이 부모보다 크면 부모의 영역만큼 차지하고 필요시 스크롤바를 만들게 됩니다.

```HTML    
	<div id="fixed">고정</div>
    <div style="background-color: yellow; overflow: hidden; height: 150px">
        <div style="float: right; background-color: green; height: 500px">둥둥둥둥</div>
        <div>가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하</div>
    </div>
```

![image](https://user-images.githubusercontent.com/79847020/180639966-a346cb7a-c13d-46d8-ac2e-5b414b03b919.png)

기본값 overflow: visible; 이면 부모 \<div\>는 블록 포맷 컨텍스트로 인정되지 않아 float이 포함되지 않습니다.

![image](https://user-images.githubusercontent.com/79847020/180640052-35bb8d3c-9ebd-47c2-bf7b-0d7472826cbe.png)

보통은 overflow: hidden; 을 사용해서 자식 태그를 부모 태그로 감쌉니다.  
 
블록 포맷 컨텍스트를 사용하기 싫다면 text-align: right; 이나 position: absolute; 를 사용하는 것이 한가지 방법입니다. 

```HTML
    <div id="fixed">고정</div>
    <div style="background-color: yellow; overflow: visible; height: 150px">
        <div style="float: right; background-color: green; height: 50px">둥둥둥둥</div>
        <div>가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하</div>
    </div>
```	

![image](https://user-images.githubusercontent.com/79847020/180640128-72f2e5ed-4983-4976-aef1-44cd311e8710.png)

이 상황은 float이 떠있고 형제태그가 끼워넣어지게 되지만 float이 차지하는 공간은 피해서 보여지게 됩니다. 둥둥둥둥 공간만큼 피해서 보여지게 됩니다.  

만약 여기서 둥둥둥둥을 감싸고 싶지 않다면 가나다라~타파하에서도 블록 포맷 컨텍스트를 만들어주면 됩니다.

display: inline-block;을 적용하면 

```HTML
    <div id="fixed">고정</div>
    <div style="background-color: yellow; overflow: visible; height: 150px">
        <div style="float: right; background-color: green; height: 50px">둥둥둥둥</div>
        <div style="display: inline-block;">가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하</div>
    </div>
```	

![image](https://user-images.githubusercontent.com/79847020/180640246-68737815-5692-4573-8a29-8fbe91aac992.png)

inline-block을 하니깐 블록 포맷 컨텍스트가 되긴 했는데 둥둥둥둥 밑으로 새로운 블록을 만들었습니다. 

대신에 overflow:auto를 주면 

```HTML
    <div id="fixed">고정</div>
    <div style="background-color: yellow; overflow: visible; height: 150px">
        <div style="float: right; background-color: green; height: 50px">둥둥둥둥</div>
        <div style=" overflow: auto;">가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하가나다라마바사아자차카타파하</div>
    </div>
```	
![image](https://user-images.githubusercontent.com/79847020/180640306-7a74f7df-4724-4706-a4af-aa233444903f.png)

별도로 블롯 포맷 컨텍스트를 생성해서 감싸는 것을 방지할 수 있습니다.















