---
layout:     post
title:      "Recursion"
subtitle:   " "
date:       2019-07-10 11:00:00 +0900
background: '/img/universe.jpg'
comments:   true
categories: algorithms
---


[인프런 강좌: 순환(Recursion)의 개념과 기본 예제](https://www.inflearn.com/course/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EA%B0%95%EC%A2%8C/)를 참고하였다.


Recursion(재귀함수)이란 자기 자신을 호출하는 함수를 말한다.
```java
void func(...){
    ...
    func(...);
    ...
}
```

<br>
Recursion이 무한루프에 빠지지 않으려면 recursive case와 함께 recursion에 빠지지 않는 base case를 만들어줘야 한다. 이때 recursion을 반복하다가 결국 base case로 수렴해야 한다.


```java
public class Code02{
    public static void main(String [] args) {
    int n = 4;
    func(n);
    }
    
    public static void func(int k){
        if (k <= 0)
            return;   // base case
        else {
            System.out.println("Hello...");
            func(k-1);   // recursive case
        }
    }
}
```

<br>
###### 1. Factorial


0! = 1  <br>
n! = n × (n-1)! &nbsp;&thinsp;&ensp;&emsp;n>0

```java
public static int factorial(int n)
{
    if (n==0)
        return 1;
    else
        return n * factorial(n-1);
}
```

<br><br>

###### 2. Fibonacci Number


f<sub>0</sub> = 0  
f<sub>1</sub> = 1  
f<sub>n</sub> = f<sub>n-1</sub> + f<sub>n-2</sub>&nbsp;&thinsp;&ensp;&emsp; n>1

```java
public int fibonacci(int n){
    if (n<2)
        return n;
    else
        return fibonacci(n-1) + fibonacci(n-2);
}
```

<br>
###### 3. 최대공약수: Euclid Method

m ≥ n인 두 양의 정수 m과 n에 대해서 m이 n의 배수이면 gcd(m, n) = n이고, 그렇지 않으면 gcd(n, m%n)이다.

```java
public static int gcd(int m, int n) {
    if (m<n) {
        int tmp=m; m=n; n=tmp;       // swap m and n
    }
    if (m%n == 0)
        return n;
    else
        return gcd(n, m%n);
}
```
```java
public static int gcd(int p, int q){
    if (q == 0)
        return p;
    else
        return gcd(q, p%q);
}
```

<br>
<br><br>
수학함수뿐 아니라 다른 많은 문제들을 recursion으로 해결할 수 있다.
<br><br>

###### 1. 문자열의 길이 계산
```
if the string is empty
    return 0;
else
    return 1 + (the length of the string that excludes the first character)
```
```java
public static int length(String str){
    if (str.equals(""))
        return 0;
    else
        return 1 + length(str.substring(1));
}
```


<br>
###### 2. 문자열의 프린트

```java
public static void printChars(String str){
    if (str.length()==0)
        return;
    else {
        System.out.print(str.charAt(0));  // 첫 번째 글자를 출력
        printChars(str.substring(1));  // 첫 글자를 제외한 나머지 문자열을 출력
    }
}
```

<br>
<br>

###### Recursion vs. Iteration
- 모든 순환함수는 반복문(iteration)으로 변경 가능
- 그 역도 성립함. 즉 모든 반복문은 recursion으로 표현 가능함
- 순환 함수는 복잡한 알고리즘을 단순하고 알기 쉽게 표현하는 것을 가능하게 함
- 하지만 함수 호출에 따른 오버헤드가 있음 (매개변수 전달, 액티베이션 프레임 생성) 속도가 더 느려질 수 있음

<br><br><br><br>



### Designing Recursion: 순환 알고리즘의 설계
<br>
암시적(implicit) 매개변수를 명시적(explicit) 매개변수로 바꾸어라.
<br><br>
###### 순차 탐색
```c
int search(int [] data, int n, int target) {
    for (int i=0; i<n; i++)
        if (data[i] == target)
            return i;
    return -1
}
```
```c
int search(int [] data, int begin, int end, int target) {
    if (begin > end)
        return -1;
    else if (target == data[begin])
        return begin;
    else
        return search(data, begin+1, end, target);
}
```

<br><br>


##### 미로찾기 문제

현재 위치에서 출구까지 가는 경로가 있으려면

1) 현재 위치가 출구이거나 혹은

2) 이웃한 셀들 중 하나에서 <u>현재 위치를 지나지 않고</u> 출구까지 가는 경로가 있거나
<br><br>
###### Decision Problem (답이 yes/no인 문제)

```
boolean findPath(x, y)
    if (x,y) is the exit
        return true;
    else
        for each neighbouring cell (x', y') of (x, y) do
            if (x', y') is on the pathway and not visited
                if findPath(x', y')
                    return true;
        return false;
```
```java
public class Maze {
    private static int N=8;
    private static int [][] maze = {
    }

    private static final int PATHWAY_COLOUR = 0;    // white
    private static final int WALL_COLOUR = 1;       // blue
    private static final int BLOCKED_COLOUR = 2;    // red
    private static final int PATH_COLOUR = 3;       // green
    
    public static boolean findMazePath(int x, int y) {
        if (x<0 || y<0 || x>=N || y >=N)
            return false;
        else if (maxe[x][y] != PATHWAY_COLOUR)
            return false;
        else if (x==N-1 && y==N-1) {
            maze[x][y] = PATH_COLOUR;
            return true;
        }
        else {
            maze[x][y] = PATH_COLOUR;
            if (findMazePath(x-1, y) || findMazePath(x, y+1) || findMazePath(x+1, y) || findMazePath(x, y-1)) {
                return true;
            }
            maze[x][y] = BLOCKED_COLOUR;      // dead end
            return false;
        }
    }
    
    public static void main(String [] args) {
        printMaze();
        findMazePath(0,0);
        printMaze();
    }
}
```
<br><br>

