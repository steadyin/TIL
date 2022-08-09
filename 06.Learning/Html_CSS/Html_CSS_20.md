네이버를 시작페이지로 쥬니어네이버 해피빈 메뉴 영역의 id를 header-top으로 지정합니다.

```HTML
    <div class="center-align">
        <div id="header-top">
            <span>네이버를 시작페이지로</span>
            <span>쥬니어네이버</span>
            <span>해피빈</span>
        </div>
        <div id="header-search">
            <a href="https://www.naver.com">
                <h1>
                    <span>네이버</span>
                    <!-- 주석(comment)입니다 -->
                    <!-- 속성(attribute)입니다 -->
                </h1>
            </a>
            <h2 class="blind">검색창</h2>
            <fieldset>
                <legend class="blind">검색</legend>
                <input/>
                <button>
                    <span class="blind">검색</span>
                    <span id="search-image"></span>
                </button>
            </fieldset>
        </div>
    </div>
```

```
#header-top {
    text-align: right;
}
```

text-align: right; 도 있고 position: absolute; top; right;도 있고 하지만 나는 굳이 float: right;로 하겠다면 이 경우에는 header-top의 다음 형제태그 header-search가 float, header-top의 영역에 침범합니다. 이것을 막으려면 header-top을 block format context로 감싸줘도 되지만 block format context를 사용하지 않겠다면 그 다음 태그에다가 영역이 겹치게 되는 header-search 태그에다가 clear: both; 를 주거나 float:right; 이기 때문에 clar: right; 만 주어도 됩니다.

