# 8.2 white-space와 opacity

현재는 주제들이 영역을 초과하여 다음과 같이 렌더링 되고 있습니다.

![image](https://user-images.githubusercontent.com/79847020/187441260-dd0efb3b-5f86-41d0-a74d-75e850ef94df.png)

이렇게 다음줄로 내려가는 것을 막으려면 white-space: nowrap;을 적용하고 적당한 height를 적용해서 다음줄이 생길 수 없는 height를 적용해주고 overflow:hidden으로 넘치는 내용을 hidden처리하면 됩니다.

```CSS
#subject-list ul {
    list-style: none;
    margin: 0;
    padding: 0;
    /*width: 1000px;*/
    white-space: nowrap;
    overflow: hidden;
    height: 40px;
}

#subject-list li {
    float:left;
    margin-right: 10px;
    padding: 14px 7px 0;
}
```

![image](https://user-images.githubusercontent.com/79847020/187443035-0a6714c1-9dec-42ea-bf31-f8803ad47ed3.png)

그리고 overflow:hidden;이 없으면 넘치는 내용들이 보이게 됩니다. 그리고 height도 지정해주셔야 합니다. height를 지정하지 않으면 \<ul\>이 content 높이까지 늘어나서 접히는게 자연스러워져버립니다. height를 적당히 지정해주어야만 다음줄 공간이 확보되지 않게되어 줄내림이 생기지 않습니다. 그 다음 width를 지정하면 알아서 content들이 쏙 들어가게 됩니다.

사실, white-space: nowrap;이 맞긴한데 지금 #subject-prev와 #subject-next가 float:left;로 처리되어 있어서 white-space: nowrap; 하나 안하나 차이가 없어지네요. 

정리하면 heigt를 적용하고 white-space: nowrap;을 적용하고 overflow:hidden;을 적용하면 한줄로 퍼진다고 생각하시면 됩니다. 

![image](https://user-images.githubusercontent.com/79847020/187439130-31d71177-fd44-455a-b0d4-5a47df2d1276.png)

그리고 width를 적용해야 가려지는 것 까지 적용됩니다. 왜냐하면 #subject-prev가 위에 있어서 가려지게 되기 때문입니다. 정확히는 부모 영역 바깥으로 튀어나온 자식이 overflow: hidden에 의해 숨겨지게 됩니다.

![image](https://user-images.githubusercontent.com/79847020/187439193-1851a238-79fb-492a-ae4a-0b6d3063af8c.png)

폰트를 적용합니다.

```CSS
#subject-list li {
    float:left;
    margin-right: 10px;
    padding: 14px 7px 0;
    font-weight: 700;
    font-size: 14px;
    font-family: NanumSquare,Dotum,"돋움",Helvetica,"Apple SD Gothic Neo",sans-serif;
}
```

![image](https://user-images.githubusercontent.com/79847020/187443947-8dc8576b-e237-4079-bbc8-ba0130637c48.png)

주제 중에 하나는 하이라이트가 되어 있습니다. 저희는 건강에 하이라이트를 적용하겠습니다.

![image](https://user-images.githubusercontent.com/79847020/187444643-96a18f31-dd9e-4fe7-a0ec-e833cf3149ae.png)

class를 하나 지정해주면 되겠죠.

```HTML
<div id="subject-list">
	<ul>
		<li>리빙</li>
		<li>푸드</li>
		<li>스포츠</li>
		<li>자동차</li>
		<li>패션뷰티</li>
		<li>부모i</li>
		<li class="highlight">건강</li>
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
#subject-list li.highlight {
    bor-bottom: 3px solid #00c73c;
    color: #00b336
	height: 45px;
}
```

font-weight는 기본적으로 400 또는 normal인데 글자 굵기는 얇게 하고 싶다면 300, 200, 100단위로 낮추시면 되고 굵게 하고 싶다면 500, 600, 700, 800, 900을 지정하거나 또는 bolder, lighter을 사용할 수 있습니다. 보통은 100이 얇게, 400이 보통, 700이 굵게할 때 사용한다고 생각하시면 됩니다. 

양끝에 살짝 가리는 효과가 있습니다.(???)

![image](https://user-images.githubusercontent.com/79847020/187446571-3c870c3e-32b4-4608-9a76-3f18e6f4b235.png)

이미지를 활용한 효과입니다.

![image](https://user-images.githubusercontent.com/79847020/187446627-c5d369e9-7394-46a6-98be-3868cc6796dd.png)

그라디언트가 있어서 CSS로도 할 수 있을 것 같긴한데 네이버는 이미지를 사용했습니다. 

영억을 생성합니다.

```HTML
	<div id="subject-list">
		<div class="opacity left"></div>
		<ul>
			<li>리빙</li>
			<li>푸드</li>
			<li>스포츠</li>
			<li>자동차</li>
			<li>패션뷰티</li>
			<li>부모i</li>
			<li class="highlight">건강</li>
			<li>웹툰</li>
			<li>게임</li>
			<li>TV연예</li>
			<li>뮤직</li>
			<li>영화</li>
			<li>책문화</li>
			<div class="opacity right"></div>
		</ul>
	</div>
```						

\<div class=\"opacity left\"\>\<\/div\>는 opacity, left 클래스 2개가 지정됨을 인식하셔야 합니다. opacity(오페이시티)는 불투명도인데 참고로, 쌓임맥락을 형성한다고 나와있었습니다. 

0부터 1까지 숫자를 적어주면 되는데 1이면 불투한것이고 0이면 투명한것이겠죠. opacity는 기본적으로 백그라운드는 투명합니다.

```CSS
.opacity {
    opacity: 0.5;
	background: white;
}

.opacity.left {
    position: absolute;
    left: 0;
    top: 0;
    height: 45px;
    width: 15px;
}

.opacity.right {
    position: absolute;
    right: 0;
    top: 0;
    height: 45px;
    width: 15px;
}
```

그리고 subject-list는 position: relative;를 적용합니다. 그래야 containing block이 됩니다. containing block 기준으로 absolute가 적용되기 때문입니다.

```CSS
#subject-list {
    width: 603px;
    position: relative;
}
```

리빙부분이 opacity가 적용된 것을 알 수 있습니다.

![image](https://user-images.githubusercontent.com/79847020/187449529-2c42f8f4-84ce-4336-a1e8-bdd101c478a0.png)

![image](https://user-images.githubusercontent.com/79847020/187449698-ff1c0618-308e-4fc6-8327-09dc8f2a5d69.png)

그라데이션 효과를 주는 방법도 있는데 gradient, 각자 찾아보시기 바랍니다.


