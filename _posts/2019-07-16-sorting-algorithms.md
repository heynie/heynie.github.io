---
layout:     post
title:      "Sorting Algorithms"
subtitle:   " "
date:       2019-07-16 11:00:00 +0900
background: '/img/universe.jpg'
comments:   true
categories: algorithms
---

###### 정렬 알고리즘

- Bubble sort, insertion sort, selection sort: simple, slow
- Merge sort, heap sort: fast
- Radix sort: O(N)

<br><br>
### 1. Selection Sort


배열에서 가장 큰 값을 찾아 마지막 위치에 있는 값과 자리를 바꾸는 과정을 반복한다.

```java
selectionSort(A[], n)
{
    for last ← n downto 2 {                   // (1)
        A[1...last] 중 가장 큰 수 A[k]를 찾는다;    // (2)
        A[k] ⟷ A[last];                      // (3) A[k]와 A[last]의 값을 교환
    }
}
```

- 실행 시간
    - (1)의 for 루프는 n-1번 반복
    - (2)에서 가장 큰 수를 찾기 위한 비교 횟수: n-1, n-2, ..., 2, 1
    - (3)의 교환은 상수 시간 작업

- 시간복잡도 $$T(n) = (n-1) + (n-2) + ... + 2+ 1 = O(n^2)$$


<br><br>

### 2. Bubble Sort


1번째 값을 2번째 값과 비교해서 1번째 값이 더 크면 서로 자리를 바꾼다. 2번째 값과 3번째 값을 비교해서 2번째 값이 더 크면 서로 자리를 바꾼다. 이를 반복하여 (n-1)번째 값과 n번째 값을 비교해서 서로 자리를 바꾸는 것까지 완료하면 n번째 값은 그 자리에 고정시키고, 1번째 값부터 (n-1)번째 값에 대해서 같은 작업을 반복한다.
```java
bubbleSort(A[], n)
{
    for last ← n downto 2       // (1)
        for i ← 1 to last-1     // (2)
            if(A[i] > A[i+1]) then A[i]⟷A[i+1];  // (3)교환
}
```

- 수행시간
    - (1)의 for 루프는 n-1번 반복
    - (2)의 for 루프는 각각 n-1, n-2, ..., 2, 1번 반복
    - (3)의 교환은 상수시간 작업
- 시간복잡도 $$ T(n) = (n-1) + (n-2) + ... + 2 + 1 = O(n^2) $$


<br><br>

### 3. Insertion Sort

<img src="/img/algorithms/sorting-algorithms-insertion-sort.png" alt="insertion sort" width="100%">

2번째 값부터 시작하는데, 2번째 값이 1번째 값보다 작으면 그 앞으로 옮긴다. 그렇지 않으면 그대로 둔다. 3번째 값을 기준으로 잡고 앞의 두 값과 비교해서 순서가 정렬되는 지점에 값을 삽입한다. 이때 뒤쪽에 있는 값부터 비교하는 것이 좀 더 효율적이다. 이 과정을 반복해서 n번째 값까지 완료한다. 

```java
INSERTION-SORT(A)
    for j = 2 to A.length     // (1)
        key = A[j]
        // (2) Insert A[j] into the sorted sequence A[1..j-1].
        i = j - 1
        while i > 0 and A[i] > key
            A[i+1] = A[i]
            i = i - 1
        A[i+1] = key
```

- 수행시간
    - (1)의 for 루프는 n-1번 반복
    - (2)의 삽입은 최악의 경우 i-1번 비교
- 최악의 경우: $$ T(n) = (n-1) + (n-2) + ... + 2 + 1 = O(n^2)$$


<br>
## Divide and Conquer

- Divide: 해결하고자 하는 문제를 작은 크기의 동일한 문제들로 분할
- Conquer: 각각의 작은 문제를 순환적으로 해결
- merge sort와 quick sort가 이에 해당
    - merge: 작은 문제의 해를 합하여 원래 문제에 대한 해를 구함

<br><br>

### 4. Merge Sort


<!--![insertion sort](/img/algorithms/sorting-algorithms-insertion-sort.png)-->
<img src="/img/algorithms/sorting-algorithms-merge-sort.png" alt="merge sort" width="100%">

데이터가 저장된 배열을 절반으로 나누고 각각을 순환적으로 정렬한다. 정렬된 두 개의 배열을 합쳐서 전체를 정렬한다.