![image](https://user-images.githubusercontent.com/79847020/180653276-da3e6134-8e3d-4e2b-aa9b-1276561ea45d.png)


float: right;는 clear: right;, clear: both;, float: right, left 모두 해제할 수 있습니다. 

ID와 클래스는 딱히 공식은 없습니다. 팀에서 약속하기 나름이어서 공식은 없는데 id는 두 단어 이상으로 조합하는 것을 권장합니다. 왜냐하면 id는 HTML문서에서 한번만 사용할 수 있는 것이기 때문입니다. 만약 #header라고 하면 이 header라는 단어를 못쓰는 것이거든요. 그러면 단어를 소모해버리기 때문에 아이디는 두 글자이상으로 짓고 클래스는 여러번 동시에 써도 되기 때문에 한 단어 이상으로 명명합니다. 그 다음 \_(언더스코어)로 하냐 -(하이픈)으로 하냐도 명명법이 있는데 회사마다 체계를 갖추고 있을겁니다. 나중에가면 html 문서가 여러개가 생기기 때문에 html문서마다 클래스나 id같은 것들이 겹칠수가 있어서 뒤에다가 #header-search_kakao, #header-search_naver 이런식으로 추가적으로 붙이는 명명법들이 회사마다 존재합니다. 실무에서 특히 si나 에이전시에서 html, css 짜놓은 코드를 보면 규칙이 신기하게 되어있는게 많고요. 그다음에 외계어로 되있는 경우도 많습니다. 외계어는 사람이 지은게 아니라 프로그램이 알아보기쉬운 것으로 바꿔버린 경우입니다. 왜이렇게 하냐면 예를 들어 #header-search는 사람이 의미를 바로 알수 있는데, 의미를 너무 드러나도 문제입니다. 크롤링하는 봇들이 클래스나 아이디가 뉴스이면 content를 긁어가버리는데 그런것들을 막기 위해서 프로그램으로 외계어로 바꾸는 경우가 있습니다. 또는 프로그램으로 주기적으로 계속해서 바꾸기도 합니다. 크롤링에 너무 당하는 사이트는 이런식으로 방지합니다.  

block format context 고려하는 것이 너무 번거롭다. 그냥 float을 사용해서 겹치지 않게 하고 싶다. 그러면 clear:both;를 넣으시면 됩니다. 

 그 다음 block format context 다음으로 설명드릴게 쌓인 맥락(Stacking Context)인데 앞에 두개보다는 중요하지 않아서 필요는 없습니다. 제트 인덱스는 마우스 커서를 올리면 나오는 급상승 검색에서 배울것입니다.
 
![image](https://user-images.githubusercontent.com/79847020/183280860-fa2ef619-2e9c-4bc4-9bf4-e9ffcd1d7c68.png)

급상승 검색어처럼 레이어 위에 표시할 수 있는 것을 제트 인덱스입니다. 위로 올라가고 내려가고가 제트인덱스인데 제트인덱스에도 규칙이 있습니다. 참고로 float은 설명을 할 때 둥둥 뜬다고 설명을 했지만 실제로는 A가 B위에 있다는 개념은 아닙니다. 정상적으로 위에서 아래로 왼쪽에서 오른쪽으로 배치되는 흐름에서 벗어났다. position: absolute 처럼. position: absolute도 위에서 아래로 왼쪽에서 오른쪽으로 배치되는 정상적인 흐름에서 벗어났잖아요. 그것처럼 float: right도 벗어난다는 의미에서 그렇게 표현한것이지 실제로 위에서 겹쳐서 쌓여있다는 개념은 아닙니다. 

실시간 급상승 검색어처럼 표현되는 것이 겹쳐서 쌓여있는 것입니다. 이것이 제트 인덱스인데 여기서 쌓인 맥락에 대해서 알아보겠습니다. 

그래서 블록 포맷 컨텍스트는 부모입장에서 블록 포맷 컨텍스트여야만 자식을 온전히 감쌓을수 있다. 안그러면 자식이 삐져나간다. 특히 float 일 때 이현상을 보게 됩니다. 네이버는 float: absolute;를 많이 사용하는 것 같습니다. float: right;을 왜 안사용하는지는 모르겠지만 float: absolute;를 선호하는 것을 볼 수 있습니다. 방법은 여러가지 이므로 편한 방법으로 사용하시면 됩니다. text-align; 을 사용하든, float을 사용하든.

그런데 position: absolute;가 나은 방법인것 같은게 요즘은 flex를 사용하면서 float을 거의 사용하지 않습니다.(float을 쓸 수 밖에 없는 경우도 있으니 알아야합니다.) 때문에 position: absolute; 방법을 추천을 드립니다. 

![image](https://user-images.githubusercontent.com/79847020/183584996-8f749c11-17fe-495a-a1d7-d7d2593dd15d.png)

그래서 #header-top 의 첫번째 span, first-child를 사용할 때는 조심하라고 했죠. 순서가 바뀔 것 같으면 first-child 보다는 클래스를 하나 붙여주시는게 좋습니다. 

```CSS
#header-top span:first-child {
	color: #888; /*#888888;*/
	letter-spacing: -1px;
	font-size: 11px;
	line-height: 22px;
}
```

처음 배우는게 많은데 color는 글자 색상, #888888이면 #888으로 줄일 수 있습니다. 두글자씩 묶어서 줄이는겁니다. 예를 들어 #aabbcc는 #abc로 줄일 수 있습니다. background-color는 배경색입니다. letter-spacing는 글자간격, 자간입니다. font-size는 글자크기, line-height(라인 하이트)는 다음과 같이 font-size가 커져도 line-height가 고정되어있기 때문에 이 글자의 높이는 22px로 고정됩니다.

![image](https://user-images.githubusercontent.com/79847020/183585893-e92d3dd0-9fe7-4ae7-ba22-212cd1018a14.png)

line-height가 적용되어있지 않으면 font-size의 맞는 height만큼 점유하는데 line-height를 지정하면 해당 px만큼 height가 고정됩니다. 참고로 font-size가 height는 아닙니다. font의 높이를 지정하고 싶다면 line-height를 활용할 수 있습니다. 

![image](https://user-images.githubusercontent.com/79847020/183586110-c87d8c70-2a14-4e9b-a150-28f5e5886120.png)

폰트도 사실 많은 기능들이 있는데 파고들면 내용이 많습니다. 윗부분, 아랫부분, 위로 튀어나가는 것, 밑으로 넘치는 것 등등등이 있는데 나중에 따로 공부하시는 것을 추천드립니다. 

line-height가 중요한게 font가 여러줄이 되었을 때 글자가 위아래로 겹치게 됩니다. 
 
![image](https://user-images.githubusercontent.com/79847020/183586937-82125782-fb20-4f79-a337-2ac379d8dbbf.png)

그 다음 쥬니어네이버는 마우스를 올리면 색상이 바뀌게 됩니다. 

![image](https://user-images.githubusercontent.com/79847020/183587350-e900e65b-f627-439d-b1fa-72cafaa3454b.png)

해피빈도 마찬가지인데, 둘다 글자가 아니다라고 생각하시면 됩니다. 컬러풀하게 바뀌면 보통 글자가 아니라고 생각하시면 됩니다.

둘다 이미지 스프라이트인데, 이미 앞서 설명드렸기 때문에 여기서는 글자로 대체하겠습니다.

![image](https://user-images.githubusercontent.com/79847020/183587831-e76418b7-59d5-4797-bb36-9da6590c8bcb.png)

![image](https://user-images.githubusercontent.com/79847020/183588236-5ea2b43a-dca8-410c-b0be-3ed988396029.png)

그 다음 header-search에다가 clear:both;를 해주어야한다고 했죠.  header-top의 float을 해제하기 위해서는 header-search에 clear: both;를 해줍니다.

```CSS
#header-top {
    float: right;
}
```

![image](https://user-images.githubusercontent.com/79847020/183588876-bf48ee4d-d4c9-4089-82d6-2895cccc5658.png)

그래야 float 공간만큼 header-search 부분이 겹치지 않게됩니다.

