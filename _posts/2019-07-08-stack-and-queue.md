---
layout:     post
title:      "Stack and Queue"
subtitle:   " "
date:       2019-07-09 11:45:00 +0900
background: '/img/universe.jpg'
comments:   true
categories: algorithms
---


### 1. Stack
<br>
```python
class myStack:

    class stackNode:
        def __init__(self, data, next):
            self.data = data
            self.next = next

    top = None

    def push(self, item):
        newItem = self.stackNode(item, self.top)
        self.top = newItem

    def pop(self):
        if self.top == None:
            raise Exception('The stack is empty')
        item = self.top.data
        self.top = self.top.next
        return item

    def peek(self):
        if self.top == None:
            raise Exception('The stack is empty')
        return self.top.data

    def isEmpty(self):
        return self.top == None
```

```python
stack = myStack()
```

```python
stack.push(1)
stack.push(2)
stack.push(3)
```

```python
stack.isEmpty()
```
```profile
False
```

```python
stack.pop()
```
```profile
3
```
```python
stack.pop()
```
```profile
2
```
```python
stack.peek()
```
```profile
1
```
```python
stack.pop()
```
```profile
1
```
```python
stack.pop()
```
```profile
---------------------------------------------------------------------------
Exception                                 Traceback (most recent call last)
<ipython-input-15-415460d3b717> in <module>()
----> 1 stack.pop()

<ipython-input-8-65cfc88e7d0c> in pop(self)
     14     def pop(self):
     15         if self.top == None:
---> 16             raise Exception('The Stack is Empty')
     17         item = self.top.data
     18         self.top = self.top.next

Exception: The stack is empty
```
```python
stack.isEmpty()
```
```profile
True
```




<br><br><br>
### 2. Queue
<br>
```python
class myQueue:

    class queueNode:
        def __init__(self, data, next):
            self.data = data
            self.next = next

    first = None
    last = None

    def add(self, item):
        newItem = self.queueNode(item, None)
        if self.last != None:
            self.last.next = newItem
        self.last = newItem
        if self.first == None:
            self.first = self.last

    def remove(self):
        if self.first == None:
            raise Exception('The queue is empty')
        item = self.first.data
        self.first = self.first.next
        if self.first == None:
            self.last == None
        return item

    def peek(self):
        if self.first == None:
            raise Exception('The queue is empty')
        return self.first.data

    def isEmpty(self):
        return self.first == None
```

```python
queue = myQueue()
```

```python
queue.add(1)
queue.add(2)
queue.add(3)
```

```python
queue.isEmpty()
```
```profile
False
```

```python
queue.remove()
```
```profile
1
```
```python
queue.remove()
```
```profile
2
```
```python
queue.peek()
```
```profile
3
```
```python
queue.remove()
```
```profile
3
```
```python
queue.remove()
```
```profile
---------------------------------------------------------------------------
Exception                                 Traceback (most recent call last)
<ipython-input-121-db71006a7f9a> in <module>()
----> 1 queue.remove()

<ipython-input-105-35281e437e06> in remove(self)
     20     def remove(self):
     21         if self.first == None:
---> 22             raise Exception('The queue is empty')
     23         item = self.first.data
     24         self.first = self.first.next

Exception: The queue is empty
```
```python
queue.isEmpty()
```
```profile
True
```




<br><br><br>
### 3. list를 활용하기

<br>


```python
stack = list()
```
```python
stack.append(1)
stack.append(2)
stack.append(3)
```
```python
stack.pop()
```
```profile
3
```
```python
stack.pop()
```
```profile
2
```
```
```python
stack.pop()
```
```profile
1
```
```python
stack.pop()
```
```profile
---------------------------------------------------------------------------
IndexError                                Traceback (most recent call last)
<ipython-input-43-415460d3b717> in <module>()
----> 1 stack.pop()

IndexError: pop from empty list
```


<br>

```python
queue = list()
```

```python
queue.append(1)
queue.append(2)
queue.append(3)
```

```python
queue.pop(0) # index에 해당하는 값을 return한다
```
```profile
1
```
```python
queue.pop(0)
```
```profile
2
```
```python
queue.pop(0)
```

```profile
3
```
```python
queue.pop(0)
```
```profile
---------------------------------------------------------------------------
IndexError                                Traceback (most recent call last)
<ipython-input-195-038138b33931> in <module>()
----> 1 queue.pop(0)

IndexError: pop from empty list
```

<br><br><br>
### 4. collections.deque
<br>
```python
from collections import deque
```
```python
stack = deque()
```
```python
stack.append(1)
stack.append(2)
stack.append(3)
```
```python
stack.pop()
```
```profile
3
```
```python
stack.pop()
```
```profile
2
```
```python
stack.pop()
```
```profile
1
```
```python
stack.pop()
```
```profile
---------------------------------------------------------------------------
IndexError                                Traceback (most recent call last)
<ipython-input-206-415460d3b717> in <module>()
----> 1 stack.pop()

IndexError: pop from an empty deque
```

<br>
```python
queue = deque()
```
```python
queue.append(1)
queue.append(2)
queue.append(3)
```
```python
queue.popleft()
```
```profile
1
```
```python
queue.popleft()
```
```profile
2
```
```python
queue.popleft()
```
```profile
3
```
```python
queue.popleft()
```
```profile
---------------------------------------------------------------------------
IndexError                                Traceback (most recent call last)
<ipython-input-216-e488fdbdaa60> in <module>()
----> 1 queue.popleft()

IndexError: pop from an empty deque
```
