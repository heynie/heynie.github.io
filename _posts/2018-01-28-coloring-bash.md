---
layout:     post
title:      "Coloring Bash"
subtitle:   "bash를 예쁘게 꾸며보자~"
date:       2018-01-28 18:10:00 +0900
background: '/img/universe.jpg'
comments:   true
categories: bash
---

최근에 ubuntu bash를 사용했는데 기본적으로 폰트에 색깔이 있는 것을 보고 내 bash도 꾸미고 싶어졌다. 색깔이 마음에 쏙 드는 건 아니지만 똑같은 색깔로 나오는 것보다는 보기 편해서 너무 좋다! 방법도 아주 간단하다.

<br>
## 1
*.bash_profile* 파일에 다음을 추가하고,

```buildoutcfg
export CLICOLOR=1
export LSCOLORS=gxfxcxdxbxegedabagaced
export GREP_OPTIONS='--color=auto'
PS1='\[\e[0;33m\]\u\[\e[0m\]@\[\e[0;32m\]\h\[\e[0m\]:\[\e[0;34m\]\w\[\e[0m\]\$ '
```
<br>

## 2
*.bashrc* 파일에 다음을 추가한다.
```buildoutcfg
source ~/.bash_profile
```
<br>
<img src="/img/coloring-bash.png" alt="coloring bash" width="100%">

완성!

<br>
## + 색깔 바꾸기

다른 거는 아직 잘 모르겠고 LSCOLORS를 바꾸는 법은 찾아냈다!

gxfxcxdxbxegedabagaced 이 값은 두 글자씩 세트인데, 앞글자는 글자색이고, 뒷글자는 배경색이다. 색상은 다음과 같고,

a: black<br>
b: red<br>
c: green<br>
d: brown<br>
e: blue<br>
f: magenta<br>
g: cyan<br>
h: light grey<br>
x: default foreground or background

a-h는 대문자로 쓰면 bold로 나타낼 수 있다.

또한 앞에서 두 글자씩 순서대로 다음을 나타낸다.

1. directory
2. symbolic link
3. socket
4. pipe
5. executable
6. block special
7. character special
8. executable with setuid bit set
9. executable with setgid bit set
10. directory writable to others, with sticky bit
11. directory writable to others, without sticky bit

즉  `LSCOLORS=Gx....`는 directory를 bold cyan으로 나타내겠다는 뜻이다.



<br><br>
참고: [[mac os x] ls 결과를 색깔로 구분해서 표시하기](http://cafecola.tistory.com/81)