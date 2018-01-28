---
layout:     post
title:      "Creating Github Pages with Jekyll"
subtitle:   "블로그를 만들어 보자!"
date:       2018-01-13 20:20:20 +0900
background: '/img/universe.jpg'
comments:   true
categories: etc
---


우여곡절 끝에 블로그를 완성했다! 여러 시행착오를 겪은 만큼 과정을 간단하게 정리해놓아야겠다. 다음에 헤매지 않게! &#128534;

<br>

# Github Pages와 Jekyll

블로그에도 많은 종류가 있다. 가장 먼저 떠오르는 네이버 블로그, 구글링하면서 자주 들어가게 되는 티스토리 등등 다양한 종류가 있는데 나는 마크다운으로 깔끔하게 내용을 작성하고 싶어서 Github Pages를 선택했다. 블로그 선택을 하기 전에 구글링을 많이 해보았는데, [여기](http://blog.kalkin7.com/2015/07/07/maintain-a-blog-for-a-long-time/)에서 깔끔하게 정리된 설명을 참고할 수 있다.

Github pages로 블로그를 만들고 포스트를 작성하려면 마크다운 파일을 작성하고, github에 올리기만 하면 되므로 매우 간단하다. 만약 Git 사용법을 모른다면 [누구나 쉽게 이해할 수 있는 Git 입문](https://backlog.com/git-tutorial/kr/)을 참고하면 된다.

블로그를 꾸미기 위해서 jekyll을 이용하였다. jekyll 테마를 다운로드 받아 사용하면 내가 일일이 디자인을 하지 않아도 되기 때문이다. 원한다면 customizing을 할 수도 있다. [jekyll themes](http://jekyllthemes.org)에서 마음에 드는 테마를 골라 다운로드 받거나, 해당 테마의 github repository로 이동하여 fork하면 된다.

<br>

## 1. Github에서 new repository 생성

repository 이름은 `<username>.github.io`로 만들면 된다. 그러면 자신의 블로그가 `https://<username>.github.io`라는 주소에 개설되는 것이다!

<br>

## 2. Jekyll 적용하기

나는 막 초기화를 마친 Mac OS High Sierra에서 만들기 시작했고, jekyll을 적용하기 위해 homebrew 설치부터 시작하였다. 또한 [Jekyll을 사용하여 GitHub Pages 만들기](http://blog.saltfactory.net/upgrade-github-pages-dependency-versions/)를 참고하면 더욱 자세한 설명을 볼 수 있다.

<br>


#### (1) Install Homebrew

```buildoutcfg
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
<br>

#### (2) Install RVM
```buildoutcfg
$ curl -sSL https://get.rvm.io | bash -s stable --ruby
```
<br>

#### (2-1) Update Ruby
mac에 내장되어 있는 ruby가 있는데, 나의 경우 나중에 gemfile을 설치할 때 버전 업데이트가 필요했기 때문에 다시 install했다. (Skip해도 됨)
```buildoutcfg
$ rvm install ruby-2.2.5
```
```buildoutcfg
$ rvm --default use 2.2.5
```
<br>

#### (3) Install Jekyll
```buildoutcfg
$ gem install jekyll
```
<br>

#### (4) 테마 적용

fork한 repository를 clone하거나, 다운로드 받은 파일을 unzip하여 내 local 폴더로 옮긴다. 여기에 들어있는 gemfile을 적용하기 위해 `$ bundle install`을 해야 하는데, 나와 같은 초기 상태라면 `-bash: bundle: command not found`라는 에러가 발생할 것이다. `$ gem install bundler`를 입력하면 해결될 것이다.

<br>

#### (5) 서버 실행

remote repository로 push하기 전에 local에서 블로그를 구동시킬 수 있다.
```buildoutcfg
$ jekyll server --watch
```
를 실행하면 bash에 서버 주소가 나온다. 이동하여 확인해보자.

<br>

#### (6) 커스터마이징

이제 원하는대로 커스터마이징을 할 수 있다! 기본 정보는 *_config.yml*파일에서 수정하면 될 것이다. 나는 웹 프로그래밍을 아주 조금 배워본 적이 있는데 어렴풋이... 기억하고 있다... ~~CSS가 제일 귀찮고 어렵다는 것을...~~

<br>

#### 3. Post 작성하기

post는 html로 작성할 수도 있고, markdown으로 작성할 수도 있다. 나는 마크다운이 더 편하기 때문에! 앞으로도 마크다운을 사용할 것이다.

*_posts*폴더에 새로운 파일을 생성하여 자유롭게 작성하면 된다. 단 지켜야할 형식이 있다.

1. 제목은 **YYYY-MM-DD-\<filename\>.md**

2. 내용을 작성하기 전, 파일의 맨 위에 YAML front matter를 입력하여 post 정보를 등록해야 한다.

```buildoutcfg
---
layout:     post
title:      "<TITLE>"
date:       YYYY-MM-DD HH:MM:MM +0900
background: '<imgPATH>'
---
```

처음할 때는 포스트가 제대로 안 올라가고 하는 문제가 있었는데, 어느 순간부터 잘 돼서... 모르겠다...ㅎ 
[Jekyll Basic for Beginners (초보를 위한 지킬 사용의 기본)](https://www.youtube.com/watch?time_continue=363&v=5Td33bQPLf4) 이 동영상을 보면 전체적인 내용을 한눈에 알 수 있다.

그밖에 post를 풍성하게 만들기 위해 참고할 수 있는 링크들이 있다.
- [jekyll로 Github Page 만들기](http://alex.devpools.kr/2017/03/16/jekyll로-github-page-만들기/)
- [Emoji](https://steemit.com/steemkr-guide/@snow-airline/steemkr-quick-start-guide)

앞으로 얼마나 부지런히 작성할 수 있을지 모르겠지만! 열심히 해봐야지 &#128035;

<br>

##### 오늘 한 일
- pages 생성
- google search 신청

##### 앞으로 할 일
- disqus 적용
- category 추가하기
