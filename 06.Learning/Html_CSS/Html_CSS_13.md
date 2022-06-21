## 

![image](https://user-images.githubusercontent.com/79847020/174795208-a84fdbc5-81eb-4ccb-92ce-119ea314d03c.png)

이제 해야할 것이 검색버튼을 주어야 합니다. 

![image](https://user-images.githubusercontent.com/79847020/174795550-b729ffea-5442-4f68-b7f6-070fb7dfdc74.png)

div header-serch 자식의 fieldset 자식 button 태그에 CSS를 적용해야 합니다. button은 width:49px height:49px 입니다. 

```CSS
#header-search fieldset button {
    width: 49px;
    height: 49px;
}
```

현재 button은 다음과 같이 blind처리가 적용되어 있습니다.

```html
...
  <h2>검색창</h2>
  <fieldset>
      <legend class="blind">검색</legend>
      <input/>
      <button class="blind">검색</button>
  </fieldset>
...
```

버튼은 blind 해제하고 글자만 blind 처리를 하고 싶다면 \<span\>를 추가합니다. 

```html
...
  <h2>검색창</h2>
  <fieldset>
      <legend class="blind">검색</legend>
      <input/>
    <button><span class="blind">검색</span></button>
  </fieldset>
...
```

![image](https://user-images.githubusercontent.com/79847020/174796442-92543c0e-2cd5-48e6-bd4b-3c1de4277c9b.png)

이 button을 우리가 원하는 대로 바꾸어보도록 하겠습니다. 버튼도 기본 CSS가 적용되어 있습니다. 

![image](https://user-images.githubusercontent.com/79847020/174796798-e7cc389c-820a-4931-8600-a646a6b59b64.png)

보시다시피 border, padding이 적용되어 있고 background-color은 회색입니다.

이전에 background를 할 때 background: url(); 을 사용해서 배경 이미지를 적용했지만 하나의 컬러를 넣을 수도 있습니다.

```CSS
#header-search fieldset button {
    width: 49px;
    height: 49px;
    border: 0px;
    padding: 0px;
    background: #03CF5D;
}
```
![image](https://user-images.githubusercontent.com/79847020/174797104-b84a02c3-d3df-4e3d-afa9-a0d21df67077.png)

그 다음 버튼안에 이미지를 넣어보도록 하겠습니다.

![image](https://user-images.githubusercontent.com/79847020/174798181-59e20697-c39a-4e05-a2b0-268924f46a72.png)

돋보기를 넣기 위해서는 이전과 똑같이 background 이미지를 사용하면 되겠죠.

```HTML
...
    <button>
        <span class="blind">검색</span>
    </button>
...
```

버튼에 background 이미지를 적용하겠습니다.

```CSS
#header-search fieldset button {
    width: 49px;
    height: 49px;
    border: 0px;
    padding: 0px;
    background: #03CF5D;
    background-image: url(./sp_search.png);
}
```

![image](https://user-images.githubusercontent.com/79847020/174799182-e77afbd1-20eb-42ef-a370-2b8ca62c5d76.png)

이제 background-position으로 위치를 잡아주어야 합니다.

```CSS
#header-search fieldset button {
    width: 49px;
    height: 49px;
    border: 0px;
    padding: 0px;
    background: #03CF5D;
    background-image: url(./sp_search.png);
    background-position: 0px -50px;
    background-repeat: no-repeat;
}
```
![image](https://user-images.githubusercontent.com/79847020/174799765-113952f6-a14c-43dd-8fae-55569884b107.png)

브라우저 개발자도구에서 적절한 포지션를 살핍니다.

![image](https://user-images.githubusercontent.com/79847020/174800012-be6cb768-ea1d-4a1b-b0b9-bdc1180bb523.png)

![image](https://user-images.githubusercontent.com/79847020/174799948-b7b9046c-3e21-42f0-bc49-b7d33bc32e15.png)

그런데 옆에 있는 다른 이미지들이 나오게 됩니다. 

이런 경우 background-size로 시도해보지만 background-image 원본 이미지 전체 사이즈를 조정하는 것이기 때문에 안됩니다.

검색 이미지용 \<span\>을 추가해서 적용하겠습니다.

```HTML
...
<fieldset>
    <legend class="blind">검색</legend>
    <input/>
    <button>
        <span class="blind">검색</span>
        <span class="search-image"></span>
    </button>
</fieldset>
...
```

다음과 같이 CSS를 적용할 수도 있지만 너무 길다 싶으면

```CSS
#header-search fieldset .search-image {
...
}
```

태그에 id를 지정하고

```HTML
    <span id="search-image"></span>
```

CSS를 지정할 수 있습니다. width와 height는 돋보기 그림 크기에 맞춰 21px 21px를 지정합니다.
```CSS
#search-image {
    background-image: url(./sp_search.png);
    background-position: 10px -50px;
    background-repeat: no-repeat;
    width: 21px;
    height: 21px;
}
```

![image](https://user-images.githubusercontent.com/79847020/174808282-9ef7e706-c135-4792-bd69-09bbf234a5b5.png)

기본적으로 span은 CSS display가 inline이 기본값입니다. inline인 경우에는 가로 세로를 바꿀 수가 없어요. 포함된 content에 의해 크기가 변경되기 때문에 지금과 같은 경우 \<span\>에 content가 없기 때문에 현재는 0px x 0px 입니다.

![image](https://user-images.githubusercontent.com/79847020/174808827-5ab083c1-1f08-4b3b-b033-44e93fe6b8a1.png)

이것을 가로 세로를 지정할 수 있는 display:inline-block; 으로 바꾸는 순간 \<span\>의 content width, height가 21px x 21px로 변경됩니다.

background-position을 조정해서 이미지를 맞추면 다음과 같아집니다.

```CSS
...
#search-image {
    background-image: url(./sp_search.png);
    background-position: -4px -60px;
    background-repeat: no-repeat;
    width: 21px;
    height: 21px;
    display: inline-block;
}
...
```

![image](https://user-images.githubusercontent.com/79847020/174808962-0440218a-cd4e-4716-8ead-d04a64622cf3.png)

그리고 가운데 정렬을 해야하는데 실무에서는 position을 사용하겠지만 현재는 배우지 않았으므로 margin을 사용해서 정렬을 할 수 있습니다.

49px x 49px 버튼 안에 21px x 21px 이미지(\<span\>)이 들어가 있으므로 margin을 각 14px 주면 맞을 것 같습니다.

```CSS
...
#search-image {
    background-image: url(./sp_search.png);
    background-position: -4px -60px;
    background-repeat: no-repeat;
    width: 21px;
    height: 21px;
    display: inline-block;
    margin: 14px;
}
...
```

![image](https://user-images.githubusercontent.com/79847020/174809674-8bc070a0-60a7-48f8-952e-ff89523ab124.png)

이후에 position을 적용해보겠습니다.