```java
MERGE-SORT(A,p,r)
    if p < r
        q = ⌊(p + r) / 2⌋
        MERGE-SORT(A,p,q)
        MERGE-SORT(A,q+1,r)
        MERGE(A,p,q,r)

MERGE(A,p,q,r)  // [p .. q .. r]
    n1 = q - p + 1
    n2 = r - q
    let L[1..n1+1] and R[1..n2+1] be new arrays
    for i = 1 to n1
        L[i] = A[p+i-1]
    for j = 1 to n2
        R[j] = A[q+j]
    L[n1+1] = ∞
    L[n2+1] = ∞
    i = 1
    j = 1
    for k = p to r
        if L[i] ≤ R[j]
            A[k] = L[i]
            i = i + 1
        else A[k] = R[j]
            j = j + 1
```

- 시간복잡도 
$$ T(n) = 
\begin{cases}
0 & \text{if $n=1$} \\
T(\lceil\frac n2 \rceil) + T(\lfloor \frac n2 \rfloor) + n & \text{otherwise}
\end{cases}
$$ 

<br><br>

### 5. Quick Sort

<img src="/img/algorithms/sorting-algorithms-quick-sort.png" alt="quick sort" width="50%">

- Divide: elements in lower parts ≤ elements in upper parts
- Conquer: 각 부분을 순환적으로 정렬
- Merge: nothing to do

하나의 값을 pivot으로 설정하여 pivot보다 작은 값들을 왼쪽으로, 큰 값들을 오른쪽으로 옮긴다. 따라서 merge sort에서처럼 배열이 완전히 반으로 나누어지지 않는다.

```python
def quickSort(A, p, r):
    if p < r:
        q = partition(A, p, r)
        quickSort(A, p, q-1)
        quickSort(A, q+1, r)

def partition(A, p, r):
    x = A[r]
    i = p - 1
    for j in range(p, r):
        if A[j] <= x:
            i += 1
            A[i], A[j] = A[j], A[i]
    A[i+1], A[r] = A[r], A[i+1]
    return i+1
```

- Quick sort의 시간 복잡도는 최선의 경우와 최악의 경우에 따라 매우 다르다.
- Worst case: 배열이 이미 정렬되어 있을 때 (항상 한 쪽은 0개, 다른 쪽은 n-1개로 분할되는 경우)
    - $$\theta(n)$$은 분할하는 데 걸리는 시간


$$
\begin{align}
T(n) & = T(0) + T(n-1) + \theta(n) \\
& = T(n-1) + \theta(n) \\
& = T(n-2) + T(n-1) + \theta(n-1) + \theta(n) \\
& ... \\
& = \theta(1) + \theta(2) + ... + \theta(n-1) + \theta(n) \\
& = \theta(n^2)
\end{align}
$$

- Best case: 항상 절반으로 분할되는 경우

$$
\begin{align}
T(n) & = 2T(\frac n2) + \theta(n) \\
& = \theta(n\log{n})
\end{align}
$$

- Balanced partition: 항상 한쪽이 적어도 1/9 이상이 되도록 분할된다면?

<img src="/img/algorithms/sorting-algorithms-quick-sort-tree.png" alt="quick sort tree" width="100%">

가장 오른쪽 tree가 가장 깊은 tree인데,
$$ (\frac{9}{10})^kn = 1$$이 되는 $$k=\log_{\frac{9}{10}}{n}$$이다. 따라서 시간 복잡도는 $$\theta(\log n)$$이다. 분할이 아주 극단적으로 이루어지지만 않으면 충분히 빠른 알고리즘이다.

| rank of pivot | probability | result of partition | running time |
|---|---|---|---|
|$$1$$|$$\frac 1n$$|$$0 : n-1$$|$$A(0) + A(n-1)$$|
|$$2$$|$$\frac 1n$$|$$1 : n-2$$|$$A(1) + A(n-2)$$|
|$$3$$|$$\frac 1n$$|$$2 : n-3$$|$$A(2) + A(n-3)$$|
|...|...|...|...|
|$$n-1$$|$$\frac 1n$$|$$n-2 : 1$$|$$A(1) + A(n-2)$$|
|$$n$$|$$\frac 1n$$|$$n-1 : 0$$|$$A(0) + A(n-1)$$|


평균 시간복잡도 $$ A(n) = \frac 2n \sum_{i=0}^{n-1}A(i) + \theta(n) = \theta(n\log_2n) $$

<br><br>

### 6. Heap Sort

