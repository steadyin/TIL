# 7-2. inline-block 총정리와 iframe

다시 한번 살펴보겠습니다. 

세로 정렬을 할 때는 ain-first-block에서 vertical-align: top;을 사용합니다. 

```CSS
#main-first-block {
    display: inline-block;
    width: 100%;
    vertical-align: top;
}
```

자식들의 margin이 있는 경우는 최대한 부모로 옮겨주시는게 좋습니다. 

main-second-block.inline-block의 main-ytn의 margin을 inline-block으로 옮겨줍니다. 

더 이상 inline-block은 inline-block이 아니라 float: left를 적용하므로 클래스도 float: left를 적용합니다.



```CSS
.float-left {
    float: left;
}

#main-second-block {
    display: inline-block;
    vertical-align: top;
    margin-bottom: 8px;
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

#main-newsstand {
    display: inline-block;
    width: 740px;
    height: 246px;
    background-color: white;
    margin-right: 8px;
	/*margin-bottom: 8px;*/
    vertical-align: top;
}

#main-second-ad {
    display: inline-block;
    width: 332px;
    height: 150px;
    background: #555;
    vertical-align: top;
    /*margin-bottom: 8px;*/
}

#main-weather {
    display: inline-block;
    width: 332px;
    height: 142px;
    background-color: white;
    margin-bottom: 8px;
    vertical-align: top;
}

#main-third-block,
#main-fourth-block {
    display: inline-block;
    width: 100%;
    vertical-align: top;
}

#main-third-ad {
    display: inline-block;
    width: 332px;
    height: 130px;
    background: white;
    margin-bottom: 8px;
    vertical-align: top;
}
````

inline-block간의 세로 배열일 때 verticla-align: top입니다.

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


## iframe

main-shopping 부분은 네이버에서 iframe으로 구현했습니다. 

\<frame\>, \<center\>, \<font\>, \<marquee\> 이 태그들은 사용되다가 지금은 사용되지 않는 depreciated 된 태그들입니다.

![image](https://user-images.githubusercontent.com/79847020/185781344-48d92e44-7952-48d9-aa04-99ae594e389f.png)

height, width로 크기를 조절하고 overflow: hidden으로 스크롤을 안생기게 할 수 있습니다.
 
naver에서는 height를 880px인데 코드에서는 900px으로 해야 스크롤이 생기지 않습니다. 이 차이가 무엇때문에 발생하는 것이냐면 스크롤바 때문에 문제가 생기는 것으로 보입니다.

frameborder="0"; scrolling="no";를 적용합니다.

```HTML
...
            <div id="main-third-block">
                <div id="main-category">
                    <h3>주제별 보기</h3>
                </div>
                <div id="main-shopping">
                    <iframe src="https://castbox.shopping.naver.com/shopbox/main.nhn?"
                            frameborder="0"
                            scrolling="no"
                    ></iframe>
                </div>
            </div>
...
```

![image](https://user-images.githubusercontent.com/79847020/185781344-48d92e44-7952-48d9-aa04-99ae594e389f.png)

![image](https://user-images.githubusercontent.com/79847020/185781948-a8ce5c61-2859-42f6-9cdf-60e8d9d1edda.png)



			
