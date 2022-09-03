# 8-3. WebFont와 Validator

지금 CSS가 600줄이 넘었습니다. 지금은 CSS 파일이 하나밖에 없는데 쪼잴 수 있습니다. 여러개의 CSS파일을 불러올 수 있기 때문입니다. 

```HTML
<link rel="stylesheet" href="./naver1.css"/>
<link rel="stylesheet" href="./naver2.css"/>
<link rel="stylesheet" href="./naver3.css"/>
```

## \<style\>

\<head\> 태그에 \<style\> 태그가 있는데, CSS 코드들을 HTML의 \<style\>에 적용해도 됩니다. 보통 공통적으로 사용되는 CSS를 넣습니다. HTML은 웹의 구조와 Content를 담당하고 디자인은 CSS가 담당해야 합니다. 동작은 Javascript가 담당합니다.

그럼 \<style\> 태그를 사용한다는 것은 어긋나는 것인데, HTML에 디자인을 담당하는 CSS를 넣었기 때문입니다. 그런데 실무에서는 그렇게 해야하는 케이스들도 있습니다. 그리고 Google은 권장하고 있습니다. 권장하는 이유는 CSS파일들이 로드하는 시간이 발생하게 되고 그만큼 고객들에게 렌더링 되는 시간이 늦어지기 때문입니다. 하지만 HTML에 포함된 \<style\>의 CSS들은 바로 적용됩니다. 외부 파일들을 로드하는 경우 로드 시간이 소요됩니다. 그래서 Google에서는 HTML의 필수적인 디자인, CSS들은 \<style\>에 넣는 것을 권장합니다. 원칙적으로는 HTML과 CSS를 분리하는 것이 맞지만 실무상의 이유로 고객 관점의 이유로 HTML에 CSS를 작성하기도 한다고 생각하면 됩니다. 

참고로 \<style\>에 CSS를 작성할 때 순서를 조심하셔야 합니다. CSS는 아래 작성된 것이 더 우선순위가 높게 적용됩니다. 

다음 HTML에서는 \<style\>의 CSS가 naver.css의 CSS보다 위에 작성되어있는 것입니다.

```HTML
<head>
    <meta charset="utf-8">
    <title>네이버</title>
    <link rel="shortcut icon" type="image/x-icon" href="./favicon.ico"/>
	<style>
		CSS
	</style>
    <link rel="stylesheet" href="./naver.css"/>
</head>
```

다음과 우선순위가 다르게 됩니다.
```
    <link rel="stylesheet" href="./naver.css"/>
	<style>
		CSS
	</style>
```

## 실무에서 nth-child

보통 nth-child를 사용하지 않고 실무에서는 보통 class이름을 사용합니다. 코드는 언제든 바뀔 수 있어, 순서로 식별하는 것은 유지보수상 문제가 있습니다. 
```CSS
#nav-menu li:nth-child(8) span:first-child {
    width: 26px;
    background-position: -279px -208px;
}
```

## font

