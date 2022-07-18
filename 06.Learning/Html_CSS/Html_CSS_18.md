이어서 네이버는 두번째 메뉴부터는 padding이 적용되어 있습니다.

first-child를 제외한 nth-child 전체 CSS에 padding-left를 적용해주어도 좋지만 first-child 때문에 공통적으로 묶지 못하고 하나하나 적어주는 셈입니다.

```CSS
nav li {
    display: inline-block;
}

nav li span {
    background-image: url(./sp_nav.png);
    background-repeat: no-repeat;
    display: inline-block;
    height: 16px;
}

nav li:first-child span:first-child {
    width: 25px;
    background-position: 0 -285px;
}

nav li:nth-child(2) span:first-child {
    width: 27px;
    background-position: -279px -52px;
    padding-left: 14px;
}

nav li:nth-child(3) span:first-child {
    width: 40px;
    background-position: -100px -182px;
    padding-left: 14px;
}
nav li:nth-child(4) span:first-child {
    width: 40px;
    background-position: -101px -156px;
    padding-left: 14px;
}
```

boxsizing border box 때문에 위치가 이렇게 됐습니다.

![image](https://user-images.githubusercontent.com/79847020/179539130-18b615a7-709e-43a1-9994-94d08e790f7f.png)

border-box면 padding이랑 content랑 합쳐서 25가 되는거죠.

이 경우에는 boxsize borderbox를 contentbox로 다시 바꿔주어야 합니다.

아니면 마진으로 바꾸든지..

```CSS
nav li:nth-child(2) span:first-child {
    width: 27px;
    background-position: -279px -52px;
    margin-left: 14px;
}
```

![image](https://user-images.githubusercontent.com/79847020/179539430-bd5589fb-ec31-4412-87c8-7499abd824e3.png)

![image](https://user-images.githubusercontent.com/79847020/179539553-7e7d3cec-b2b6-43fd-9135-b17a4fa09bdf.png)

간격이 떨어지는데 문제는 첫번째 것은 마진이 없는데 그렇다고 나머지 전체에 margin을 주는 것은 비효울적입니다.

그럴 경우에는 공통에 margin-left를 주고 first에만 margin-left:0; 을 설정하면 됩니다.

```CSS
nav li {
    display: inline-block;
}

nav li span {
    background-image: url(./sp_nav.png);
    background-repeat: no-repeat;
    display: inline-block;
    height: 16px;
    margin-left: 14px;
}

nav li:first-child span:first-child {
    width: 25px;
    background-position: 0 -285px;
    margin-left: 0;
}

nav li:nth-child(2) span:first-child {
    width: 27px;
    background-position: -279px -52px;
}

nav li:nth-child(3) span:first-child {
    width: 40px;
    background-position: -100px -182px;
}
```

![image](https://user-images.githubusercontent.com/79847020/179540055-935a3740-eff9-42cd-9368-1bfec64b86ea.png)

다음과 같이 margin-left: 14px; 가 취소되어 있는 것을 확인할 수 있는데

![image](https://user-images.githubusercontent.com/79847020/179540646-be855a01-e7b5-4bb9-8659-91d27212c0d8.png)

이것을 캐스캐이딩이라고 합니다. 말그대로 덮어씌운거죠. 

덮어씌우는 기준을 잘 알아야 되는데 CSS에는 우선순위가 있습니다.

https://www.zerocho.com/category/CSS/post/588cb95ca63e64132496a5d5

* 기본적으로 뒤에 나오는 css가 우선순위가 높습니다. 

* !important > inline style attribute > id > class, 다른 attribute, 수도클래스(:first-child같은 것) > tag element, 수도엘레먼트(::before같은 것) 순으로 우선순위가 높습니다.

* 우선순위가 같다면 개수가 많은 css가 우선순위가 높습니다.

기본적으로 동등한 레벨이면 뒤에 나오는 CSS가 우선순위가 높습니다. 그리도 위에 경우는 두번째 규칙에 해당되는데 외우셔야 합니다. 

important > inline > id > class > attribute > tag

그래서 점수를 이렇게 매길 수 있습니다. 

태그들을 동메달이라고 치면 다음은 동메달을 3 딴 것입니다.

```CSS
nav li span {
    display: inline-block;
}
```

다음은 태그와 ID가 있어서 동메달 1개 은메달 1개입니다.

```CSS
nav .nav-item-mail {

}
```

은메달이 한국식으로 순위를 매기면 동메달 3개보다 순위가 높으므로 덮어씌우게 됩니다.

아이디가 금메달, 클래스가 은메달, 태그가 동메달이라고 생각하시면 됩니다.

다음은 태그가 3개, 동메달 3개, 클래스가 1개여서 은메달 1개가 됩니다. (first-child도 은메달???) 

```CSS
nav li:first-child span:first-child {
    width: 25px;
    /*height: 16px;*/
    background-position: 0 -285px;
    /*background-image: url(./sp_nav.png);*/
    /*background-repeat: no-repeat;*/
    /*display: inline-block;*/
    margin-left: 0;
}
```

결국은 핵심은 다음을 잘 알고있어야 합니다.

* 기본적으로 뒤에 나오는 css가 우선순위가 높습니다. 

* !important > inline style attribute > id > class, 다른 attribute, 수도클래스(:first-child같은 것) > tag element, 수도엘레먼트(::before같은 것) 순으로 우선순위가 높습니다.

* 우선순위가 같다면 개수가 많은 css가 우선순위가 높습니다.

꼼수가 하나 있는데 

```CSS
nav li:first-child span.nav-item-mail {

}

nav li:first-child span.nav-item-mail {

}
```

다음가 같은 CSS속성이 있을 때 메달개수가 같으므로 아래것이 적용되는데 위에 CSS를 우선순위를 더 높게하고 싶다면 순서를 바꾸는 됩니다.

또는 다음과 같이 꼼수로 같은 클래스를 한번더 붙여 우선순위를 높이는 방법도 있습니다.

```CSS
nav li:first-child span.nav-item-mail.nav-item-mail {

}

nav li:first-child span.nav-item-mail {

}
```

참고로 first-child는 의사(pseudo, 수도) 클래스고, 수도클래스라 부르기도 하며 은메달 레벨을 갖고 있습니다. 수도 클래스는 기본 태그들을 꾸며주는 역할을 하는데 꾸며주는게 하나라도 더 붙어 있으면 우선순위가 더 높습니다.

```CSS
nav li span:first-child {
}

nav li:first-child span:first-child {
}
```

기본적으로 뒤에 있는게 먼저 적용되고 그 다음에 important lnline id class 다른attribute 일반tag 순서로 메달 색깔이라고 생각하면 되고 그것마저도 같으면 갯수를 세서 많은 것이 우선순위가 높다.

important는 치트키 같은 개념이여서 가장 최우선으로 적용됩니다.
```CSS
nav li span {
    background-image: url(./sp_nav.png);
    background-repeat: no-repeat;
    display: inline-block;
    height: 16px;
    margin-left: 14px !important;
}
```

!important;를 너무 남발하면 이해하기 힘든 CSS가 되므로 최후의 수단으로 사용하는 것이 좋습니다. 차라리 위에서 언급한 클래스를 여러번 붙이는 꼼수같은 것을 사용해서 우선순위를 높이는 것이 더 나은 방법입니다.


