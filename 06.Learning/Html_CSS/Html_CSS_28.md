# 7-2. inline-block 총정리와 iframe

다시 한번 살펴보겠습니다. main-first-block에서 vertical-align: top;을 하고 자식들의 margin이 있는 경우는 최대한 부모로 옮겨주시는게 좋습니다. 

main-second-block.inline-block의 main-ytn의 margin을 inline-block으로 옮겨줍니다. 

더 이상 inline-block은 inline-block이 아니라 float: left를 적용하므로 클래스도 float: left를 적용합니다.



```CSS
.float-left {
    float: left;
}


#main-second-block .float-left:first-child {
    width: 740px;
    margin-right: 8px;
    vertical-align: top;
}

#main-second-block .float-left:last-child {
    width: 332px;
    vertical-align: top;
}

#main-ytn {
    display: inline-block;
    width: 740px;
    height: 46px;
    background-color: white;
    margin-bottom: 8px;
}
````

```HTML
	<div id="main-second-block">
		<div class="float-left">
			<div id="main-ytn">
				<h3>연합뉴스</h3>
				<ol>
					<li>충격) 제로초 지금 목아파...</li>
				</ol>
			</div>
			<div id="main-newsstand">
				<h3>언론사 목록</h3>
				<ul>
					<li><img src="./과학동아.png" alt="과학동아" /></li>
...생략
				</ul>
			</div>
		</div>
		<div class="float-left">
			<div id="main-weather"></div>
			<div id="main-second-ad"></div>
		</div>
	</div>
```			

main-newstand의 margin이 부모를 뚫고 나가는 것도 최대한 지양합니다. 

```
#main-newsstand {
    display: inline-block;
    width: 740px;
    height: 246px;
    background-color: white;
	margin-right: 8px;
}

#main-second-ad {
    display: inline-block;
    width: 332px;
    height: 150px;
    background: #555;
}




