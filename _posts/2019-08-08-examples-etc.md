---
layout:     post
title:      "Examples"
subtitle:   "boj"
date:       2019-08-08 17:40:00 +0900
background: '/img/universe.jpg'
comments:   true
categories: algorithms
---

그 외 문제들

<br>

## (2751) 수 정렬하기 2
N개의 수가 주어졌을 때, 이를 오름차순으로 정렬하는 프로그램을 작성하시오.

파이썬에서 흔히 입력을 input()으로 받지만 sys 모듈의 sys.stdin.readline()을 이용하면 속도가 더 빨라진다. 이 문제에서는 코드는 똑같지만 입력을 받는 함수로 input()을 사용하면 시간초과가 뜨고 sys.stdin.readline()을 사용하면 통과할 수 있다.
```python
import sys

N = int(sys.stdin.readline())
numbers = []
for _ in range(N):
    numbers.append(int(sys.stdin.readline()))
print(*sorted(numbers), sep='\n') # *로 list의 data만 가져올 수 있다
```

<br>
## (11650) 좌표 정렬하기

이 문제는 input()으로도 통과하는데 시간이 4400ms이 넘고 sys.stdin.readline()을 사용하면 500ms 만에 통과한다.

```python
import sys

N = int(sys.stdin.readline())
coord = []
for _ in range(N):
    coord.append(list(map(int, sys.stdin.readline().split())))
for c in sorted(coord):
    c = list(map(str, c))
    print(' '.join(c))
```

<br>

## (11651) 좌표 정렬하기 2
sorted() 함수를 사용하면 리스트 내에 리스트 또는 튜플이 있을 때 자동으로 첫 번째 원소를 기준으로 정렬을 하는데, 두 번째 원소부터 정렬을 하려면 key값을 lambda 함수로 지정하여 사용할 수 있다.

```python
import sys

N = int(sys.stdin.readline())
coord = []
for _ in range(N):
    coord.append(list(map(int, sys.stdin.readline().split())))
for c in sorted(coord, key=lambda element: (element[1], element[0])):
    print(' '.join(list(map(str, c))))
```