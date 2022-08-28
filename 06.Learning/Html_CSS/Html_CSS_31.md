# 7-5 form의 button과 a태그의 차이

로그인 부분을 완성하겠습니다.

![image](https://user-images.githubusercontent.com/79847020/187065400-8b910a57-664b-4b57-9132-249bb3905c46.png)   

\<a\>안에 \<btton\>을 많이 넣으시는데 그렇게 안하는 것이 좋습니다. 버튼은 클릭할 수 있다고해서 다 버튼이 아니라 클릭되면 그에 대한 동작을 하는것입니다. 링크는 다른 페이지로 이동하는 것입니다. 다른 페이지로 이동하는 것은 버튼으로 만들지 마시고 \<a\>로 만드는 것은 권장드립니다.

버튼은 마우스를 올려도 손가락 모양으로 변하지 않고 화살표 모양 그대로 입니다. \<a\>태그는 손가락 모양으로 마우스 커서 모양이 바뀝니다. 

그래서 \<a\>태그와 \<button\>을 같이 사용하지 말고 구분해서 사용합시다.

왜 네이버의 돋보기 모양 버튼은 다른 페이지를 가는데도 버튼인지 따로 알려드리겠습니다.

```HTML
<div id="main-centered" class="center-align">
	<div id="main-first-block"> <!--style="font-size: 0"-->
		<div id="main-ad"></div>
		<div id="main-login">
			<h3 class="blind">로그인</h3>
			<div class="login-wrapper">
				<span class="login-text">네이버를 더 안전하고 편리하게 이용하세요.</span>
				<a href="https://nid.naver.com/nidlogin.login?mode=form&amp;url=https%3A%2F%2Fwww.naver.com">
					<span class="blind">NAVER 로그인</span>
					<span class="login-image"></span>					
				</a>
			</div>
		</div>
	</div>
```
```CSS
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

#main-login .login-wrapper {
    padding: 15px 25px;
}

#main-login .login-wrapper a {
    display: block;
    margin-top: 12px;
}

#main-login .login-text {
    color: #888;
    font-size: 12px;
    line-height: 14px;
}

#main-login .login-image {
    background: url(./sp_login.png) 0 -47px no-repeat;
    display: inline-block;
    width: 280px;
    height: 37px;
}
```

NAVER 로그인은 버튼인 것처럼 보이지만 버튼이 아니라 a태그입니다. 마우스를 올리면 손가락 모양으로 바뀝니다. a태그라는 것이죠.

![image](https://user-images.githubusercontent.com/79847020/187068873-c7533206-cb17-449b-92d7-f5aa5db533ad.png)

다음 background-position은 2줄을 1줄로 작성할 수 있습니다.
```
#main-login .login-image {
    background: url(./sp_login.png);
    background-postion: 0 -47px no-repeat;
	display: inline-block;
    width: 280px;
    height: 37px;	
}
```
```
#main-login .login-image {
    background: url(./sp_login.png) 0 -47px no-repeat;
    width: 280px;
    height: 37px;

}
```			

![image](https://user-images.githubusercontent.com/79847020/187068860-e9a6ca97-b6de-4562-a17d-5f9793a45288.png)

아이디 비밀번호 찾기 모두 손가락 모양으로 바뀌는 a태그입니다.

![image](https://user-images.githubusercontent.com/79847020/187068905-cd491f5c-8ee2-4932-95db-632e00dba3de.png)

아이디와 비밀번호 부분은 따로 볼게 없을 것 같고 회원가입을 만들어보겠습니다..

```HTML
<div class="login-wrapper">
	<span class="login-text">네이버를 더 안전하고 편리하게 이용하세요.</span>
	<a href="https://nid.naver.com/nidlogin.login?mode=form&amp;url=https%3A%2F%2Fwww.naver.com">
		<span class="blind">NAVER 로그인</span>
		<span class="login-image"></span>
	</a>
	<a class="login-signup" href="https://nid.naver.com/nidregister.form?url=https%3A%2F%2Fwww.naver.com">
		회원가입
	</a>
</div>
```

![image](https://user-images.githubusercontent.com/79847020/187069225-62369574-ab2b-4256-a297-15ea8ff30be3.png)

CSS는 float: right해서 오른쪽으로 보내주고 a태그의 기본 스타일이 있어서 기본스타일을 없애주어야 합니다. 밑줄은 text-decoration을 사용합니다. 

```CSS
#main-login .login-signup {
    float: right;
    font-size: 12px;
    font-height: 38px;
    color: #666;
    text-decoration: none;
}
```

![image](https://user-images.githubusercontent.com/79847020/187069292-0c040fc7-5a01-44da-bd9b-6fb827ac31b1.png)

또 CSS 겹치는 현상이 발견되서 수정하겠습니다. 

![image](https://user-images.githubusercontent.com/79847020/187069336-1f15a04b-46a4-4417-aff9-ef13d346ff1b.png)

```CSS
#main-login .login-wrapper a {
    display: block;
}

#main-login .login-signup {
    float: right;
    font-size: 12px;
    font-height: 38px;
    color: #666;
    text-decoration: none;
    margin-top: 12px;
}
```

![image](https://user-images.githubusercontent.com/79847020/187069400-b701fdfc-24f5-4354-9916-629c778344b3.png)

Naver를 살펴보면 회원가입에 손을 올렸을 때 밑줄이 갑니다.
 
이럴 때는 hover를 이용합니다.

```
#main-login .login-signup:hover {
    text-decoration: underline;
}
```

![image](https://user-images.githubusercontent.com/79847020/187069499-a0fd8154-a46a-4177-b8ae-881a11197f49.png)

마지막으로 분명 네이버는 검색 버튼 눌렀을 때 다른 페이지로 이동하는데 \<a\>태그가 아니라 \<button\>을 사용했을까를 알아야 합니다.

![image](https://user-images.githubusercontent.com/79847020/187069555-739d998a-448b-47ee-8cc3-b04cb93343e1.png)

여기서 form이 나오게 됩니다. 검색창은 form. 이렇게 사용자가 입력하는 공간을 \<form\>이라는 태그로 감쌓을 수 있습니다. 

그리고 form에는 action옵션을 줄 수 있습니다. action에다가 주소를 넣을 수 있습니다. 사용자가 입력한 내용을 action 주소로 보낸다는 의미로도 볼 수 있습니다. 

form안의 들어있는 button은 눌렀을 때 action페이지로 이동하게 역할을 한다. 그런데 form안에 있는 button을 눌러도 이동하지 않게 하고 싶으면 type="button"옵션을 설정하면 action으로 이동하지 않습니다. action동작을 하고 싶지 않으면 type옵션을 주지 말던가, type="submit"을 주면 됩니다. 

```
<div id="header-search">
	<a href="https://www.naver.com">
		<h1>
			<span>네이버</span>
			<!-- 주석(comment)입니다 -->
			<!-- 속성(attribute)입니다 -->
		</h1>
	</a>
	<h2 class="blind">검색창</h2>
	<form class="inline-block" action="https://search.naver.com/search.naver?sm=top_hty&fbm=0&ie=utf8&query=">
		<fieldset>
			<legend class="blind">검색</legend>
			<input/>
			<span id="search-keyboard"></span>
			<span id="search-history"></span>
			<button>
				<span class="blind">검색</span>
				<span id="search-image"></span>
			</button>
		</fieldset>
	</form>
</div>
```			

이제 입력을 하고 버튼을 클릭하면 검색페이지로 이동합니다.

![image](https://user-images.githubusercontent.com/79847020/187069786-8db50a39-249c-47f1-967a-3cd49ce3173f.png)

![image](https://user-images.githubusercontent.com/79847020/187069811-43975dcd-58fe-42ec-8ffa-d34feac26e2f.png)

form일 경우는 특수하다고 생각하시면 될 것 같아요. 검색 버튼은 다른 페이지로 이동시키는 역할을 한다. 그래서 a태그와는 조금 다르다. 

a태그는 마우스 모양이 달라지고 다른 페이지로 이동하는데 form은 버튼입니다. 
