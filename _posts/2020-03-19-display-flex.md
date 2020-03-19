---
layout: default 
title: Flex
categories: [css]
tags: [css, flex]
---

# CSS Flex 

플렉스는 플렉서블 박스입니다. 즉, 유연한 박스라는 말이겠죠. (언제부터 도입)

![Flex](/assets/images/flex-base.jpg)

플렉스는 컨테이너와 아이템으로 보실 수 있습니다.

플렉스를 위한 컨테이너는 `display: flex;` 또는 `display: inline-flex;` 로 정의합니다.


| dips. prop | description
|:-----------|:-----------
| flex       | block특성의 flex container를 정의
| inline-flex| inline 특성의 flex container를 정의

![flex-display](/assets/images/flex-display.jpg)

위의 그림에서 볼수 있듯이, flex;는 컨테이너가 상위 컨테이너에 대해 블럭(수직)으로 쌓이고, inline이 지정되었을 때는 수평으로 쌓이게 됩니다. 둘다 내부 item에는 영향을 주지 않습니다.

자. 이번에는 flex-flow를 살펴봅시다.
flex-direction / flex-wrap으로 구성된게 flex-flow 입니다.



flex-wap

| value | description | default
|:------| -- | --
| nowrap | 한줄로 표시 | nowrap
| wrap | 여러줄로 표시 | 
| wrap-reverse | items를 wrap의 역방향으로 여러 줄로 묶음.

![flex-wrap](/assets/images/flex-wrap.jpg)


justify-content 

main-axis의 정렬방법을 설정합니다.
| value | description 
|:-|:-
| flex-start | 
| flex-end | 
| flex-center | items를 가운데 정렬
| space-between | 시작점과 끝쩜까지 고르게 정렬
| space-around | items를 균등한 여백을 포함하여 정렬

![flex-justify-content](/assets/images/flex-justify-content.jpg)

align-content

cross-axis의 정렬방법을 설정합니다.
주의할 점은 flex-wrap 속성을 통해 items가 여러줄(2줄이상)이고 여백이 있을 경우만 사용할 수 있습니다.

> items가 한줄일 경우 align-items 속성을 사용하세요.

값 | 	의미|
|:---|:-- |
|stretch| 기본값,	Container의 교차 축을 채우기 위해 Items를 늘림|
|flex-start|	Items를 시작점(flex-start)으로 정렬	
|flex-end|	Items를 끝점(flex-end)으로 정렬	
|center|	Items를 가운데 정렬	
|space-between|	시작 Item은 시작점에, 마지막 Item은 끝점에 정렬되고 나머지 Items는 사이에 고르게 정렬됨	
|space-around|	Items를 균등한 여백을 포함하여 정렬	

![flex-align-content](/assets/images/flex-align-content.jpg)

|속성 |	의미|
|:-|:-|
|order|	Flex Item의 순서를 설정|
|flex| 	flex-grow, flex-shrink, flex-basis의 단축 속성
|flex-grow|	Flex Item의 증가 너비 비율을 설정
|flex-shrink|	Flex Item의 감소 너비 비율을 설정
|flex-basis|	Flex Item의 (**공간 배분 전**) 기본 너비 설정
|align-self|	교차 축(cross-axis)에서 Item의 정렬 방법을 설정

```css
.item {
  flex: 1 1 20px;  /* 증가너비 감소너비 기본너비 */
  flex: 1 1;  /* 증가너비 감소너비 */
  flex: 1 20px;  /* 증가너비 기본너비 (단위를 사용하면 flex-basis가 적용됩니다) */
}
```

flex-grow

Item의 증가 너비 비율을 설정합니다.
숫자가 크면 더 많은 너비를 가집니다.
Item이 가변 너비가 아니거나, 값이 0일 경우 효과가 없습니다.

|값	 |의미|	기본값|
|:--|:---|:---|
|숫자|	Item의 증가 너비 비율을 설정	|0

모든 Items의 총 증가 너비(flex-grow)에서 각 Item의 증가 너비의 비율 만큼 너비를 가질 수 있습니다.
예를 들어 Item이 3개이고 증가 너비가 각각 1, 2, 1이라면,
첫 번째 Item은 총 너비의 25%(1/4)을,
두 번째 Item은 총 너비의 50%(2/4)를,
세 번째 Item은 총 너비의 25%(1/4)을 가지게 됩니다.

![flex-grow](/assets/images/flex-grow.jpg)

flex-grow: 0 은? 고정값인가?
효과가 없다는 뜻. 

#flex-sh