![image](https://user-images.githubusercontent.com/79847020/165528153-d9279264-687c-44d4-8c31-a7f940bd8db4.png)

naver.html을 생성합니다. 

웹 브라우저는 Html, CSS, JavaScript 실행기라고 생각하면 됩니다. 

IDE는 WebStorm을 사용하겠습니다. 폴더를 열면 폴더 구조를 WebStrom에서 그대로 가져올 수 있습니다.

WebStrom이 좋은 이유는 예를 들어 빈 html에 doc를 입력하고 엔터만 쳐도 다음 코드가 입력됩니다.

```html
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>

</body>
</html>
```

개발자는 실력도 중요하지만 정확도, 속도도 중요한 실력의 척도입니다. 따라서 자기에 맞는 IDE를 잘 활용하는 것이 좋습니다. 

네이버의 실제 태그를 살펴보겠습니다.

![image](https://user-images.githubusercontent.com/79847020/165535375-d29d3b57-0e24-46ab-8591-b42da434a41d.png)

크롬의 개발자도구에서 JavaScript는 Console탭에서, Html은 Elements탭에서 볼수 있습니다. Html 태그에 마우스를 갖다대면 해당 태그가 웹 페이지의 어떤 부분을 담당하는지 살펴볼 수 있습니다. 

![image](https://user-images.githubusercontent.com/79847020/165535706-95acb5eb-9162-450e-a7ad-40725295ed9e.png)

Html의 태그는 \< \> 에 둘러쌓여있고 트리구조로 되어있습니다. 상위 태그안에 하위 태그들이 포함되어 있습니다. 계층적인 구조를 가진다.

구조를 보면 <!DOCTYPE>이 있고 <Html>, <Head>, <Body>순으로 되어있습니다. 

![image](https://user-images.githubusercontent.com/79847020/165544624-8d16783e-e1b0-49e8-aaa4-c50a9c9315d3.png)
  
HTML의 기본구조입니다.  

```html
<!doctype html>
<html>
    <head>

    </head>
    <body>
    
    </body>
</html>
```

Doctype은 다양하게 되있는데 현대 웹에서는 <!Doctype HTML>만 사용된다고 생각하시면 됩니다.
  
HTML의 태그는 다음 구조로 되어있습니다.
  
```html
<태그>
  <자식태그>
    <손주태그>
    </손주태그>
  </자식태그>
  <자식(형제)태그>
  </자식(형제)태그>
</태그>
```
  
자식태그와 자식태그는 형제태그라고 합니다. 자손관계와 손자관계가 다르다는 것을 알아두어야 합니다. 손자관계는 <태그>와 <손주태그> 관계를 말하고 자손관계는 <태그>와 <자식태그><손주태그> 관계를 말합니다. 
  
영어로 표현하면 다음과 같습니다. 검색할 때는 영어로 나오기 때문에 알아두어야 합니다.
  
```html
<parent>
  <children>
    <grandchildren></grandchildren>
  <children>
  <sibling>
  </sibling>
</parent>
```
    
    

        
  
  



  
  
  
  
  