###### Counting Cells in a Blob

input
- N*N 크기의 2차원 grid
- 하나의 좌표 (x,y)

output:
- 픽셀 (x,y)가 포함된 blob의 크기
- (x,y)가 어떤 blob에도 속하지 않는 경우에는 0

Recursive Thinking
현재 픽셀이 속한 blob의 크기를 카운트하려면
- 현재 픽셀이 image color가 아니라면
    - 0을 반환한다.
- 현재 픽셀이 image color라면
    - 먼저 현재 픽셀을 count한다 (count=1)
    - 현재 픽셀이 중복 카운트되는 것을 방지하기 위해 다른 색으로 칠한다.
    - 현재 픽셀에 이웃한 모든 픽셀들에 대해서
        - 그 픽셀이 속한 blob의 크기를 카운트하여 카운터에 더해준다.
    - 카운터를 반환한다.

```java
// Java
private static int BACKGROUND_COLOR = 0;
private static int IMAGE_COLOR = 1;
private static int ALREADY_COUNTED = 2;

public int countCells(int x, int y){
    if (x<0 || x>=N || y<0 || y>=N)
        return 0;
    else if (grid[x][y] != IMAGE_COLOR)
        return 0;
    else {
        grid[x][y] = ALREADY_COUNTED;
        return 1 + countCells(x-1, y+1) + countCells(x, y+1) + countCells(x+1, y+1) + countCells(x-1, y) + countCells(x+1, y) + countCells(x-1, y-1) + countCells(x, y-1) + countCells(x+1, y-1) 
    }
}
```

```python
# Python
def countCells(grid, x, y, count):
    if x < 0 or y < 0 or x >= len(grid) or y >= len(grid[0]):
        return 0
    elif grid[x][y] != 1:
        return 0
    else:
        grid[x][y] = 2;
        return 1 + countCells(grid, x-1, y, count) + countCells(grid, x, y+1, count) + countCells(grid, x, y-1, count) + countCells(grid, x, y+1, count) + countCells(grid, x+1, y-1, count) + countCells(grid, x+1, y, count) + countCells(grid, x+1, y+1, count)
```


<br><br>

###### N-Queens Problem

N*N 체스판에서 N개의 queen을 가로, 세로, 대각선 어느 방향으로도 충돌하지 않도록 놓는 방법이 존재하는지 알아보는 문제이다. Backtracking의 대표적인 예제라고 할 수 있다.

Backtracking이란 모든 경우의 수를 탐색하는 방법인데, 쉽게 말하면 어떤 길로 가다가 막다른 길을 만났을 때 다시 되돌아가서 다른 길을 탐색하는 것이다. DFS(깊이우선탐색)과 비슷하지만, DFS는 모든 경로를 탐색하는 데 반해 backtracking은 가지치기를 통해 불필요한 경로를 차단하기 때문에 모든 경로를 탐색하지 않는다는 차이가 있다. 이 문제에서는 queen을 놓을 자리를 탐색하는데 이미 이전에 놓은 queen과 충돌이 일어난다면 그 경우의 수는 고려하지 않는 것이다.
```java
int [] cols = new int [N+1];
boolean queens(int level)
// 매개변수 level은 현재 노드의 행을 표현하고, 1번에서 level말이 어디에 놓였는지는 전역변수 배열 cols로 표현하자. cols[i] = j는 i번 말이 (i행, j열)에 놓였음을 의미한다.
// cols = [1,4,2,3,...]
{
    if(!promising(level))
        return false;
    else if (level==N) // promising 테스트를 통과했다는 가정 하에 level==N이면 모든 말이 놓였다는 의미, 즉 성공
        for (int i=1; i<=N; i++)
            System.out.println("(" + i ", " + cols[i] + ")");
        return true;
    for (int i=1; i<=N; i++){
        cols[level+1] = i;
        if (queens(level+1))
            return true;
    }
    return false;
}

boolean promising( int level )
{
    for (int i=1; i<level; i++){
        if (cols[i] == cols[level]) // 현재 놓은 queen 이전 행들의 queen과 같은 열에 놓였는지 확
            return false;
        else if (level-i == Math.abs(cols[level] - cols[i]))
            return false;
    }
}

// 처음에는 queens(0)으로 호출한다.
```

<br><br><br>


### 멱집합 Powerset

{a,b,c,d,e,f}의 부분집합은 {b,c,d,e,f}의 부분집합에 {a}를 추가한 집합과 {a}를 추가하지 않은 집합으로 나눌 수 있다.
{b,c,d,e,f}의 모든 부분집합에 {a}를 추가한 집합들을 나열하려면 {c,d,e,f}의 모든 부분집합들에 {a}를 추가한 집합들을 나열하고 {c,d,e,f}의 모든 부분집합에 {a,b}를 추가한 집합들을 나열한다.

```java
// data[k], ..., data[n-1]의 멱집합을 구한 후 각각에 include[i]=true, i=0, ..., k-1인 원소를 추가하여 출력하라.
// 처음 이 함수를 호출할 때는 powerSet(0)로 호출한다. 즉 P는 공집합이고 S는 전체집합이다.

private static char data[] = {'a','b','c','d','e','f'};
private static int n = data.length;
private static boolean [] include = new boolean [n];

public static void powerSet(int k){
    if (k==n) {
        for (int i=0; i<n; i++)
            if (include[i])  System.out.print(data[i] + " ");
        System.out.println();
        return;
    }
    include[k] = false;
    powerSet(k+1);
    include[k] = true;
    powerSet(k+1);
}
```