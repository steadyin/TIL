# 8.1 주제별 캐스트 메뉴 만들기

네이버의 주제별 캐스트 메뉴를 만들어보도록 하겠습니다. 

![image](https://user-images.githubusercontent.com/79847020/187340622-4290b612-78d1-4548-9bc6-3a0c972d5ea9.png)

![image](https://user-images.githubusercontent.com/79847020/187343073-f7a45266-8e62-45e7-970a-5f95daad187d.png)

클릭했을 때 popup되는 메뉴는 어떻게 구현해야할까요 ? 클릭은 javascript도 구현하겠지만 position: absolute; z-index를 적용해서 구현하면 됩니다. 실시간 검색어랑 원리가 같습니다. 

가로세로 규칙에 따라 나누면은 다음과 같이 나눌 수 있을 것입니다. 가로로 나누고 버튼 성격별로 세로영역으로 나눕니다.

![image](https://user-images.githubusercontent.com/79847020/187343582-15d766ab-7349-4173-aebc-34bfd8a13d12.png)

대략적으로 다음과 같은 형태가 될 것입니다.

```HTML
	<div id="main-category">
		<h3 class="blind">주제별 보기</h3>
		<div>
			<button></button>
			<div>
				<button></button>
				<button></button>
			</div>
			<button></button>
			<button></button> 
		</div>
	</div>
```				

버튼들에 이름을 주는 것이 좋습니다. 스크린 리더나 검색엔진이 버튼 역할을 알 수 있도로고 하기위함입니다. 

Naver 또한 그렇게 구현되있는 것을 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/79847020/187343866-055ecbaa-9938-41f8-8af9-81ff2e57e5d0.png)

```HTML
	<div id="main-category">
		<h3 class="blind">주제별 보기</h3>
		<div>
			<button id="subject-all">
				<span class="blind">전체 주제 열기</span>
			</button>
			<div id="subject-list"></div>
			<button id="subject-prev">
				<span class="blind">이전 주제 열기</span>
			</button>
			<button id="subject-next">
				<span class="blind">다음 주제 열기</span>
			</button>
		</div>
	</div>
```			

subject-list에는 	DIB(display: inline-block)를 해도 되고 float:left를 주셔도 됩니다. 왜 float:left를 고려하냐면 \<button\>들이 inline-block이 되는데 inline-block이 좌우로 연속해서 배열될 때 생기는 공간 때문에 처음부터 float:left를 사용해서 구역을 잡아도 무방합니다.

```CSS
#subject-list, #subject-all, #subject-prev, #subject-next {
    float: left;
}
```

그리고 block-format-context를 잡아줍니다.

```HTML
	<div id="main-category">
		<h3 class="blind">주제별 보기</h3>
		<div id="main-category-top">
			<button id="subject-all">
				<span class="blind">전체 주제 열기</span>
			</button>
			<div id="subject-list"></div>
			<button id="subject-prev">
				<span class="blind">이전 주제 열기</span>
			</button>
			<button id="subject-next">
				<span class="blind">다음 주제 열기</span>
			</button>
		</div>
	</div>
```

```CSS
#main-category-top {
	display: inline-block;
	width: 100%
}
```

버튼의 width, height를 설정해주고, border를 설정합니다. 

```CSS
#subject-list, #subject-all, #subject-prev, #subject-next {
    float: left;
    height: 45px;
}

#subject-all, #subject-prev, #subject-next {
    width: 45px;
    background: white;
    broder: none;
    border-left: 1px solid #ebeef3;
}
```

먼저 border로 none을 적용해주고 border-left만 설정해주는 것도 가능합니다. 

이미지 스프라이트를 적용할 이미지를 다운로드 합니다.

```CSS
#subject-all {
    background-image: url(./sp_themecast.png);
    background-repeat: no-repeat;
    background-position: -218px -257px;
}

#subject-prev {
    background-image: url(./sp_themecast.png);
    background-repeat: no-repeat;
    background-position: -320px 0;
}

#subject-next {
    background-image: url(./sp_themecast.png);
    background-repeat: no-repeat;
    background-position: -320px -52px;

}
```

그리고 같은 CSS들을 모아줍니다.

```CSS
#subject-all, #subject-prev, #subject-next {
    width: 45px;
    background: white;
    border: none;
    border-left: 1px solid #ebeef3;
    background-image: url(./sp_themecast.png);
    background-repeat: no-repeat;
}

#subject-all {
    background-position: -218px -257px;
}

#subject-prev {
    background-position: -320px 0;
}

#subject-next {
    background-position: -320px -52px;
}
```
다음과 같이 우리가 의도한 것과는 다르게 렌더링 됩니다.

![image](https://user-images.githubusercontent.com/79847020/187426814-7876c813-f88f-4b9d-a6af-745495ab59d2.png)

width,와 height를 제한해주지 않아 발생한 것 같습니다. 

네이버는 \<button\>안에 일반적으로 2개의 \<span\>태그를 넣어줍니다. 스크린리더용 \<span\>과 이미지용 \<span\>을 넣어주고 있습니다. 

first-child가 있고, nth-child가 있고 last-child가 있습니다. 참고로 nth-of-type도 있기 때문에 first-of-type과 last-of-type도 있습니다. 배경이 되어야 하는 이미지 크기가 제한되있으므로 width와 height를 적용해주어야 합니다. 

```HTML
<div id="main-third-block">
	<div id="main-category">
		<h3 class="blind">주제별 보기</h3>
		<div id="main-category-top">
			<button id="subject-all">
				<span class="blind">전체 주제 열기</span>
				<span></span>
			</button>
			<div id="subject-list"></div>
			<button id="subject-prev">
				<span class="blind">이전 주제 열기</span>
				<span></span>
			</button>
			<button id="subject-next">
				<span class="blind">다음 주제 열기</span>
				<span></span>
			</button>
		</div>
	</div>
	...
```
```CSS
#subject-all, #subject-prev, #subject-next {
    width: 45px;
    border: none;
    border-left: 1px solid #ebeef3;
}


#subject-all span:last-child {
    position: absolute;
    top: 15px;
    left: 12px;
    width: 20px;
    height: 14px;
    background-image: url(./sp_themecast.png);
    background-repeat: no-repeat;
    background-position: -218px -257px;
}

#subject-prev span:last-child {
    position: absolute;
    top: 15px;
    left: 12px;
    width: 20px;
    height: 14px;
    background-image: url(./sp_themecast.png);
    background-repeat: no-repeat;
    background-position: -320px 0;
}

#subject-next span:last-child {
    position: absolute;
    top: 15px;
    left: 12px;
    width: 20px;
    height: 14px;
    background-image: url(./sp_themecast.png);
    background-repeat: no-repeat;
    background-position: -320px -52px;
}
```

![image](https://user-images.githubusercontent.com/79847020/187428871-204cce4a-2a4b-45f1-bd1d-41331017b763.png)

position:absolute;도 적용해주고 있는데 position:absolute;가 적용되어 있으면 containing block을 생각해야 합니다. #subject-all과 #subject-prev, #subject-next가 containong block이 되려면 position: relative;여야 겠죠. 

```CSS
#subject-all, #subject-prev, #subject-next {
	position: relative;
    width: 45px;
    border: none;
    border-left: 1px solid #ebeef3;
	background: white;
}
```

![image](https://user-images.githubusercontent.com/79847020/187429163-3f122f68-394e-4827-bb31-e6496658050c.png)

이제 내부 리스트를 구현해야 하는데 리스트는 항상 \<li\>로 구현합니다.

```
	<div id="subject-list">
		<ul>
			<li>리빙</li>
			<li>푸드</li>
			<li>스포츠</li>
			<li>자동차</li>
			<li>패션뷰티</li>
			<li>부모i</li>
			<li>건강</li>
			<li>웹툰</li>
			<li>게임</li>
			<li>TV연예</li>
			<li>뮤직</li>
			<li>영화</li>
			<li>책문화</li>
		</ul>
	</div>
```
```CSS
#subject-list ul {
    list-style: none;
    margin: 0;
    padding: 0;
}

#subject-list li {
	float:left;
}
```

li는 float:left;를 적용하겠씁니다. DIB인 경우 공간 문제가 발생할 수 있기 때문입니다.

뒤에 더 많은 list들이 숨겨져 있는데 이부분은 javascript 영역입니다. list가 너무 길어서 허용된 공간을 넘어서는 경우가 생기는데 어떻게 처리하는지 보겠습니다. 