![image](https://user-images.githubusercontent.com/79847020/188257146-75840378-c02f-479d-881f-a0206c226ea1.png)

글자 CSS를 적용하려고 하는데, 단순히 글만 있다면 line-height를 조절하면 됩니다. 한줄의 높이기 때문에 line-height를 조절해서 가운데를 맞춰줄 수는 있는데 여러가지 icon들이 세로로 배치되어 있고 하면 line-height가 좋은 선택이 아닐 수도 있습니다.

![image](https://user-images.githubusercontent.com/79847020/188257531-6e2de44a-f452-41f9-a9fb-fea852b0be78.png)

그럴 때는 lien-height(라인 하이트) 주시지 마시고 padding을 적용하는 것을 권장드립니다. 

```CSS
#main-ytn h3, #main-newsstand h3 {
    font-size: 14px;
    font-family: NanumSquare, Dotum, "돋움", Helvetica, "Apple SD Gothic Neo", sans-serif;
    line-height: 45px;
    padding-left: 15px;
}
```

![image](https://user-images.githubusercontent.com/79847020/188257630-349f96b7-dc43-46a2-ae2b-00f30fbe27d0.png)

```CSS
#main-ytn h3, #main-newsstand h3 {
    font-size: 14px;
    font-family: NanumSquare, Dotum, "돋움", Helvetica, "Apple SD Gothic Neo", sans-serif;
    /*line-height: 45px;*/
    padding-top: 15px;
    padding-left: 15px;
}
```

![image](https://user-images.githubusercontent.com/79847020/188257664-7a9fe1e3-6795-4cd3-bfea-c485e8745d32.png)

네이버에서 사용하는 font css를 찾습니다. font css파일을 생성하겠습니다. 보통 webfont라고 합니다. font를 web에서 다운로드 받기 때문에 webfont라고 합니다.

```HTML
<head>
    <meta charset="utf-8">
    <title>네이버</title>
    <link rel="shortcut icon" type="image/x-icon" href="./favicon.ico"/>
    <link rel="stylesheet" href="./naver.css"/>
    <link rel="stylesheet" href="./webfont.css"/>
</head>
```

```CSS
@font-face {
    font-weight: 400;
    font-family: NanumSquare;
    src: url(https://s.pstatic.net/static/www/font/NanumSquareR.eot);
    src: url(https://s.pstatic.net/static/www/font/NanumSquareR.eot?#iefix) format("embedded-opentype"), url(https://s.pstatic.net/static/www/font/NanumSquareR.woff) format("woff"), url(https://s.pstatic.net/static/www/font/NanumSquareR.ttf) format("truetype")
}

@font-face {
    font-weight: 300;
    font-family: NanumSquare;
    src: url(https://s.pstatic.net/static/www/font/NanumSquareL.eot);
    src: url(https://s.pstatic.net/static/www/font/NanumSquareL.eot?#iefix) format("embedded-opentype"), url(https://s.pstatic.net/static/www/font/NanumSquareL.woff) format("woff"), url(https://s.pstatic.net/static/www/font/NanumSquareL.ttf) format("truetype")
}

@font-face {
    font-weight: 700;
    font-family: NanumSquare;
    src: url(https://s.pstatic.net/static/www/font/NanumSquareB.eot);
    src: url(https://s.pstatic.net/static/www/font/NanumSquareB.eot?#iefix) format("embedded-opentype"), url(https://s.pstatic.net/static/www/font/NanumSquareB.woff) format("woff"), url(https://s.pstatic.net/static/www/font/NanumSquareB.ttf) format("truetype")
}

@font-face {
    font-weight: 900;
    font-family: NanumSquare;
    src: url(https://s.pstatic.net/static/www/font/NanumSquareEB.eot);
    src: url(https://s.pstatic.net/static/www/font/NanumSquareEB.eot?#iefix) format("embedded-opentype"), url(https://s.pstatic.net/static/www/font/NanumSquareEB.woff) format("woff"), url(https://s.pstatic.net/static/www/font/NanumSquareEB.ttf) format("truetype")
}
```

@font-face를 font-weight별로 구성해놓았습니다. 400이 보통이고, 300이 약간 얇은, 700이 약간 두꺼운, 900이 두꺼운 입니다. font-famil에 font이름을 적어주고 font를 어디서 다운로드 받을 수 있는지 적어줍니다.

font를 제공하는 업체에서 보통 CSS를 제공을 할 것입니다. 복사를 하셔서 webfont.css를 만들고 HTML에 추가하면 됩니다. 

폰트 적용전

![image](https://user-images.githubusercontent.com/79847020/188260407-047caa1c-0743-4604-9655-25ad3e86de01.png)

폰트 적용후

![image](https://user-images.githubusercontent.com/79847020/188260415-aa38d7d5-5f1a-448f-b4dd-16eccb439552.png)


## section

\<div\>에 의미를 부여하는 태그를 symentic태그라고 하는데, 그 중 \<section\>가 있습니다. 특정 구역을 나타내는 역할을 합니다. 

어떤 것에 적용하면 좋냐면 이런 사각 박스에 적용하기 좋습니다.

![image](https://user-images.githubusercontent.com/79847020/188260477-ddc78c2a-42f0-4378-b8b4-94d78816b5c2.png)

이런 것들을 section으로 바꿔주겠습니다. 

```HTML
...생략
	<section id="main-ad"></section>
	<section id="main-login">
		<h3 class="blind">로그인</h3>
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
	</section>
...생략
```		
```CSS
section {
    border: 1px solid #dee3eb;
}
```

![image](https://user-images.githubusercontent.com/79847020/188260610-c9f3cbfe-5563-449e-b2fd-772604e6ab6b.png)

section 태그를 살펴보면 보통, h3들이 넣어줍니다.

접근성 지침은 전문적으로 HTML을 하실 분은 확인을 하셔야합니다. 법인인 경우 접근성을 위한 의무가 있기 때문입니다. 우리가 다운로드 받은 확장앱 Validity를 이용하거나 validator.w3c.org에 접속해서 

![image](https://user-images.githubusercontent.com/79847020/188260926-e400ff34-d162-4cfa-8935-c0d78bf44678.png)

![image](https://user-images.githubusercontent.com/79847020/188260915-5c5a03b5-432e-451d-bac7-8584152857e2.png)

![image](https://user-images.githubusercontent.com/79847020/188260975-5fdc56de-490c-4532-98f8-e58364b00dd2.png)

결과가 다음가 같이 나오는데

![image](https://user-images.githubusercontent.com/79847020/188261360-b0ff4ad0-2569-4564-b891-044b00df4ee5.png)

frameborder와 scrolling을 사용하지 말라는 내용입니다. 

이미 intellij에서도 체크를 해주고 있었습니다.

![image](https://user-images.githubusercontent.com/79847020/188261403-0a7d0163-f6a0-4d20-945e-901dd3f475e6.png)

```HTML
<iframe src="https://castbox.shopping.naver.com/shopbox/main.nhn?">
</iframe>
```
```CSS
#main-shopping iframe {
    width: 100%;
    height: 100%;
    border: none;
    overflow: hidden;
}
```
frameborder="0"는 border:none;로, scrolling="no";는 어떻게 해결해야할지 모르겠는데 각자 찾아보는걸로 합시다. 

이게 웹 표준 지키기 위함이고, 또 웹 접근성은 따로 공부를 하셔야합니다. 

