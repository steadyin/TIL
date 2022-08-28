# 7.4 Z-index와 쌓인 맥락

Z-Index를 보겠습니다. 지금 예제로는 설명하기 힘들어 추가를 하겠습니다. 

```HTML
            <div id="search-ranking">
                <h2 class="blind">실시간 검색어</h2>
                <div>
                    <span id="rank-number">1</span>
                    <span id="rank-title">제로초</span>
                </div>
                <ul>
					<li>
						<span class="rank-number">1</span>
						<span class="rank-title">제로초</span>
					</li>
					...
				</ul>
                <div id="z-example"></div>
            </div>
```

```CSS
#z-example {
    position: absolute;
    background-color: yellow;
    width: 100px;
    height: 100px;
}
```
				
![image](https://user-images.githubusercontent.com/79847020/187062355-4592c8c2-8e65-459d-bea9-c9674db76686.png)

렌더링 되었을 때 \<ul\>이 \<z-example\> 아래에 위치합니다. 기본적으로 태그에서 \<ul>보다 \<z-example\>이 먼저 나오기 때문에 \<ul\>영역이 \<z-example\>영역보다 위에 위치하는 것이 맞습니다. 이것을 Z-Index로 조정할 수 있습니다.

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
    z-index: 100;
}

#z-example {
    position: absolute;
    background-color: yellow;
    width: 100px;
    height: 100px;
    z-index: 1;
}
```
![image](https://user-images.githubusercontent.com/79847020/187062728-782bcbac-1d3c-4a4f-a81c-5079889744ea.png)

여기까지는 간단합니다. 이런 경우에는 헷갈릴 수 있습니다. 

```HTML
...
	<ul>
		<li>
			<span class="rank-number">1</span>
			<span class="rank-title">제로초</span>
		</li>
		...
	</ul>
	<div id="z-example">
		<div id="z-inner"></div>
	</div>
...		
```		

```CSS
#z-inner {
    position: absolute;
    background-color: red;
    width: 50px;
    height: 50px;
    z-index: 1000;
}
```

Z-Index만 아니라 부모 자식 관계도 있습니다. UL과 Z-example는 형제관계이고 Z-inner는 Z-example과 부모자식관계 입니다.

![image](https://user-images.githubusercontent.com/79847020/187063222-c6eece7f-7118-45dc-866e-375918c4451e.png)

Z-Index가 100밖에 되지 않는 UL이 가장 위에 올라오게 됩니다. 

z-example과 z-inner만 보면
z-example은 z-index가 1이고 z-inner는 z-index가 100이므로 z-index에 의해 z-inner가 z-exmaple보다 위에 있는 것이 맞다고 생각되어집니다.

여기서 등장하는 개념이 쌓인 맥락입니다. MDN에서 즐겨찾기하고 두고 보면 좋다는 3개가 Containing block, Block Format Context, 쌓인 맥락 추가로 Margin Colapse까지 보면 좋습니다. 

또 도저히 이해가 안간다고 언급한 것은 display:inline 블록이 연속해서 배열될 때 생기는 문제들을 강좌에서 배워가시면 좋을 것 같습니다.

나머지는 속성을 외우는 개념이지만 이 5가지는 원리를 알아야 합니다. 

쌓인 맥락도 글자로만 보면 이해하기가 쉽지 않습니다. 

![image](https://user-images.githubusercontent.com/79847020/187064257-7e6c2eeb-78d2-42b4-a36f-e0e705e4ed11.png)

여기서 3가지를 외우시면 됩니다.

* html 뿌리 엘리먼트
* position 속성이 지정되고(absolute, relative든 상관없이) z-index속성이 auto이외의 값을 가지는 엘리먼트들
* 투명도(overlay)가 1보다 적게 지정된 엘리먼트들(opacity)

저희가 flex를 안했기 때문에 3개만 외우시면 됩니다. 

![image](https://user-images.githubusercontent.com/79847020/187064368-278d261e-7c8a-45fe-a185-d0955a9baf32.png)

그 다음에 쌓임 맥락은 다른 쌓임 맥락 안에 포함될 수 있다.

이게 왜 중요하냐면 z-example도 z-index 있고 position: absolute;니깐 쌓임 맥락이죠. z-inner도 쌓임 맥락이니깐 쌓임 맥락 자식이 쌓임 맥락입니다. 

그리고 부모 쌓임 맥락들은 자손 쌓임 맥락과는 완전히 독립되어 있다. 오로지 자손 쌓임 맥락만이 부모 쌓임 맥락의 영향을 받는다.

쉽게 설명 하자면 부모간의 관계를, 먼저 ul과 z-example을 비교합니다. ul이 z-index이 높습니다. z-inner는 z-example의 자식인데 z-example보다 ul보다 z-index가 낮았죠. z-index 낮은 태그의 자식은 아무리 커도 부모 태그보다 z-index가 높은 태그보다 우선순위가 높을 수가 없습니다. 그래서 우리가 예제로 작성했던 것처럼 동작했던 것입니다. 

부모 태그의 z-index를 이기는 태그를 자식 태그는 절대 이길 수 없습니다. 자식 태그가 아무리 z-index 수치가 높아봤자 부모 태그간의 z-index에 영향을 줄 수가 없습니다.

팝업이나 모델 처럼 버튼 눌렀을 떄 뜨는 것들이 여러개 쌓이다보면 z-index간의 관계 때문에 혼동되는 상황이 발생하니깐 이것을 알아두셔야 됩니다. 자식은 부모의 z-index관계를 뛰어넘지 못한다. 부모에서 z-index가 정해졌으면 자식은 아무리 z-index가 높아도 부모의 z-index가 적용되는 것입니다. 자식의 z-index는 형재 간에서만 중요하다고 생각하시면 됩니다. 

![image](https://user-images.githubusercontent.com/79847020/187064666-396deedd-a4e1-45b2-bf3c-46dcf8dd4a6d.png)

급상승 검색어도 가로,세로 룰을 계속 적용하면 됩니다. 가로로 '급상승 검색어, 전체연령, 현재' 부분에서 자를 수 있고 '1-10위, 11-20위' 부분에서 자를 수 있고 랭킹 부분은 \<ul\>을 사용해서 분할하면 되겠죠. 마지막 일자, 출처도 자를 수 있겠죠.

그 다음에 세로로 자르면 됩니다. 전체연령 | 현재는 float: right을 사용하면 되겠죠.


