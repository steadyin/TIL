# 6-1 마진 상쇄(Margin Collasping)

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
            <h3>로그인</h3>
            <h3>뉴스</h3>
            <h3>법률</h3>
            <h3>쇼핑</h3>
        </div>
    </main>
```

![image](https://user-images.githubusercontent.com/79847020/184478432-18faa1ad-d21c-4323-bf1d-546fbcae1e78.png)

\<main\> 태그안에 \<div\> 태그에 class=center-align; 을 적용했는데 메뉴와 공간이 있는 것을 볼 수 있습니다. \<nav\> 밑에 바로 \<main\>이 있는데 왜 공간이 있는지 이해하지 못할 수도 있습니다. 

개발자 모드에서 확인하면 \<h3\> 태그 부분이 해당 공간을 사용하고 있는 것을 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/79847020/184478575-e42ad27d-6fd7-4476-8a76-7cf5db3338a0.png)	
	
부모태그가 자식태그의 margin을 감싸고 있지않습니다. 이것이 block format context 입니다. \<div\>가 block format context가 아니기 때문에 자식 태그의 영역을 커버하지 못하는 것입니다. \<h3\> 자식태그의 margin이 부모 태그의 영역을 벗어나고, 또 \<main\> 역시 block format context가 아니기 때문에 \<main\> 역시 자식 태그를 감싸지 못하는 것을 확인할 수 있습니다.

이런 현상들 중에서도 margin이 벗어나는 경우를 마진 상쇄(collasping) 현상이라고 말합니다. 

https://developer.mozilla.org/ko/docs/Web/CSS/CSS_Box_Model/Mastering_margin_collapsing

마진 상쇄는 3가지 케이스가 있습니다.

1. 형제간의 마진상쇄

2. 부모에서 First child, Last Child 간의 마진 상쇄

3. 빈 블록의 마진 상쇄

```
    <div style="height:20px;margin-bottom: 20px; background:yellow"></div>
    <div style="height:20px;margin-top: 20px; background:green"></div>
```

2개의 \<div\>가 있는데 하나는 bottom에 20 margin을 두고 하나는 top에 20 margin을 두었습니다. 그럼 40 만큼 margin이 발생해야 합니다. 

![image](https://user-images.githubusercontent.com/79847020/184485654-e3e1f693-c672-4042-aa8e-152984dc4aac.png)

하지만 실제로는 서로 margin이 겹쳐서 20만큼만 margin이 발생합니다. 이것을 마진상쇄라 하고 좌우 margin은 겹치지 않지만 상하 margin은 겹친다는 사실을 잘 알아두셔야 합니다. 형제 태그간의 margin이 상하로 겹치면 작은 margin이 큰 margin과 겹쳐집니다.


따라서 다음과 같이 margin-top:30px;으로 변경하면 다음과 같이 적용됩니다.
```
    <div style="height:20px;margin-bottom: 20px; background:yellow"></div>
    <div style="height:20px;margin-top: 30px; background:green"></div>
``` 

![image](https://user-images.githubusercontent.com/79847020/184525439-4dcbac8e-2eca-4b1b-a682-ee12063a145d.png)


그리고 빈 태그라는 것이 있습니다. height가 없는 태그를 말하는데 height가 없어도 margin만 존재하면 서로 margin을 밀어냅니다. 

![image](https://user-images.githubusercontent.com/79847020/184525530-29ad173c-1524-4d66-9334-94754b5c9c70.png)

content가 존재한다면 두 margin사이에 존재할텐데 content가 없으면 빈 태그에 margin-top과 margin-bottom이 적용되어 margin-top이 margin-bottom 위에 존재하게 됩니다.

mdn에는 다음과 정의되어 있습니다.

![image](https://user-images.githubusercontent.com/79847020/184525589-fce3a50c-7366-4774-b31b-816aad394184.png)

margin이 서로 있음에도 불구하고 무시하고 겹쳐버립니다. height가 들어가 있는 경우네는 형제 블록을 상쇄하게 되고 마지막 케이스가 자식과 부모일 때 상쇄입니다.

```HTML
    <div style="height:20px; margin-bottom: 20px; background:yellow;">
        <div style="height:20px; margin-top: 20px; background:green; "></div>
    </div>
