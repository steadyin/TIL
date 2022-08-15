# 6-4 float과 BFC로 해결하기

inline-block 줄내림 현상을 해결하기 위해 다음과 같은 단계를 밟았습니다.

1. inline-block 줄내림 현상이 발생합니다.
2. 해결하기 위해서 자식 태그에 float: left;를 적용해서 해결합니다.
3. float은 둥둥 뜨기 때문에 부모 태그에 자식 태그로 인식하지 못합니다.
4. 부모 태그를 block-format-context로 지정하기 위해 display: inline-block; width: 100%; 합니다.

```HTML
    <main>
        <div id="main-centered" class="center-align">
            <div id="main-first-block">
                <div id="main-ad">
				</div>
                <div id="main-login">
                    <h3>로그인</h3>
                </div>
            </div>
``` 

main-ad에 float: left;를 적용하면 자식의 경우에 display: inline-block이 필요가 없습니다. 대신에 float 자식 태그의 부모 태그에는 display: inline-block; width: 100%; 가 적용되어야 합니다.

```CSS
#main-ad {
    /*display: inline-block;*/
    width: 740px;
    height: 120px;
    background: #555;
    margin-right: 8px;
    margin-bottom: 8px;
    float: left;
    /*font-size: 16px;*/
}

#main-login {
    /*display: inline-block;*/
    width: 332px;
    height: 120px;
    vertical-align: top;
    background-color: white;
    margin-bottom: 8px;
    float: left;
    /*font-size: 16px;*/
}
```

두번째 블록은 main-second-block를 지정하겠습니다.

```HTML
            </div>
            <div id="main-second-block">
                <div id="main-ytn">
                    <h3>연합뉴스</h3>
                    <ol>
                        <li>충격) 제로초 지금 목아파...</li>
                        <li>속보) 방송 종료하고 싶어</li>
                        <li>중대발표: 다음 강좌 언제 될지 모르겠 다</li>
                    </ol>
```

