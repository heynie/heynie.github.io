---
layout:     post
title:      "How to sort a dictionary"
subtitle:   ""
date:       2019-07-24 11:00:00 +0900
background: '/img/universe.jpg'
comments:   true
categories: python
---

#### key로 정렬하기
```python
import operator
x = {1: 2, 3: 4, 4: 3, 2: 1, 0: 0}
sorted_x = sorted(x.items(), key=operator.itemgetter(0))
print(sorted_x)
```
```profile
[(0, 0), (1, 2), (2, 1), (3, 4), (4, 3)]
```
<br><br>

#### value로 정렬하기

```python
import operator
x = {1: 2, 3: 4, 4: 3, 2: 1, 0: 0}
sorted_x = sorted(x.items(), key=operator.itemgetter(1))
print(sorted_x)
```
```profile
[(0, 0), (2, 1), (1, 2), (4, 3), (3, 4)]
```