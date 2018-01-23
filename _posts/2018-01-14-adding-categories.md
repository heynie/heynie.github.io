---
layout:     post
title:      "Adding Categories to Jekyll"
subtitle:   "카테고리를 추가하자!"
date:       2018-01-14 17:28:00 +0900
background: '/img/universe.jpg'
comments:   true
categories: info
lcb: "{"
---


Jekyll 테마를 그대로 가져와서 쓰려고 보니 Post에 카테고리가 없었다 ㅠㅠ 아무래도 너무 불편할 것 같아서 열심히 구글링한 끝에 드디어 완성했다 &#128079;&#128079; 아직 CSS는 미완성이지만 조만간 수정해야지..!

내 페이지의 경우 Navbar에 Post가 있기 때문에 이를 dropdown으로 만들어 카테고리를 볼 수 있게끔 했다. 이건 자기 페이지에 맞게 수정하면 된다.

<br>
# 1. Category 만들기

전체 project 폴더 밑에 category 폴더를 만들고 카테고리별로 html 파일을 만들면 된다. 예를 들어 category1, category2 이렇게 두 개의 카테고리를 만들고 싶다면 `/category/category1.html`, `/category/cagetory2.html` 이렇게 두 개의 파일을 만들면 되는 것이다!

각 html 파일의 내용은 다음과 같이 시작한다.

```
---
layout: <원하는 layout>
title: CATEGORY 1
---
```

만약 category를 위한 layout이 필요하다면 `/_layouts` 안에 `category.html`을 만들어서 사용하면 된다. 나는 기존에 있던 layout을 활용하였다.

내용은 기본적으로

```
<ul>
    {{page.lcb}}% for post in site.categories.<categoryname> %}
        <li>
            <a href="{{page.lcb}}{{page.lcb}} post.url }}">{{page.lcb}}{{page.lcb}} post.title }}</a>
        </li>
    {{page.lcb}}% endfor %}
</ul>
```
이런 형식을 사용하면 된다. \<categoryname\>에 'category1' 등 자신이 만든 카테고리명을 넣으면 된다. 페이지는 자유롭게 구성하면 되고, 나는 기존에 있던 페이지를 복사해와서 사용했다.

<br>
# 2. Post 작성하기

원래 하던 대로 `_posts` 폴더 아래에 파일을 생성하면 된다. 파일 맨 위에

```buildoutcfg
---
layout:     <원하는 layout>
title:      "<TITLE>"
date:       YYYY-MM-DD HH:MM:MM +0900
background: '<imgPATH>'
categories: category1
---
```
이렇게 카테고리를 추가해주면 그 포스트가 해당 카테고리로 분류된다.

<br>
# 3. Front 수정하기

category 페이지를 어떻게 보여줄 것인지는 자유롭게 선택하면 된다. 카테고리 하나하나 Navbar Sidebar에 띄울 수도 있고, 나처럼 dropdown을 활용할 수도 있다. 만약 bootstrap으로 만든 페이지라면 [getbootstrap.com](https://getbootstrap.com)에 들어가서 Documentation \> Components \> Navbar 페이지에서 dropdown을 이용할 수 있다. 어려운 건 CSS를 수정하는 것이다. 처음 dropdown을 달았을 때 테마랑 따로 놀아서 하나하나 고치는 데 애를 먹었다&#128575; ~~그리고 여전히 엉망진창이다...~~


사실 이것도 시행착오를 겪은 거라 지금 이렇게 정리해놓은 게 맞는지 잘 모르겠다. 빼먹은 단계가 있을 수도 있고... (ㅠㅠ) 나중에 다시 확인해봐야겠다.

기능을 하나 추가하려면 수정해야 하는 게 너무너무 많다. 그래도 처음에 좀 고생하면 나중에는 편하게 쓸 수 있겠지! 라는 생각으로 열심히 해야겠다 &#128123;



## 참고

- [Jekyll Docs](https://jekyllrb.com/docs/posts/)
- [Jekyll 변수 설명](http://jekyllrb-ko.github.io/docs/variables/)


