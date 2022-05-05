네이버에는 밑줄이 표시되어있는데 링크가 걸려있을 때 밑줄로 표현됩니다. 링크는 \<a\>태그를 사용합니다.

```html
<h1>
    <a href="">네이버</a>
</h1>
```

링크라는 것은 다른 웹 페이지로 넘어갈 수 있게 해주는 태그를 의미합니다. \<a\> 태그만 지정하면 제대로 동작할 수 없기 때문에 부가정보가 필요합니다.

href는 \<a\>의 부가정보입니다. 부가정보는 속성, Attribute라 부릅니다.

\<html\>태그는 lang이라는 속성이 있습니다.

```html
<html lang="ko">
```

코드에 대한 설명을 다는 것은 주석을 단다고 말하는데 html의 주석은 \<!-- --\>로 표현합니다. 하이픈으로 표현합니다. Comment라고 말합니다.

```
<h1>
    <a href="https://www.naver.com">네이버</a>
    <!-- 주석입니다. -->
    주석입니다.
    <!-- 주석입니다. -->
</h1>
```

![image](https://user-images.githubusercontent.com/79847020/166930689-99d3ed85-662b-4fc9-a45c-e4225b6cf579.png)

```html
<!-- 주석(comment)입니다 -->
<!-- 속성(attribute)입니다 -->
```

네이버의 검색창은 희안한게 디자인 되어있습니다. \<fieldset\> \<legend\>이 사용됩니다. legend는 범위를 의미합니다.

\<input\> 태그는 대표적으로 안에 컨텐츠가 없습니다. 바로 열자마자 닫아도 무방합니다.

```html
  <fieldset>
      <legend>검색</legend>
      <input />
  </fieldset>
```
![image](https://user-images.githubusercontent.com/79847020/166931972-6a9d1cda-6401-4f59-97c3-327f93ba525d.png)

\<textarea\>도 있습니다. 긴글을 입력할 때 사용합니다.

![image](https://user-images.githubusercontent.com/79847020/166932143-b1ae6d53-7c0b-435a-a0ce-33d571311678.png)

\<input\>태그로 표현할 수 있는 입력 타입은 다양합니다.

\<input type="checkbox"\>
\<input type="checkbox"\>
\<input type="radio" name="group"\>
\<input type="radio" name="group"\>

체크박스는 서로 선택에 영향을 주지 않지만 radio버튼은 양자택일, 다중택일해야 합니다. 

![image](https://user-images.githubusercontent.com/79847020/166933347-85a8f9ba-b369-4f14-923e-a46faf9636be.png)

그 다음 \<button\>가 있습니다.

```html
<button>검색</button>
```

![image](https://user-images.githubusercontent.com/79847020/166933478-61e3744e-0237-41f3-a7bd-299a33ed3bd4.png)

링크는 손가락모양으로 뜨지만 버튼은 기본 마우스 모양으로 뜹니다.



  











