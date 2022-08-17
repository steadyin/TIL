# calc와 inline-block 아래 공간 문제

지난 시간에 main-newsstand 작업을 inline-block 문제 때문에 작업을 중단했습니다. inline-block 간의 간격에 여백이 발생하면서 그 문제를 해결하기 위해 float: left;를 사용해서 해결한다고 했습니다. float: left; 를 사용하면서 또 발생하는 문제가 containing block에서 벗어나는 문제가 발생하고 containing block을 지정하기 위해서는 overflow: hidden; 이나 display:inline-block; 을 사용합니다. 그런데 display: inline-block;을 사용하면 또 그 문제가 발생해서 float: left;로 바꿔주고 부모를 display: inline-block;으로 바꿔주어야 하는 문제가 반복해서 발생합니다.

그래서 inline-block; 연달아 있는 경우에는 float: left;를 지정해주는 것이 좋습니다. 

그리고 더 좋은 방법은 Internet Explorer를 지원할 필요가 없다면 flex 기법이라고 새로운 기법이 있는데 마찬가지 문제를 해결할 수 있습니다. 

![image](https://user-images.githubusercontent.com/79847020/185105490-8e667232-63e5-48b5-9390-54ece8743e21.png)

간격들을 8px로 지정했지만 일정해 보이지 않습니다. 

오늘은 이런 간격 문제를 해결하고 Z Index를 사용해서 실시간 검색어까지 표현해보겠습니다.

![image](https://user-images.githubusercontent.com/79847020/185108812-b194ba3b-3dca-48ef-9658-544b646aa650.png)

먼저 \<ul\> 태그를 살펴보면 740 x 0 입니다. 왜 높이가 0이냐면 \<li\> 들이 모두 float: left;가 적용되어 있기 때문입니다. float: left;를 적용하면 containing block에서 벗어나기 때문에 자식 태그들을 감싸지 못합니다. 이럴 때는 앞서 언급했뜻이 \<ul\> 태그를 display: inline-block;를 적용합니다.

![image](https://user-images.githubusercontent.com/79847020/185109712-4c474cf9-9fed-40e6-a8f5-af53b8c0bf97.png)

 display: inline-block;는 연달아 사용하는 경우게 문제가 된다고 했죠. 그래서 display: inline-block;을 사용할 때는 연달아 사용하고 있는지 확인을 하시는 것이 좋고, 만약 연달아 사용하고 있다면 float으로 inline-block 문제를 해결하셔야 합니다. 
 
 \<ul\>태그의 너비가 740px인데 \<li\>가 6개가 연속해서 배치되는데 740/6=123.3333 입니다. 우리는 \<li\>의 width:123px;이므로 지정한 간격보다 간격이 더 크게 발생합니다. \<li\>의 width를 123.3333px로 지정할수도 있겠지만 이 떄는 calc를 활용하시면 됩니다. width: calc(100%/6);를 적용하면 740을 6으로 나눈셈입니다. 
 
 calc를 사용할 때 주의해야 할 점이 calc(100% / 6) 뛰어쓰기를 꼭 해주셔야 합니다. \-를 사용할 때 문제가 생길 수 있습니다. \-일 때 뛰어쓰기를 안하면 \-를 인식 못하는 문제가 발생하기 때문에 calc를 사용할 때는 습관적으로 뛰어쓰기를 해주는시는 것을 권장드립니다. 
 
 ```CSS
 #main-newsstand ul {
    display: inline-block;
    margin: 0;
    padding: 0;
    list-style: none;
}

#main-newsstand li {
    float: left;
    width: calc(100% / 6);
    height: 67px;
    border-right: 1px solid #f1f3f6;
    border-bottom: 1px solid #f1f3f6;
    text-align: center;
    vertical-align: center;
}
```

inline-block이 연속적으로 사용되면 가로 간격만 틀어지는 것이 아니라 세로 간격도 틀어집니다.

main-first-block, main-second-block 모두 inline-block이기 때문에 세로로 배열되어있지만 간격을 검토해봐야합니다. 세로간의 문제는 일단 vertical-align: top;를 적용합니다. 

inline-block의 횡 배열의 간격 문제는 float: left;로 해결하고 종 배열의 간격 문제는 vertical-align: top;으로 해결한다. 그것도 안되면 float: left;로 해결할 수도 있는데 대부분의 문제가 verticla-align: top;으로 해결됩니다.

![image](https://user-images.githubusercontent.com/79847020/185127113-c59f815f-390b-41b5-aa8f-258054c3fbe7.png)

노란색으로 하이라이트한 세로 간격이 벌어진 것을 확인할 수 있습니다. main-first-block이 inline-block라서 벌어진 것입니다. inline-block 끼리 종으로 연속적으로 있을 때 간격 문제가 생기면 vertical-align: top;을 적용합니다.

```CSS
#main-first-block {
    display: inline-block;
    width: 100%;
	vertical-algin: top;
}
```

![image](https://user-images.githubusercontent.com/79847020/185128312-5b9fc4f6-6b63-4119-88e9-73b26280ac32.png)

자세히 보면  오른쪽 margin과 main-weather영역이 겹쳐있는 것을 알 수 있

```HTML
            <div id="main-second-block">
                <div class="inline-block">
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
							....생략
                        </ul>
                    </div>
                </div>
                <div class="inline-block">
                    <div id="main-weather"></div>
                    <div id="main-second-ad"></div>
                </div>
            </div>
```			

main-second-block의 inline-block(main-ytn, main-newsstand)과 inline-block(main-weather, main-second-ad)의 가로 간격 문제입니다.

```CSS
#main-second-block .inline-block:first-child {
    width: 740px;
    float: left;
    margin-right: 8px;

}

#main-second-block .inline-block:last-child {
    width: 332px;
    vertical-align: top;
    float: left;
}
```

부모 태그에 display: inline-block;을 적용합니다.

```CSS
#main-second-block {
    display: inline-block;
    width: 100%;
}
```

![image](https://user-images.githubusercontent.com/79847020/185138751-53491d20-18bb-425c-97f5-236c728b118d.png)

상단의 간격은 이제 맞습니다.

![image](https://user-images.githubusercontent.com/79847020/185139593-49c56ade-921b-4af6-a5cc-f7a699154300.png)

main-weather 영역과 main-second-ad 세로 간격 문제는 main-weather에 vertical-align: top;을 적용합니다.

```CSS
#main-weather {
    display: inline-block;
    width: 332px;
    height: 142px;
    background-color: white;
    margin-bottom: 8px;
    vertical-align: top;
}
```

![image](https://user-images.githubusercontent.com/79847020/185141754-18d66bac-9109-4056-9d1d-2f3a8cbdeb04.png)

main-newsstand 영역과 main-category 영역의 세로 간격이 벌어진 것을 알 수 있습니다. 

![image](https://user-images.githubusercontent.com/79847020/185142858-5b968c58-cb67-42e4-be41-fa0db230f1b9.png)

![image](https://user-images.githubusercontent.com/79847020/185142998-1bd44909-a84c-428b-b132-343e801cd27d.png)

main-second-block의 inline-block과 inline-block의 높이 차이가 나는 것을 확인할 수 있습니다. 두번째 inline-block이 float: left; 되면서 block format context를 생성하고 자식의 margin 마저 감싸서 자식의 margin에다가 부모의 margin을 더해서 margin이 2배가 됩니다. 이런 상황 때문에 마진 상쇄에 대해서 잘 알아야 되는데 자식의 margin-bottom을 제거하고 부모에게 margin-bottom을 적용하겠습니다. 

자식 태그에 margin이 적용되어 있으면 부모 태그 밖으로 나올려는 현상 때문에 혼동이 오기 때문에 자식 태그에는 최대한 부모와 경계선에 margin을 두지 않는 것을 추천드립니다.

```
#main-second-ad {
    display: inline-block;
    width: 332px;
    height: 150px;
    background: #555;
    margin-right: 8px;
    /*margin-bottom: 8px;*/
}
```

![image](https://user-images.githubusercontent.com/79847020/185145642-539efae1-9226-4a3e-8211-b9e5ca30f84a.png)

레이아웃 간격들이 8px로 맞춰진 것을 확인할 수 있습니다.

```CSS
#main-third-block,
#main-fourth-block {
    display: inline-block;
    width: 100%;
    vertical-align: top;
}
```

이런 문제들이 important로 해결할 수 있는 문제는 아닙니다. display: inline-block;의 문제나 containing block, block format context 것들이 연관되기 때문에 다 조합을 해서 생각해야 해결을 할 수 있습니다.

그런데 만약 인터넷 익스플로러 지원이 필요 없다면 flex로 해결하는 방법이 가장 간단합니다. 


