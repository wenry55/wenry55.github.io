---
layout: default
title: Using Jekyll
tags: [jekyll, howto]
categories: [jekyll, howto]
---
##### 지킬 사용하기

프론트매터에 지정되는 카테고리는 트리구조의 형태로 url에 매핑이 됩니다.
포스트에 카테고리가 지정되면, 포스트의 url은 cat1 -> cat2 -> cat3 와 같은 형태로 매핑되는 것을 볼 수 있습니다.
그래서, 처음에 포스트를 작성하고, 카테고리를 추가한 다음 페이지 리프레쉬를 하면 ```page not found``` 를 볼 수 있습니다.

##### 지킬에서 사용하는 언어

지킬을 어느정도 사용하다 보면, 커스터마이징을 하기 위하여 프로그래밍이 필요합니다. 이 때 지킬에서 사용가능한 언어는 [```Liquid```](https://jekyllrb.com/docs/liquid/) 라는 템플릿팅 언어입니다. 

참고로, Liquid 에서 핵심적인 노테이션은 두가지 인데, {% raw %}`{ expression }`{% endraw %} 과 {% raw %}`{% statement %}`{% endraw %} 입니다. 