# 6-3 inline-block의 치명적인 문제

main-ad 영역과 main-login 영역간의 간격이 8px인데 결정해야 할 것이 main-ad의 오른쪽 margin이 8px일 수 있고 main-login 영역의 왼쪽 margin이 8px일 수 있습니다. 이런 것은 개발자가 기준을 정하시면 됩니다. 오른쪽, 아래에 margin을 두는 것을 기준으로 하겠다와 같이 정한 후 통일하시면 됩니다.

main 영역에 알맞게 margin-right와 margin-bottom을 적용합니다.

```CSS
#main-ad {
    display: inline-block;
    width: 740px;
    height: 120px;
    background: #555;
    margin-right: 8px;
    margin-bottom: 8px;
}

#main-login {
    display: inline-block;
    width: 332px;
    height: 120px;
    vertical-align: top;
    background-color: white;
    margin-bottom: 8px;
}

#main-ytn {
    display: inline-block;
    width: 740px;
    height: 46px;
    background-color: white;
    margin-right: 8px;
    margin-bottom: 8px;
}

#main-newsstand {
    display: inline-block;
    width: 740px;
    height: 246px;
    background-color: white;
    margin-right: 8px;
    margin-bottom: 8px;
}

....
```

main-login 영역이 아래로 밀린 것을 확인할 수 있는데, 부모 영역의 width를 초과해서 발생하는 현상입니다.

main-centered 영역의 width가 1080px이고 main-ad 영역의 width가 740px margin-right가 8px, main-login 영역의 width가 332px입니다. 

그런데 740 px + 8 px + 332 px = 1080px 입니다. 초과하지 않았는데 줄내림 현상이 발생했습니다. inline-block간의 간격 문제인데 margin colase 처럼 번거로운 문제입니다. 

![image](https://user-images.githubusercontent.com/79847020/184609725-a3ac0227-750f-4ffc-9a8a-a37d1d6af557.png)

main-ad의 \<\/div\> 태그 뒤에 줄내림이 있는데 이것을 제거하면 main-login 영역의 내림현상이 해결됩니다.

![image](https://user-images.githubusercontent.com/79847020/184609897-5117826d-1921-4e9f-b74e-e42959b47275.png)

![image](https://user-images.githubusercontent.com/79847020/184610165-b765b8f5-208f-4497-879d-529e1366bc20.png)

CSS 코딩할 때 가장 이해하기 힘든 현상인데 inline-block일 때 CSS 소스코드에서 줄여백이 렌더링에 영향을 주는 현상입니다.

그렇다고 CSS를 작성할 때 줄내림 없이 작성하는 것은 안되니, float: left;를 하거나 margin을 -4px을 하기도 합니다. margin은 음수로 지정할 수 있습니다. 음수만큼 겹치게 됩니다. 그런데 브라우저마다 -4px 수치가 다를 수도 있습니다. 

편법 해결방법이 하나 더 언급하면 inline-block을 감싸고 있는 \<div\>에 font-size: 0을 두는 방법이 있습니다. font-size: 0;을 하면 문제가 자식 태그 font-size: 0;이 되기 때문에 자식 태그들에서 font-size를 다시 지정해주어야 한다는 단점이 있습니다.

```HTML
        <div id="main-centered" class="center-align">
            <div style="font-size: 0">
                <div id="main-ad"></div>
                <div id="main-login" >
                    <h3>로그인</h3>
                </div>
            </div>
```
```CSS
#main-ad {
    display: inline-block;
    width: 740px;
    height: 120px;
    background: #555;
    margin-right: 8px;
    margin-bottom: 8px;
	font-size: 16px;
}

#main-login {
    display: inline-block;
    width: 332px;
    height: 120px;
    vertical-align: top;
    background-color: white;
    margin-bottom: 8px;
    float: left;
	font-size: 16px;
}
```

![image](https://user-images.githubusercontent.com/79847020/184612303-9a0df4f3-11cc-47c2-844e-28ce2b31043e.png)

그래서 이 해결방법 중 어떤 해결방법을 선택해야할지 고민을 해야되는데, 추천드리는 것은 float: left;를 추천드립니다. 단점은 float은 둥둥 떠있는 개념이기 때문에 부모 태그가 자식 태그를 인식을 못하게 됩니다. 

```HTML
        <div id="main-centered" class="center-align">
            <div>
                <div id="main-ad"></div>
                <div id="main-login">
                    <h3>로그인</h3>
                </div>
            </div>
```
```CSS
#main-ad {
    display: inline-block;
    width: 740px;
    height: 120px;
    background: #555;
    margin-right: 8px;
    margin-bottom: 8px;
    float: left;
}

#main-login {
    display: inline-block;
    width: 332px;
    height: 120px;
    vertical-align: top;
    background-color: white;
    margin-bottom: 8px;
    float: left;
}
```

![image](https://user-images.githubusercontent.com/79847020/184612303-9a0df4f3-11cc-47c2-844e-28ce2b31043e.png)

![image](https://user-images.githubusercontent.com/79847020/184612664-f4cdaf8a-be07-46e7-8576-8cf17a9264f0.png)

			

float인 자식 태그를 인식하려면, 부모 태그가 block-format-context를 형성하면 됩니다. 부모태그에 overflow:auto;을 적용하면 float인 자식 태그를 인식해서 감싸게 됩니다.

```
        <div id="main-centered" class="center-align">
            <div style="overflow: auto"> <!--style="font-size: 0"-->
                <div id="main-ad"></div>
                <div id="main-login">
                    <h3>로그인</h3>
                </div>
            </div>
```			

![image](https://user-images.githubusercontent.com/79847020/184613012-c6e4f0cb-0a33-4d0b-be7f-6f94267d7424.png)

또는 margin colaspe 해결 방법처럼 부모태그에 display: inline-block; width: 100%;를 지정하면 됩니다. display: inline-block; width: 100%;를 사용하는 것을 권장드립니다. width는 꼭 지정할 필요는 없습니다. 
