![image](https://user-images.githubusercontent.com/79847020/166941690-992deaa1-bb7c-4eb5-a521-73228d35a179.png)

한글 입력기나 자동완성 레이어를 왜 안만드는지 의문이 들수 있는데 웹 사이트는 시각장애인도 사용하기 때문에 한글입력기가 필요합니다.

또한 탭을 눌러서 커서를 이동시킬 수 있는데 이런것들도 꼼꼼히 체크해야합니다. 또 탭을 누르다보면 왼쪽 상단에 화면 이동을 위한 기능들이 동작합니다.

![image](https://user-images.githubusercontent.com/79847020/166942202-4f368d16-9182-4bc3-82bb-25bfd0d3621e.png)

이런 것들을 웹 접근성이라고 합니다. 모든 사용자들이 해당 웹 페이지를 이용할 수 있도록 하는것을 말합니다.

![image](https://user-images.githubusercontent.com/79847020/166942768-c5874aac-92cf-41bd-b333-347a2cf96a22.png)

맨 상단위에 그런것들이 위치해 있는데 화면에는 안보이지만 이런것들이 숨겨져 있습니다. 이런 것들을 이번 강좌 수준에서 다룰 수 있는 수준이 아니라서 눈에 보이는 것들만 진행하겠습니다.

![image](https://user-images.githubusercontent.com/79847020/166943029-4e890fb8-9dd1-45d7-965f-bb246a255d2b.png)

\<span\>은 그냥 텍스트라고 생각하셔도 무방합니다. 

![image](https://user-images.githubusercontent.com/79847020/166943679-6c8ffae0-cc9b-42e3-95ab-05b6d2cadfed.png)

목록의 경우 \<ul\> 안에 \<li\> 안에 \<a\>가 존재하는 것을 알 수 있습니다. \<ul\> 은 순서가 없는 리스트, \<ol\>은 순서가 있는 리스트를 의미합니다.

```html
        <ul><!--unordered list-->
            <li>메일</li><!--list item-->
            <li>카페</li><!--list item-->
            <li>블로그</li><!--list item-->
            <li>지식인</li><!--list item-->
            <li>쇼핑</li><!--list item-->
            <li>쇼핑</li><!--list item-->
        </ul>
        <ol><!--ordered list-->
            <li>메일</li><!--list item-->
            <li>카페</li><!--list item-->
            <li>블로그</li><!--list item-->
            <li>지식인</li><!--list item-->
            <li>쇼핑</li><!--list item-->
            <li>쇼핑</li><!--list item-->
        </ol>
```

![image](https://user-images.githubusercontent.com/79847020/166948540-36bb76bc-918d-43de-82ee-c6b8ab4244a9.png)

크롬 개발자모드에서 해당 내용이 소스 어디에 위치해있는지 모를 때는 Ctrl+F로 찾거나 Ctrl + Shift + C, Select an element in the page to inspect it를 활성화하면 됩니다.

![image](https://user-images.githubusercontent.com/79847020/166953821-58205cb3-a5ec-4273-ab0d-73b24d8a0b3d.png)

해당 기능으로 연합뉴스 이미지의 주소를 찾아 다운로드 받습니다. 보통 이런 이미지를 모아두는데 네이버는 pm.pstatic.net 서버에 두는 것을 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/79847020/166954828-9f593951-40d7-4441-9a69-9b681f6ede4f.png)

여기서 알아야하는 것은 html/css/javascript 작성하는 것은 남들에게 노출된다라는 것을 알아야합니다.  보안에 위험이 생기는 것을 주의하셔야 합니다.

```html
  <h3>언론사 목록</h3>
  <ul>
      <li><img src="./한국일보.png" alt="과학동아"></li>
      <li><img src="./전자신문.png"></li>
  </ul>
```

여기서 많이 놓치는 것이 시각장애인은 이미지를 보지 못하기 때문에 alt속성을 사용해서 설명을 추가해주어야 합니다. 스크린리더라는 시각장애인용 프로그램이 alt를 읽어줍니다. 

\<i\>태그에 관점 2가지가 있습니다. 기울임이 꼭 필요한가? 기울임은 디자인요소인데 html이 담당해야하는가? 아니면 중요하다는 것을 구분해주는 의미로 \<html\>에서 사용되도 무방하다. (중요하다는 의미는 주로 em태그로) 그런 논쟁이 있습니다. html은 자유로운 언어지만 회사에서 코딩할 때 규약이 있기 때문에 생각해보면 좋습니다. 

html은 여기까지 했는데 추가로 \<iframe\>과 \<table\>를 추가로 설명드리겠습니다.  
















