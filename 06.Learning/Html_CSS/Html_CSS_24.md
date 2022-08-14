![image](https://user-images.githubusercontent.com/79847020/184528930-15c855d0-eda4-4785-8ad6-2ddf857459a7.png)

안에 각 Content들은 inline-block입니다. 광고판 하나도 block이고 로그인 영역에 나누어야 하겠죠. 각각이 inline-block 입니다. 그래서 layout를 어떻게 설계해야할지 생각을 해봐야합니다. 

![image](https://user-images.githubusercontent.com/79847020/184529757-d905a32a-6eea-4721-b46a-25b2d628a4a1.png)

강의 초반에 가로, 세로 법칙을 알려줬는데, 가로를 먼저 생각하면 다음과 같이 4곳에서 가로 영역을 나눌 수 있습니다.

참고로 inline-block간에는 가로 영역으로 나누지 않아도 되는 경우가 있습니다. 만약 모든 block들이 높이가 같으면 굳이 가로로 영역을 나누지 않아도 inline-block만 배치해서 알아서 다음줄로 넘어갑니다. 지금 문제가 광고 block과 뉴스 block이 서로 높이가 다르기 때문에 가로, 세로 영역을 나누어서 구현하는 것이 좋습니다.

![image](https://user-images.githubusercontent.com/79847020/184530223-2ccd8682-a98c-4a99-aae6-c60290d2cbed.png)

언론사 목록 처럼 높이 같은 것들은 가로-세로 법칙을 사용하지 않고 inline-block들을 쭉 배치를 하면 알아서 정렬이 됩니다. 다른 항목들 역시 마찬가지입니다. 높이가 같다면 영역을 분할하지 않고 배치를 잘하면 됩니다. 

 ![image](https://user-images.githubusercontent.com/79847020/184530311-6c63eccf-c489-4f60-95b6-442911fafefc.png)
 
 광고판과 로그인 영역이 하나의 블록이 되므로 하나의 \<div\>로 지정합니다.
 
 ![image](https://user-images.githubusercontent.com/79847020/184530354-eaa79c77-58f3-463e-894e-eb50724f4cf1.png)
 
 이 부분도 마찬가지로 하나의 block으로 구분합니다.
 
```HTML
 <main>
	<div class="center-align">
		<div>
			<div id="main-ad"></div>
			<div id="main-login">
				<h3>로그인</h3>
			</div>
		</div>
		<div>
			<div id="main-ytn">
				<h3>연합뉴스</h3>
				<ol>
					<li>충격) 제로초 지금 목아파...</li>
					<li>속보) 방송 종료하고 싶어</li>
					<li>중대발표: 다음 강좌 언제 될지 모르겠다</li>
				</ol>
			</div>
			<div id="main-newsstand">
				<h3>언론사 목록</h3>
				<ul>
					<li><img src="./과학동아.png" alt="과학동아" /></li>
					<li><img src="./한국일보.png" alt="한국일보" /></li>
					<li><img src="./osen.png" alt="osen" /></li>
					<li><img src="./매일경제.png" alt="매일경제" /></li>
					<li><img src="./파이낸셜뉴스.png" alt="파이낸셜뉴스" /></li>
					<li><img src="./한국경제TV.png" alt="한국경제TV" /></li>
					<li><img src="./과학동아.png" alt="과학동아" /></li>
					<li><img src="./한국일보.png" alt="한국일보" /></li>
					<li><img src="./osen.png" alt="osen" /></li>
					<li><img src="./매일경제.png" alt="매일경제" /></li>
					<li><img src="./파이낸셜뉴스.png" alt="파이낸셜뉴스" /></li>
					<li><img src="./한국경제TV.png" alt="한국경제TV" /></li>
					<li><img src="./과학동아.png" alt="과학동아" /></li>
					<li><img src="./한국일보.png" alt="한국일보" /></li>
					<li><img src="./osen.png" alt="osen" /></li>
					<li><img src="./매일경제.png" alt="매일경제" /></li>
					<li><img src="./파이낸셜뉴스.png" alt="파이낸셜뉴스" /></li>
					<li><img src="./한국경제TV.png" alt="한국경제TV" /></li>
				</ul>
			</div>
			<div id="main-weather"></div>
			<div id="main-second-ad"></div>
		</div>
		<h3>뉴스</h3>
		<h3>법률</h3>
		<h3>쇼핑</h3>
	</div>
 </main>
```
	
세번째 블록은 주제별보기와 쇼핑입니다. 네번째 블록은 이벤트와 세번째 광고입니다.

```
...
            <div>
                <div>
                    <h3>주제별 보기</h3>
                </div>
                <div>
                    <h3>쇼핑</h3>
                </div>
            </div>
            <div>
                <div>
                    <h3>이벤트</h3>
                </div>
                <div id="main-third-ad"></div>
                </div>
            </div>
...			
```

```CSS
#main-ad {
    display: inline-block;
    width: 740px;
    height: 120px;
    background: #555;
}

#main-login {
    display: inline-block;
    width: 332px;
    height: 120px;
}

#main-ytn {
    display: inline-block;
    width: 740px;
    height: 46px;
}

#main-newsstand {
    display: inline-block;
    width: 740px;
    height: 246px;
}

#main-weather {
    display: inline-block;
    width: 332px;
    height: 142px;
}

#main-second-ad {
    display: inline-block;
    width: 332px;
    height: 150px;
    background: #555;
}
```
![image](https://user-images.githubusercontent.com/79847020/184532282-0bfb1915-5975-44e1-8e3d-74a04d769a75.png)

의도한 것과 다르게 main-ad영역이 main-login영역 아래에 위치해있는 것을 알 수 있습니다. main-ad

![image](https://user-images.githubusercontent.com/79847020/184532322-e789d318-92d0-4023-b237-dc5b19567fd9.png)

이렇게 세로 배치가 안맞을 때는 vertical-align을 활용합니다. 

```
#main-login {
    display: inline-block;
    width: 332px;
    height: 120px;
	vertical-align: top;
}
```
![image](https://user-images.githubusercontent.com/79847020/184532401-c931cd64-2888-47db-84df-6028ecc2244c.png)

나머지 블록도 CSS를 지정합니다. background-color는 사실 background 써서 첫번째 자리에 입력하면 됩니다. 

```CSS
#main-category {
    display: inline-block;
    width: 740px;
    height: 837px;
    background: white;
}

#main-shopping {
    display: inline-block;
    width: 332px;
    height: 837px;
    background: white;
}

#main-event {
    display: inline-block;
    width: 740px;
    height: 130px;
    background: white;
}

#main-third-ad {
    display: inline-block;
    width: 332px;
    height: 130px;
    background: white;
}
```

![image](https://user-images.githubusercontent.com/79847020/184532849-2129b83d-77f8-4877-9722-d5a4afbc9c99.png)

이제 레이아웃을 다 잡은 상태인데, 원하는 대로 나오지 않은 이유는 내부 Content가 부모 밖으로 오버했기 때문입니다. 

![image](https://user-images.githubusercontent.com/79847020/184532905-003b1584-daf8-4a03-8c1b-872c8f34fa4d.png)

이렇게 어긋나는 것들을 앞에서 언급했듯이 vertical-align을 적용합니다. 

그리고 block간의 간격은 margin으로 하는 것이 좋을것 같습니다. block들이 border가 존재하기 때문입니다. border 바깥 부분은 margin이죠. 

![image](https://user-images.githubusercontent.com/79847020/184533020-6dda0f51-268f-469f-9e23-22b2280d74d5.png)

이 간격은 네이버는 main의 padding을 주었습니다. 이 부분은 선택을 할 수 있습니다. 내부 Content의 margin으로 표현할 것인지, 부모 Content의 Padding으로 표현할것인지	선택하시면 됩니다. 자식의 margin이면 margin collapse 문제가 생길 수 있습니다. 그래서 부모의 padding이 더 안전할 수 있습니다. 

```HTML
...
    <main>
        <div id="main-centered" class="center-align">
            <div>
                <div id="main-ad"></div>
                <div id="main-login">
                    <h3>로그인</h3>
                </div>
            </div>
...
```

```CSS
#main-centered {
    padding: 8px 10px 0;
}
```

padding을 적용하니 로그인 영역이 한줄 아래로 내려갑니다. padding이 적용되면서 main-centered 영역보다 main-ad영역과 main-login영역의 width이 초과하면 inline-block를 다음줄로 밀립니다. 자식 Content의 가로 너비가 부모의 가로 너비보다 큰지 확인을 해보셔야 합니다.

![image](https://user-images.githubusercontent.com/79847020/184536226-65358b33-b2c2-421b-972b-991ad2498e38.png)

우리가 작성한 영역은 boxsizing이 border-box여서 padding까지 합쳐서 1080px인데 naver은 padding까지 합쳐서 1100px입니다. border-box로 하나 content-box로 하나 차이입니다. padding, content 너비 계산하는 방법이 border-box일때와 content-box일때가 다르기 때문에 주의해야합니다. 

content-box와 border-box로 둘다 사용하면 혼동이 되므로 우리는 border-box를 사용하고 있으므로 border-box로 진행하겠습니다.

```CSS
#main-centered {
    widht: 1100px;
	padding: 8px 10px 0;
}
```

![image](https://user-images.githubusercontent.com/79847020/184536495-732ad236-befb-4ffb-b624-97e145b1c1aa.png)

\<div\>영역에는 width관련해서 두가지 CSS가 적용되어 있습니다. id, #main-centerd 금메달이고 .center-align이 은메달이라고 했습니다. 그래서 1100px이 적용됩니다.

![image](https://user-images.githubusercontent.com/79847020/184536523-3c143d90-f03f-4258-ab38-fb312ce3a866.png)
