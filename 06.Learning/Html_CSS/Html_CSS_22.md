# 메뉴 따라 만들기, nth-of-type

## 

그 다음 검색창의 키보드 이미지가 오른쪽에 있습니다.

![image](https://user-images.githubusercontent.com/79847020/184320497-84ba4659-3401-4bc8-99c1-565dfc15a6bd.png)

오른쪽에 있다고 하면 div로 감쌓아서 text-align=right;를 하거나 또는 float: right;를 하거나 position: absolute; top, right 같은 방법들이 떠오를겁니다.

지금까지 보시면 position, right를 활용하는 것이 괜찮은 방법이란 것을 알았습니다. 

검색창, 키보드이미지, 화살표가 연속적으로 있습니다.

```HTML
...
	<h2 class="blind">검색창</h2>
	<fieldset>
		<legend class="blind">검색</legend>
		<input/>
		<button>
			<span class="blind">검색</span>
			<span id="search-image"></span>
		</button>
	</fieldset>
...	
```			

```HTML
...
	<h2 class="blind">검색창</h2>
	<fieldset>
		<legend class="blind">검색</legend>
		<input/>
		<span id="search-keyboard"></span>
		<span id="search-history"></span>
		<button>
			<span class="blind">검색</span>
			<span id="search-image"></span>
		</button>
	</fieldset>
</div>
...	
```	

키보드와 화살표 이미지는 sp_search에 있습니다.

![image](https://user-images.githubusercontent.com/79847020/184321700-b9854586-90c1-49b3-989e-b875092aac71.png)

키보드를 클릭하면 이미지 색상이 바뀌는데 javascript로 하는 것이기 때문에 html, css에서 하기 힘듭니다. 그런 유사한 처리를 html, css에서 모두 못하는 것은 아니고 손가락 올렸을 때 바뀌는 것은 html,css만으로도 가능합니다.(CSS문법을 알아야하지만 CSS파일을 작성할 필요는 없습니다.) 몇가지를 제외하고는 javascript도 한다고 생각하시면 됩니다. 마우스 커서를 올려서 바뀌는 것은 html/css로도 가능하다. 그 외 클릭해서 바뀌거나 이런것들은 javascript도 구현한다라고 알고 계셔도 무방합니다.

```CSS
#search-keyboard {
    background-image: url(./sp_search.png);
    background-repeat: no-repeat;
    background-position: -33px -60px;
    width: 19px;
    height: 11px;
    display: inline-block;
    padding: 0 6px;
    vertical-align: middle;
}

#search-history {
    background-image: url(./sp_search.png);
    background-repeat: no-repeat;
    background-position: -87px -60px;
    width: 9px;
    height: 4px;
    display: inline-block;
    padding: 0 6px;
    vertical-align: middle;
}
```

vertical-align(emmet vm)은 inline-block이나 inline, 옆에 붙어 있는 컨텍스트들을 정렬을 해줍니다.  block은 안됩니다. vertical-align은 사용하지 않으면 틀어지게 됩니다. 

padding: 0 6px;은 padding: 0 6px 0 6px;와 같은 의미입니다. 위,오른쪽과 아래,왼쪽이 수치가 반복되면 줄여사용할 수 있습니다. 위아래가 0, 좌우가 6px이라고 생각하시면 됩니다. 

padding: 0 6px 15px; 같은 경우는 위, 우좌, 아래라고 생각하시면 됩니다. 

```HTML
            <li><a href=""><span></span><span class="blind">카페</span></a></li><!--list item-->
            <li><a href=""><span></span><span class="blind">블로그</span></a></li><!--list item-->
            <li><a href=""><span></span><span class="blind">지식인</span></a></li><!--list item-->
            <li><a href=""><span></span><span class="blind">네이버쇼핑</span></a></li><!--list item-->
            <li><a href=""><span></span><span class="blind">네이버페이</span></a></li><!--list item-->
            <li><a href=""><span></span><span class="blind">네이버TV</span></a></li> <!-- list item -->
            <li><a href=""><span></span><span class="blind">사전</span></a></li> <!-- list item -->
            <li><a href=""><span></span><span class="blind">뉴스</span></a></li> <!-- list item -->
            <li><a href=""><span></span><span class="blind">증권</span></a></li> <!-- list item -->
            <li><a href=""><span></span><span class="blind">부동산</span></a></li> <!-- list item -->
            <li><a href=""><span></span><span class="blind">지도</span></a></li> <!-- list item -->
            <li><a href=""><span></span><span class="blind">영화</span></a></li> <!-- list item -->
            <li><a href=""><span></span><span class="blind">뮤직</span></a></li> <!-- list item -->
            <li><a href=""><span></span><span
			class="blind">책</span></a></li> <!-- list item -->
            <li><a href=""><span></span><span class="blind">웹툰</span></a></li> <!-- list item -->
```

## nth of type 

```html
    <main>
        <div class="center-align">
            <h3>연합뉴스</h3>
            <ol>
                <li>충격) 제로초 지금 목아파...</li>
                <li>속보) 방송 종료하고 싶어</li>
                <li>중대발표: 다음 강좌 언제 될지 모르겠다</li>
            </ol>
            <h3>언론사 목록</h3>
            <ul>
```

nth of type은 \<div\>의 자식태그 중 \<h3\>언론사 목록\<\/h3\>이 몇번째 자식인지 생각해봅시다.

```CSS
main h3:nth-child(3) {
    background: red;
}
```

nth-child를 적용시에 3번째 자식 태그로 인식합니다.

![image](https://user-images.githubusercontent.com/79847020/184476196-8bd91648-59fd-480c-89db-5462a6946585.png)

두번째 \<h3\>태그이므로 2번째 nth-child로 인식할 것 같지만 3번째로 인식합니다. 중간에 \<ol\>같은 태그가 있으면 자식으로 인식되서 CSS에 h3를 지정했음에도 nth-child는 2가 아니라 3번째로 인식합니다. 

그런데 nth-of-type을 사용하면 h3태그만 인식해서 동작됩니다.  

```CSS
main h3:nth-of-type(2) {
    background: red;
}
```

![image](https://user-images.githubusercontent.com/79847020/184476674-5f527032-db64-4ed7-8987-2f6cf4b3cb9f.png)

nth-child로 하면 세번째이고, nth-of-type이면 두번째입니다. 보통은 nth-of-type을 더 많이 활용하는 편입니다.



