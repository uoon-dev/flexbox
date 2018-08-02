CSS Display Model
=================

   ## 1. Display Group 
   ------------------

   > outside | inside | listitem | internal | box | legacy

   >> outside (normal flow)
   >>> *    BFC IFC RF
   >>> *    block | inline | run-in

   >> listitem (list-item)
   >>>   * 옛날에는 CSS가 아니라 태그 자체에 li처럼 그려달라는 알고리즘이 설계되어 있었다. 
   >>>     그래서 display 속성으로 listitem을 주면 자동으로 해당 속성을 가진 태그는 li로 그려졌다.
   >>>   * 이런 개념은 table 도 비슷하다. 

   >> box
   >>>    * contents | none
   >>>    * native에서 설계한 contents에 따라 그림을 그리도록 하는 속성이다. 즉 기본값을 의미한다.
   >>>    * 부모 태그에 영향을 받는 자식 태그에 display 속성으로 contents를 주면 부모를 까고 승격되어 자기 자신이 주인공이 된다.
   >>>    * iframe, input, textarea, button, img, canvas 등

   >> inside (contents layout)
   >>>    * flow | flow-root | table | flex | grid | subgrid | ruby
   >>>    * outside는 바깥쪽(부모)에서 자신(자식)을 어떻게 그리도록 할 것인지 명령하는 그룹이라면
   >>>      inside는 자기 자신의 안 쪽에 있는 태그들을 어떻게 그릴 것인지 명령하는 그룹이다.

   >> legacy
   >>>    * inline contents layout
   >>>    * 두 개의 속성을 display에 줄 수 없기 때문에 생겨난 것이 레거시이다.
   >>>      예를 들어 나 자신은 inline이고 싶은데 자식은 flex layout으로 설계하고 싶은 경우 적용할 수 있는 방법이 없다. 
   >>>      그래서 다음과 같은 명령어들이 등장했다.
   >>>    * inline-block | inline-table | inline-flex | inline-grid

   >> internal (table & ruby)
   >>>    * internal은 특정 layout system에 들어왔을 때 그 안에 있는 item들이 사용해야 하는 속성이다.
   >>>    * table-row-group | table-header-group | table-footer-group 
   >>>      table-row | table-cell | table-column-group | table-column | table-caption 
   >>>      ruby-base | ruby-text | ruby-base-container | ruby-text-container 
   >>>    * table-cell은 지금도 자주 쓰인다. 얘만 vertical align이 되기 때문이다.
   >>>    * ruby는 일본어의 첨자 같은 것 때문에 생겨났다.


   ## 2. Flex Box (css flexible box layout module)
   -----------------------------------------
   > * display: flex | inline-flex
   > * 직계 자식은 자동적으로 flex-item이 된다. (flexbox는 상속되지 않는다.)
   > * flexbox는 기본적으로 그림을 그릴 때 한 줄만 그리는 정책을 가지고 있다.
   > * 이걸 우리는 flex-line이라고 부른다.(flex-line이라는 가상의 선에 따라 flex item이 배치된다.) <hr/>
   > * main-axis: 기본값 축은 x축으로 되어있고 이를 main-axis라고 부른다. 
   > * cross-axis: main-axis와 교차되는 y축은 cross-axis라고 부른다. 
   > * flex-direction : 왼쪽에서 오른쪽으로 배치되는, 위에서 아래로 배치되는 형식 따위. 이런 default 방향은 바꿀 수 있다. 
   >   이를 거꾸로 역전하려면 row-reverse를 사용하면 된다. row라고 부르는 이유는 한 줄에 배치하기 때문이다. 
   >   아예 축을 바꾸고 싶다면 column-reverse를 사용하면 된다.
   > * justify-content : content를 정의하는 속성. 세로축 가로축 속성을 지정하는 게 아니라 main-axis에 따라 지정하는 속성을 의미한다.
   >> + flex-start | flex-end | center | space-between | space-around
   >>> - main-axis가 x축이고 flex-start 속성을 주면 가장 왼쪽부터 item들을 정렬한다. 
   >>> - flext-end 속성을 주면 가장 오른쪽부터 item들을 정렬한다.
   >>> - space-between을 주면 양끝을 붙이고 item 사이에 여백을 준다.
   >>> - space-around를 주면 양끝을 살짝 띄우고 item 사이에 여백을 준다.
   > * align-items : cross-axis 축에서 정렬할 때는 align-items를 사용한다. 
   >> + flex-start | flex-end | center | baseline | stretch
   >>> - stretch 속성을 주면 item 길이를 양끝까지 늘린다.
   > * flex-wrap
   >> + nowrap | wrap | wrap-reverse
   >>> - 기본값은 nowrap이다. 만약 wrap 속성을 주면 한 줄에 있는 마지막 item 너비가 초과되었을 때, 
   >>>              다음 줄로 내려가 가장 높이가 큰 item 하단에 새로운 flex-line을 그린다.
   > * align-content
   >> + flex-wrap에 적용되는 속성이다. 즉 flex-wrap에 따라 정렬된 flex-line을 어떻게 정렬할지 지정하는 속성이다.

   ## 2.1 Flex Item
   ----------------
   > * order : order를 이용해 순서를 마음대로 바꿀 수 있다. 
   > * DOM Rendering이 아니라 Post-Process에 따라 그려지기 때문에 속도가 빠르다. GPU에 올려서 그리기 때문.
   > * 즉, Reflow가 안 일어나고 Repaint만 하기 때문에 JS를 이용해 order만 바꿔줘도 훨씬 빠르고 쉽게 바꿔줄 수 있다.
   > * align-self : 해당되는 item의 정렬 방식을 바꿀 수 있다.
   >> + flex-start | flex-end | center 등
   > * flex-grow : 어디까지 늘어나게 할지 한계를 지정함 (다른 item과의 상대적인 비율로 지정된다.)
   > * flex-shrink : 어디까지 줄어들게 할지 한계를 지정함 (다른 item과의 상대적인 비율로 지정된다.)
   > * flex-basis : auto|content or width,height