- 최악의 경우 시간 복잡도 $$O(n\log_2n)$$
- in-place sorting: 추가 배열이 필요하지 않음 (merge sort같은 경우에는 추가 배열을 사용했음)
- Binary heap 자료구조를 사용

<br>
##### Heap

(1) Complete binary tree
<!--![complete binary tree](/img/algorithms/sorting-algorithms-complete-binary-tree.png){:.aligncenter}-->
- 마지막 레벨을 제외하면 완전히 꽉 차있고, 마지막 레벨에는 가장 오른쪽부터 연속된 몇 개의 노드가 비어있을 수 있음. 
- Full binary tree는 모든 레벨에 노드들이 꽉 차있는 형태로, full binary tree는 complete binary tree이지만 역은 반드시 성립하지는 않는다.

(2) Heap property
- Max heap property: 부모가 자식보다 크거나 같음 (아 그림은 max heap)
- Min heap property: 부모가 자식보다 작거나 같음

Heap은 (1)과 (2)의 조건을 동시에 만족해야 한다. 또한 동일한 데이터로도 여러 개의 서로 다른 heap을 만들 수 있다.

<img src="/img/algorithms/sorting-algorithms-heap.png" alt="heap" width="100%" align="center">

Heap은 1차원 배열로 표현할 수 있는데, root node를 인덱스 1로 하여 순서대로 저장하면 된다. Heap이 complete binary tree이기 때문에 데이터를 순서대로 배열에 저장해도 노드의 부모-자식 관계를 알 수 있다.
- $$A[i]$$의 parent: $$A[i/2]$$
- $$A[i]$$의 left child: $$A[2i]$$
- $$A[i]$$의 right child: $$A[2i+1]$$

```python
import math

def parent(i):
    return i/2

def left(i):
    return 2*i

def right(i):
    return 2*i+1
```

<br>

##### Max-heapify

root node를 제외하고 heap property를 만족할 때 (root node가 child node보다 작은 값일 때) max-heapify 연산을 한다. 두 자식들 중 더 큰 쪽이 그 node보다 크면 exchange한다.

```python
def maxHeapify(A, i):
    l = left(i)
    r = right(i)
    if l < len(A) and A[l] > A[i]:  # l ≤ len(A)
        largest = l
    else:
        largest = i
    if r < len(A) and A[r] > A[largest]:  # r ≤ len(A)
        largest = r
    if largest != i:
        A[i], A[largest] = A[largest], A[i]
    maxHeapify(A, i)
```

heap의 높이를 $$h$$라 했을 때 node를 exchange하는 횟수가 $$h$$보다 커질 수 없으므로 maxHeapify의 시간복잡도는 $$O(h)$$이다. Heap은 complete binary tree이므로, node의 개수를 $$n$$이라고 했을 때 $$h = \theta(\log n)$$, 즉 시간복잡도는 $$O(\log n)$$이다.

<br>

##### Build max heap

(1) Complete binary tree를 heap으로 바꾼다.
마지막 인덱스부터 subtree가 heap인지 확인한다. Heap이 아니면 heapify를 한다.

<img src="/img/algorithms/sorting-algorithms-build-max-heap.png" alt="build max heap" width="100%" align="center">

```python
def buildMaxHeap(A):
    heapSize[A] = len(A)  # 정렬할 데이터의 개수 (n)
    for i in reversed(range(1, math.floor(len(A)/2)+1)):
        maxHeapify(A, i)
```
시간복잡도는 $$O(n)$$이다.

<br>

##### Heapsort
(1) 주어진 데이터로 heap을 만든다.
(2) Heap에서 최댓값(root)을 가장 마지막 값과 바꾼다.
(3) Heap의 크기가 1 줄어든 것으로 간주한다. 즉, 가장 마지막 값은 힙의 일부가 아닌 것으로 간주한다.
(4) Root node에 대해서 heapify(1)한다.
(5) (2)-(4)를 반복한다.

<img src="/img/algorithms/sorting-algorithms-heapsort.png" alt="heapsort" width="100%" align="center">

```python
def heapSort(A):
    buildMaxHeap(A)                         # O(n)
    heap_size = len(A)
    for i in reversed(range(2, len(A))):    # (n-1) times
        A[1], A[i] = A[i], A[1]             # O(1)
        heap_size = heap_size - 1           # O(1)
        maxHeapify(A, 1)                    # O(logn)
```
시간 복잡도: $$O(n \log_2 n)$$

<!--A = [2, 8, 6, 1, 11, 15, 3, 12, 10]-->