```

자식 \<div\>가 첫번째 자식이면서 마지막 자식이기도 합니다. 이런 경우에도 마진상쇄가 일어납니다. 
  
부모 태그 영역

![image](https://user-images.githubusercontent.com/79847020/184526665-9f8a33b5-ce0c-44ea-a426-08e11828997b.png)

자식 태그 영역

![image](https://user-images.githubusercontent.com/79847020/184526766-36726310-135c-4d0f-a317-c739dd5f9069.png)

부모 태그가 맨 위에 렌더링되야 하는데 그렇지 않습니다. 부모 태그가 첫번째 자식이 margin-top이 있거나 마지막 자식이 margin-bottom이 있는 경우에는 부모 태그를 상쇄해서 적용됩니다. 부모의 영역을 침범해서 margin이 적용됩니다.

마진 상쇄를 완벽하게 이해하고 활용하시면 좋겠지만 솔직히 쉽지 않습니다. 그래서 이 세가지 케이스를 방지하는 방법이 block format context를 만들어주는 것입니다. 

그래서 overflow: auto;를 적용하면 부모 영역이 생기고 자식 영역이 생깁니다. 그런데 스크롤이 생겨서 좋아보이지 않습니다.

```HTML
    <div style="height:20px; margin-bottom: 20px; background:yellow; overflow: auto">
        <div style="height:20px; margin-top: 20px; background:green; "></div>
    </div>
```

![image](https://user-images.githubusercontent.com/79847020/184526820-2fae1ec3-ff82-4b19-8cc4-c7798c4fe303.png)

그래서 block format context를 만드는 또 다른 방법은 overflow: hidden을 적용하는 것입니다. hidden을 적용하면 부모 영역은 생기는데 자식 영역이 부모 영역을 벗어나서 보이지 않습니다. 

이 overflow 방법은 잘 사용하지는 않습니다. 

또 다른 방법으로 float:left;를 사용하고 width:100%;를 사용하는 방법이 있습니다. 그럼 부모 영역도 보이고 자식 영역도 보이게 됩니다. 부모와 자식 영역이 겹치는 현상을 방지할 수 있습니다. 

![image](https://user-images.githubusercontent.com/79847020/184527525-4fe3849c-c69a-40bc-afe9-383b981e4b9e.png)

그 다음 position: absolute;를 사용하는 방법이 있습니다. 참고로 형제 태그일 때도 유효합니다. 

다양한 방법이 있는데, 마진 상쇄 3가지가 있었습니다. 부모 안에 자식이 있는 경우와, 형제 태그인 경우와 빈 태그인 경우. 방금은 형제 태그인 경우에는 float:leaf;  width:100%;으로 해결되지 않았습니다. 

이런 마진 상쇄 3가지를 방지할 수 있는 방법이 있습니다. block format context를 만들면서 3가지 마진 상쇄 케이스를 해결할 수 있는 방법이 있습니다. display: inline-block; width:100%를 주면 됩니다. 

```HTML
    <div style="height:20px; margin-bottom: 20px; background:yellow; display: inline-block; width: 100%;"> </div>
    <div style="height:20px; margin-top: 20px; background:green; "></div>
```
![image](https://user-images.githubusercontent.com/79847020/184528449-e7b57b7a-7959-4789-9a9a-c451c80fa4ae.png)

display: inline-block; width: 100px;를 적용하면 margin collapse를 방지할 수 있습니다.
(width: 100%는 필수는 아닙니다.)

부모 태그일 때를 살펴보겠습니다.

```HTML
    <div style="height:20px; margin-bottom: 20px; background:yellow; display: inline-block; width: 100%;">
        <div style="height:20px; margin-top: 20px; background:green; "></div>
    </div>
```

![image](https://user-images.githubusercontent.com/79847020/184528513-8d0ff7c0-d1ac-4e0d-ba1b-0e5ab0c11de0.png)

부모의 첫번째 자식일 때 margin이 침범하는 것, 형제간의 margin이 겹치는 것, 빈 태그일 때 margin이 겹치는 것를 이용하면 좋고, 이용하기 번거롭다면 display: inline-block, width:100%;로 해결하면 됩니다. 	

![image](https://user-images.githubusercontent.com/79847020/184528643-da3298cc-131d-4658-9f0f-34ac0d562a86.png)
	
자식 연합뉴스 영역도 부모 영역 밖으로 마진 상쇄 현상이 발생하는 것인데 부모에 display: inline-block; width: 100px;를 적용하겠습니다.

![image](https://user-images.githubusercontent.com/79847020/184528700-612c2697-880d-43e5-b37b-52e5b642205f.png)

자식의 margin이 부모 영역 밖으로 더이상 나가지 않습니다.