![image](https://user-images.githubusercontent.com/79847020/184631656-6546487b-e01f-4369-bf0f-ea67969d3d04.png)

두번째 블록은 블록 배열 중에서도 구조가 특이한편입니다. 가로로 더 이상 분할하지 못하면 (가로선이 맞지 않기 때문에) 세로로 분할 후 생각해보아야 합니다. 

main-ytn 영역과 main-newsstand 영역을 \<div\>로 구분해주고, main-weather 영역과 main-second-ad 영역을 또 \<div\>로 구분해주겠습니다.

```HTML
            <div id="main-second-block">
				<div class="inline-block">
                    <div id="main-ytn">
                        <h3>연합뉴스</h3>
                        <ol>
                            <li>충격) 제로초 지금 목아파...</li>
                            <li>속보) 방송 종료하고 싶어</li>
                            <li>중대발표: 다음 강좌 언제 될지 모르겠 다</li>
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
                </div>
				<div class="inline-block">
                    <div id="main-weather"></div>
                    <div id="main-second-ad"></div>
                </div>
            </div>
```

가로로 영역을 분할하다가 더이상 가로로 영역을 분할하지 못할 때 세로 영역을 고려 후 다시 가로 영역을 분할하면 됩니다. 세로 영역으로 나눈 후 inline-block으로 지정했습니다. inline-block은 자주 사용되기 때문에 class로 지정해놓는 것을 추천드립니다. 

```CSS
.inline-block {
    display: inline-block;
}
```

우선 main-second-block 영역에서 각 세로 영역, inline-block에 width를 지정합니다. 그리고 정렬이 마지 않으므로 vertical-align까지 적용합니다.

```CSS
#main-second-block .inline-block:first-child {
    width: 740px;

}

#main-second-block .inline-block:last-child {
    width: 332px;
    vertical-align: top;
}
```

![image](https://user-images.githubusercontent.com/79847020/184634586-ab3eb082-d95c-4e52-b609-0322636692f7.png)


![image](https://user-images.githubusercontent.com/79847020/184634638-57f8780e-e2f1-4b23-bca8-a5007339d246.png)


![image](https://user-images.githubusercontent.com/79847020/184634693-107368c8-cbc1-47f2-85d9-cb27c0b33064.png)


![image](https://user-images.githubusercontent.com/79847020/184634747-d34a009e-8b74-4c43-a587-159591f7865f.png)

main-second-block의 영역이 어느 정도 형태를 잡았습니다. main-ytn과 main-newsstand의 content가 정리가 필요해 보입니다.

우선 main-ytn의 h3 부분과 ol 부분이 inline-block으로 지정합니다. CSS를 지정할 때 ,(콤마)를 사용해서 다중 적용할 수 있습니다. 

다만 #main-ytn h3, #main-ytn ol 이런식으로 작성해야 합니다. #main-ytn h3, ol 라고 지정하면 #main-ytn 영역의 h3 태그와 전체 영역의 ol 태그에 적용됩니다. 혼동을 방지하기 위해서 다음과 같이 작성하는 것을 추천드립니다.

```CSS
#main-ytn h3,
#main-ytn ol {
	display: inline-block;
}
```

main-ytn 영역의 한줄 기사는 하나만 남겨두겠습니다.
```
<ol>
	<li>충격) 제로초 지금 목아파...</li>
</ol>	
```	

![image](https://user-images.githubusercontent.com/79847020/184635730-8cf621ac-8e84-4e8e-81c0-f09921905f0d.png)

main-newstand의 \<h3\> 태그 언론사 목록을 45px으로 지정합니다.

```CSS
#main-newsstand h3 {
    height: 45px;
}
```

일반적으로 기본 CSS로 h태그에 CSS가 적용되어 있습니다. 그래서 h태그에 CSS를 초기해주는 것을 권장드립니다. 보통 margin이 적용되어 쓸데없는 공간을 차지하게됩니다.

```CSS
h1, h2, h3, h4, h5, h6 {
    margin: 0;
}
```

경계선을 추가합니다.

```CSS
#main-newsstand h3 {
    height: 45px;
    border-bottom: 1px solid #ebeef3;
}
```

![image](https://user-images.githubusercontent.com/79847020/184638030-a5cbef6c-9a3d-4f99-b168-5edba4093b86.png)

이 부분처럼 가로, 세로 같은 영역들을 배열 할 때는 inline-block으로 나열하면 됩니다. 알아서 배치가 됩니다. 다만 inline-block이기 때문에 앞서 살펴보면 inline-block 여백(?) 문제가 발생합니다. 그래서 float: left;를 적용해서 해결을 하겠습니다.

![image](https://user-images.githubusercontent.com/79847020/184638487-aa4a9786-f91c-412f-9385-1784736ae0f2.png)

브라우저에서 \<ul\>의 기본 CSS로 margin, padding, list-style이 적용되어 있는 것을 알 수 있습니다. 초기화 합니다. list-style은 항목기호입니다.

```CSS
#main-newsstand ul {
    margin: 0;
    padding: 0;
    list-style: none;
}
```

그리고 li에 CSS를 적용합니다.

```CSS
#main-newsstand li {
    float: left;
    width: 123px;
    height: 67px;
    border-right: 1px solid #f1f3f6;
    border-bottom: 1px solid #f1f3f6;
    text-align: center;
    vertical-align: center;
}
```

가운데 정렬은 text-align: center;을 하면 됩니다. 내부 Content가 가운데 정렬됩니다.

.center-align에 margin:0 auto;를 적용했었는데 이건 자식 태그 입장에서 부모 태그 기준으로 가운데 정렬을 하는 것이고 text-align: center;은 부모가 자식한테 가운데 정렬을 하라고 명령을 하는 것입니다.

세로 가운데 정렬은 position으로 하시면 됩니다.  

```CSS
#main-newsstand li img {
    position: relative;
    top: 50%;
    transform: translateY(-50%);
}
```

부모 태그가 li태그이고 li안에 들어있는 img태그에 적용합니다. position: relative;이고 top:50%를 적용해서 자신의 원래 위치보다 50% 아래로 위치시킵니다. transform: translateY(-50%);를 해주어야 합니다. 

그래서 가로 가운데 정렬은 부모 태그에서 text-align으로 적용하고 세로 가운데 정렬은 자식 태그에서 position: relative; top: 50%; trasnform: translateY(-50%);로 해결합니다. 

position: relative; top: 50%;의 기준점은 img의 가장 윗쪽입니다. 따라서 세로 가운데에서 아래쪽에 위치하게 되고 이미지의 height의 절반만큼 다시 transform으로 올려주는 것입니다. translateY(-50%)가 자신의 절반 높이만큼 위로 올려주는 것을 의미합니다. 세로 가운데 정렬 방법이 여러가지가 있지만 강의자는 이 방법을 많이 사용합니다.

가로 가운데 정렬은 부모 태그에서 하고 세로 가운데 정렬은 자식 태그에서 적용했습니다.

![image](https://user-images.githubusercontent.com/79847020/184640957-4eda5b5b-84d2-4efe-8d09-d1a17f8de6ce.png)

h태그의 CSS를 초기화하면서 위치가 변경되서 CSS를 수정합니다.

```
#header-search {
    clear: both;
    padding-top: 45px;
}
```

![image](https://user-images.githubusercontent.com/79847020/184641538-e5944c0f-210d-457b-94c1-920c858834ce.png)

main-category, main-shopping에 margin, padding 적용합니다.

```CSS
#main-category {
    display: inline-block;
    width: 740px;
    height: 837px;
    background: white;
    margin-right: 8px;
    margin-bottom: 8px;
}

#main-shopping {
    display: inline-block;
    width: 332px;
    height: 837px;
    background: white;
    margin-bottom: 8px;
}

#main-event {
    display: inline-block;
    width: 740px;
    height: 130px;
    background: white;
    vertical-align: top;
    margin-right: 8px;
}
```

![image](https://user-images.githubusercontent.com/79847020/184641673-257901e5-6d15-466a-b2d6-da03bdee5699.png)

마찬가지로 inline-block 간의 문제가 발생합니다. inline-block이 여러개 붙으면 4px정도의 margin이 생기는 문제입니다. 해결하기 위해서는 float: left; 적용합니다.

```CSS
#main-category {
    /*display: inline-block;*/
    float: left;
    width: 740px;
    height: 837px;
    background: white;
    margin-right: 8px;
    margin-bottom: 8px;
}

#main-shopping {
    /*display: inline-block;*/
    float: left;
    width: 332px;
    height: 837px;
    background: white;
    margin-bottom: 8px;
}

#main-event {
    /*display: inline-block;*/
    float: left;
    width: 740px;
    height: 130px;
    background: white;
    vertical-align: top;
    margin-right: 8px;
}
```

자식 태그들이 float: left;이며 부모 태그가 자식 태그를 인식하지 못하기 때문에 부모 태그에 display: inline-block;을 적용해줍니다. 필요하면 width까지 지정합니다. 이 방법이 float과 margin colapse 해결 하는 가장 간단한 방법입니다.

```HTML
        <div id="main-centered" class="center-align">
            <div id="main-first-block"> <!--style="font-size: 0"-->
				...
            </div>
            <div id="main-second-block">
				...
            </div>
            <div id="main-third-block">
                <div id="main-category">
                    <h3>주제별 보기</h3>
                </div>
                <div id="main-shopping">
                    <h3>쇼핑</h3>
                </div>
            </div>
            <div id="main-fourth-block">
                <div id="main-event">
                    <h3>이벤트</h3>
                </div>
                <div id="main-third-ad">

                </div>
            </div>
            <h3>뉴스</h3>
            <h3>법률</h3>
            <h3>쇼핑</h3>
        </div>
```

```CSS
#main-third-block,
#main-fourth-block {
    display: inline-block;
    width: 100%;
}
```

지금까지 ID를 많이 사용했는데, 우선 ID를 사용하고 빈번하게 중복이 생기면 추려서 CLASS를 만들면 좋습니다. 태그에서 ID와 CLASS를 같이 사용할 수 있으므로 적절히 활용하시는게 좋습니다.

```
ID, CLASS 동시사용
<div id="main-fourth-block" class="blind">
```

main-centered bottom에도 margin 8px를 지정합니다.

```CSS
#main-centered {
    width: 1100px;
    padding: 8px 10px;
}
```
대략적인 배치가 끝났습니다. 

![image](https://user-images.githubusercontent.com/79847020/184644680-1754ee77-9253-4b30-9804-b6e3d634e3e3.png)
