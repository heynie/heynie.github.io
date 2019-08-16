---
layout:     post
title:      "DFS BFS Examples"
subtitle:   "boj"
date:       2019-08-12 15:00:00 +0900
background: '/img/universe.jpg'
comments:   true
categories: algorithms
---


## (1260) DFS와 BFS

Python에서 재귀를 사용하면 재귀 제한 때문에 런타임 에러가 생긴다. 스택으로 바꿔서 구현하거나, sys.setrecursionlimit()를 통해 제한 범위를 늘리면 된다.

```python
import sys
sys.setrecursionlimit(10**6)

def findAdjacent(edges, v):
    adjacent = list(filter(lambda x: x[0] == v or x[1] == v, edges))
    adj = []
    for v1, v2 in adjacent:
        vertex = v1 if v1 != v else v2
        adj.append(vertex)
    return sorted(adj)

def DFS(edges, v):
    visited[v-1] = 1
    visitedNode.append(v)
    adj = findAdjacent(edges, v)
    for a in adj:
        if visited[a-1] == 0:
            DFS(edges, a)

def BFS(edges, v):
    while queue != []:
        if visited[v-1] == 0:
            visited[v-1] = 1
            visitedNode.append(v)
            queue.remove(v)
            adj = findAdjacent(edges, v)
            for a in adj:
                if visited[a-1] == 0 and a not in queue:
                    queue.append(a)
            for vertex in queue:
                BFS(edges, vertex)


if __name__ == "__main__":

    N, M, V = map(int, input().split())

    edges = []
    for _ in range(M):
        edges.append(tuple(map(int, input().split())))

    visited = [0 for _ in range(N)]
    visitedNode = []
    DFS(edges, V)
    print(' '.join(list(map(str, visitedNode))))

    visited = [0 for _ in range(N)]
    queue = [V]
    visitedNode = []
    BFS(edges, V)
    print(' '.join(list(map(str, visitedNode))))
```


<br>

## (11724) 연결 요소의 개수

DFS로 노드를 탐색하면서 끊어지는 지점에서 카운트를 하면 된다. 시간초과가 된다면 Python대신 PyPy로 제출하거나 input() 함수를 sys.stdin.readline()으로 바꿔보자.

```python
import sys
sys.setrecursionlimit(10**6)

def DFS(edges, v):
    visited[v-1] = 1
    for a in adj[v-1]:
        if visited[a-1] == 0:
            DFS(edges, a)


if __name__ == "__main__":

    N, M = map(int, sys.stdin.readline().split())

    edges = []
    for _ in range(M):
        edges.append(tuple(map(int, sys.stdin.readline().split())))

    adj = [[] for _ in range(N)]
    for v1, v2 in edges:
        adj[v1-1].append(v2)
        adj[v2-1].append(v1)

    visited = [0 for _ in range(N)]
    cnt = 0
    while 0 in visited:
        V = visited.index(0) + 1
        DFS(edges, V)
        cnt += 1
    print(cnt)
```

<br>

## (1707) 이분 그래프

노드마다 색깔을 WHITE와 BLACK으로 바꿔가며 표시하고, 만약 인접 노드의 색깔이 현재 노드의 색깔과 같다면 'NO'를 출력한다. 색깔을 체크하고 바꾸는 시점 때문에 조금 헷갈렸던 문제다.

```python
import sys
sys.setrecursionlimit(10**6)

def DFS(edges, v, color):
    global visited, bipartite, succeed
    visited[v-1] = 1
    color = 'WHITE' if color == 'BLACK' else 'BLACK'
    bipartite[v-1] = color
    for a in adj[v-1]:
        if bipartite[a-1] == color:
            succeed = 0
            break
        else:
            if visited[a-1] == 0:
                DFS(edges, a, color)


if __name__ == "__main__":

    T = int(input())
    for _ in range(T):
        N, M = map(int, sys.stdin.readline().split())

        edges = []
        for _ in range(M):
            edges.append(tuple(map(int, sys.stdin.readline().split())))

        adj = [[] for _ in range(N)]
        for v1, v2 in edges:
            adj[v1-1].append(v2)
            adj[v2-1].append(v1)

        visited = [0 for _ in range(N)]
        bipartite = [0 for _ in range(N)]
        succeed = 1
        color = 'BLACK'
        while 0 in visited:
            V = visited.index(0) + 1
            DFS(edges, V, color)
            if succeed == 0:
                break
        if succeed:
            print('YES')
        else:
            print('NO')
```

<br>

## (10451) 순열 사이클

문제를 읽어보면 (11724) 연결 요소의 개수와 같은 문제라는 것을 알 수 있다.

```python
import sys
sys.setrecursionlimit(10**6)

def DFS(edges, v):
    visited[v-1] = 1
    for a in adj[v-1]:
        if visited[a-1] == 0:
            DFS(edges, a)


if __name__ == "__main__":

    T = int(sys.stdin.readline())
    for _ in range(T):
        N = int(sys.stdin.readline())
        sequence = list(map(int, sys.stdin.readline().split()))
        edges = list(zip(range(1, N+1), sequence))

        adj = [[] for _ in range(N)]
        for v1, v2 in edges:
            adj[v1-1].append(v2)
            adj[v2-1].append(v1)

        visited = [0 for _ in range(N)]
        cnt = 0
        while 0 in visited:
            V = visited.index(0) + 1
            DFS(edges, V)
            cnt += 1
        print(cnt)
```

