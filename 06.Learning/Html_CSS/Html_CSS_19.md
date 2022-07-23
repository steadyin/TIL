이번시간에는 float과 블롯 포맷 컨테스트 또는 블록 서식 문맥이라고 번역하는 개념에 대해서도 알아보겠습니다.

컨테이닝 블록은 자식 입장에서 포지션을 가졌을 때 누구를 기준으로 위치해야하는가, 위쪽 태그를 찾아가는 규칙입니다.

그 다음 즐겨찾기를 하고 두고 보셔야 할 것이 Block Formatting Context입니다.

https://developer.mozilla.org/ko/docs/Web/Guide/CSS/Block_formatting_context

Block Format Context는 부모 입장에서 자식이 누구 까지를 내 블록에 포함해야되나를 찾아보는 개념입니다.

참고로 이 블록은 display: inline-block;의 블록과는 다른 의미입니다. 헷갈리면 안됩니다.

블록 서식 문맥은 display: block;일 때는 안생기고 display: inline-block; 일 때 블록 서식 문맥이 생깁니다. 

그래서 블록 서식 문맥의 블록과 display: block;의 블록과 구분을 하셔야 합니다.

```
* 문서의 루트 요소(<html>).
* 플로팅 요소(float이 none이 아님).
* 절대 위치를 지정한 요소(position이 absolute 또는 fixed).
* 인라인 블록(display가 inline-block).
* 표 칸(display가 table-cell, HTML 표 칸의 기본값).
* 표 주석(display가 table-caption, HTML 표 주석의 기본값).
* display가 table, table-row, table-row-group, table-header-group, table-footer-group (HTML 표에서, 각각 표 전체, 행, 본문, 헤더, 푸터의 기본값) 또는 inline-table인 요소가 암시적으로 생성한 무명 칸.
* overflow가 visible이 아닌 블록 요소.
* display가 flow-root.
* contain이 layout, content, paint.
* 스스로 플렉스, 그리드, 테이블 컨테이너가 아닌 경우의 플렉스 항목(display가 flex 또는 inline-flex인 요소의 바로 아래 자식)
* 스스로 플렉스, 그리드, 테이블 컨테이너가 아닌 경우의 그리드 항목(display가 grid 또는 inline-grid인 요소의 바로 아래 자식)
* 다열 컨테이너(column-count (en-US) 또는 (column-width (en-US)가 auto가 아닌 경우. column-count: 1 포함).
* column-span (en-US)이 all인 경우. 해당하는 요소가 다열 컨테이너 안에 위치하지 않아도 항상 새로운 블록 서식 맥락을 생성해야 합니다. (명세 변경, Chrome 버그)
```

그리고 overflow가 visible이 아닌 블록 요소를 잘 기억해두셔야 합니다. 아직 overflow를 배우지는 않았지만 블록 서식 문맥을 만드는 방법 중에 인라인 블록과 float과 overflow를 기억해두셔야 합니다.

* 인라인 블록(display가 inline-block).
* 플로팅 요소(float이 none이 아님).
* overflow가 visible이 아닌 블록 요소.

하나 더 기억하실만한게 

* 절대 위치를 지정한 요소(position이 absolute 또는 fixed)

이것들이 중요하니깐 기억하시면 좋을 것 같습니다. 







