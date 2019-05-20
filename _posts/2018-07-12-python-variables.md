---
layout:     post
title:      "Python에서 변수를 copy하고 싶을 때"
subtitle:   " "
date:       2018-07-12 10:46:00 +0900
background: '/img/universe.jpg'
comments:   true
categories: python
---


변수나 데이터를 copy해서 코드를 실행해봐야 하는 경우가 있다. 그런데 아래 코드처럼 실행하면,

```python
>>> list1 = [1,3,2,5,4]
>>> list2 = list1
>>> list2.sort()
>>> list2
[1, 2, 3, 4, 5]
>>> list1
[1, 2, 3, 4, 5]
```

분명히 copy한 list만 sorting는데 원래 데이터가 함께 바뀌는 억울한(?) 경험을 하게 된다.

이는 Python에서 값을 대입할 때 메모리를 새로 할당해주는 것이 아니라 기존의 변수가 가지고 있던 메모리의 주소를 공유하기 때문에 발생하는 문제이다. 이 문제는 두 가지 방법으로 해결할 수 있다.

<br>
### 1. copy 라이브러리 사용하기

copy 모듈의 deepcopy를 이용하여 변수를 copy할 수 있다.
```python
>>> from copy import deepcopy
>>> list1 = [1,3,2,5,4]
>>> list2 = deepcopy(list1)
>>> list2.sort()
>>> list2
[1, 2, 3, 4, 5]
>>> list1
[1, 3, 2, 5, 4]
```
<br>
### 2. Indexing

데이터 전체를 indexing하여 새로운 변수에 할당하는 경우에도 copy가 가능하다.

```python
>>> list1 = [1,3,2,5,4]
>>> list2 = list1[:] # 데이터 전체를 indexing한다.
>>> list2.sort()
>>> list2
[1, 2, 3, 4, 5]
>>> list1
[1, 3, 2, 5, 4]
```

두 번째 방법은 간단하긴 하지만 deep copy가 아닌 shallow copy이므로 주의해서 사용해야 한다.