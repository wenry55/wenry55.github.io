---
title: "지킬 사용하기"
layout: default
tags: [jekyll, howto]
categories: [howto, tech, markdown ]
---


# 안녕하세요?

{% for category in site.categories %}
  <h3>{{ category[0] }}</h3>
  <ul>
    {% for post in category[1] %}
      <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
{% endfor %}

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>

### categories 

front matter에 추가하였으나 나타나지 않거나, 페이지가 보이지 않는 경우.
위의 부분은 _pages 디렉토리 아래에 categories.html을 만들고 나서 부터는 이상없이 보이는 것 같네요.


### front matter ... 신발장앞에 있는 매트?

어떤 파일이든지 프런트매터 블럭을 포함하는 경우, 지킬이 이를 프로세스 합니다.

#### pages

페이지는 어디에 저장해야 하나요?

포스트는 _posts, 그러므로 페이지는 _pages가 아닐까요? 