<br>

## (2331) 반복수열

이건 그냥 반복문으로 풀었다. 다른 방법이 있나?

```python
def calCipher(n, powerTen):
    return n//(10**powerTen) - n//10**(powerTen+1)*10

N, P = map(int, input().split())
D = [N]
while True:
    i = 0
    nextNum = 0
    for i in range(len(str(N))):
        add = calCipher(N, i)
        nextNum += add**P
    if nextNum in D:
        break
    D.append(nextNum)
    N = nextNum
print(D.index(nextNum))
```

<br>

## (9466) 텀 프로젝트

시간초과가 났다.

```python
import sys

T = int(sys.stdin.readline())
for _ in range(T):
    N = int(sys.stdin.readline())
    sequence = list(map(int, sys.stdin.readline().split()))
    visited = [0 for _ in range(N)]
    solo = []
    while 0 in visited:
        v = visited.index(0) + 1
        visitedNum = []
        while v not in visitedNum:
            visited[v-1] = 1
            visitedNum.append(v)
            v = sequence[v-1]
        idx = visitedNum.index(v)
        solo += visitedNum[:idx]
    print(len(set(solo)))
```

<br>

## (2667) 단지 번호 붙이기

```python
def DFS(x, y):
    global cnt, flag
    town[x][y] = flag
    cnt += 1
    if x >= 0 and x < N-1:
        if town[x+1][y] == 1:
            DFS(x+1, y)
    if x > 0 and x < N:
        if town[x-1][y] == 1:
            DFS(x-1, y)
    if y >= 0 and y < N-1:
        if town[x][y+1] == 1:
            DFS(x, y+1)
    if y > 0 and y < N:
        if town[x][y-1] == 1:
            DFS(x, y-1)


if __name__ == "__main__":
    
    N = int(input())
    town = []
    for _ in range(N):
        town.append(list(map(int, ' '.join(input()).split())))
        
    numApart = []
    flag = 2
    for x in range(N):
        for y in range(N):
            if town[x][y] == 1:
                cnt = 0
                DFS(x, y)
                numApart.append(cnt)
                flag += 1
                
    print(len(numApart))
    for i in sorted(numApart):
        print(i)
```

<br>

## (4963) 섬의 개수
2667번 문제에서 &emsp;1) 높이, 너비 조건 &emsp;2) 대각선 조건을 추가해주면 된다.

```python
import sys
sys.setrecursionlimit(10**6)

def DFS(x, y):
    global cnt, flag
    land[x][y] = flag
    cnt += 1
    if x >= 0 and x < H-1:
        if land[x+1][y] == 1:
            DFS(x+1, y)
        if y >= 0 and y < W-1:
            if land[x+1][y+1] == 1:
                DFS(x+1, y+1)
        if y > 0 and y < W:
            if land[x+1][y-1] == 1:
                DFS(x+1, y-1)
    if x > 0 and x < H:
        if land[x-1][y] == 1:
            DFS(x-1, y)
        if y >= 0 and y < W-1:
            if land[x-1][y+1] == 1:
                DFS(x-1, y+1)
        if y > 0 and y < W:
            if land[x-1][y-1] == 1:
                DFS(x-1, y-1)
    if y >= 0 and y < W-1:
        if land[x][y+1] == 1:
            DFS(x, y+1)
    if y > 0 and y < W:
        if land[x][y-1] == 1:
            DFS(x, y-1)


while True:
    W, H = map(int, sys.stdin.readline().split())
    if W == 0 and H == 0:
        break
    land = []
    for _ in range(H):
        land.append(list(map(int, sys.stdin.readline().split())))

    flag = 2
    for x in range(H):
        for y in range(W):
            if land[x][y] == 1:
                cnt = 0
                DFS(x, y)
                flag += 1
    print(flag-2)
```


<br>

## (7576) 토마토

이건 BFS로 푸는 문제이다. Queue를 사용해서 풀면 되고, 재귀를 사용하면 시간초과가 뜬다.

```python
import sys


def ripen(day):
    global box, move, tomatoes
    while tomatoes != []:
        length = len(tomatoes)
        for i in range(length):
            x, y = tomatoes[i]
            for dx, dy in move:
                next_x = x + dx
                next_y = y + dy
                if next_x < 0 or next_x >= N or next_y < 0 or next_y >= M:
                    continue
                if box[next_x][next_y]:
                    continue
                box[next_x][next_y] = day + 1
                tomatoes.append((next_x, next_y))
        tomatoes = tomatoes[length:]
        day += 1
        

if __name__ == "__main__":
    M, N = map(int, sys.stdin.readline().split())
    box = []
    for _ in range(N):
        box.append(list(map(int, sys.stdin.readline().split())))

    move = [(-1,0), (1,0), (0,-1), (0,1)]
    tomatoes = []
    for x in range(N):
        for y in range(M):
            if box[x][y] == 1:
                tomatoes.append((x,y))
    ripen(1)

    if 0 in [0 for row in box if 0 in row]:
        print(-1)
    else:
        minDays = max([max(row) for row in box]) - 1
        print(minDays)
```

<br>