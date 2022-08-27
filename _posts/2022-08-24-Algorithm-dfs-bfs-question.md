---
title:  "[Algorithm] 알고리즘 정리(12)- DFS, BFS 알고리즘 기출"
excerpt: "코딩 테스트 대비 알고리즘을 정리 하는 글"

categories:
  - algorithm
tags:
  - [Blog, algorithm, python, DFS_BFS_algorithm]

toc: true
toc_sticky: true
 
date: 2022-08-24
last_modified_at: 2022-08-28
---

# 문제

## 1.특정 거리의 도시 찾기
어떤 나라에 1~N번까지 도로가 존재한다.  
특정 도시 x로 부터 모든 도시로 도착할 수 있는 시간이 정확히 k인 도시를 구하라

## 1-1.내가 푼 답

```python
from collections import deque
from turtle import distance

n, m, k, x = map(int, input().split())

map_list = [[] for _ in range(n+1)]

for _ in range(m):
    a, b = map(int, input().split())
    map_list[a].append(b)

distance = [0] * (n+1)
visited = [False] * (n+1)

q = deque()

def bfs(start):
    q.append([start])
    distance[start] = 0
    visited[start] = 0

    result = []

    while q:
        cur = q.popleft()
        for i in map_list[cur]:
            if visited[i] == False:
                visited[i] = True
                distance[i] = distance[cur] + 1
            
                if distance[i] == k:
                    result.append(i)

    if len(result) == 0:
        print(-1)
    else:
        result.sort()
        for i in result:
            print(i)

```

## 1-2.다른 풀이

```python

```

## 1-3.고찰


## 2.연구소
바이러스를 막기 위해 벽을 세울려고 한다.  
연구소의 크기는 NxM이고 0은 빈칸 1은 벽 2는 바이러스이다.  
벽을 3개를 세운뒤 바이러스가 퍼질 수 없는 곳을 안전 영역이라 할때 안전 영역의 최대값을 구하라.

## 2-1.내가 푼 답(틀림)

```python
from itertools import combinations, count

n, m = map(int, input().split())

graph = [list(map(int, input().split())) for _ in range(m)]

dx = [-1, 1, 0, 0]
dy = [0, 0, -1, 1]

virus_list = []
blank = []

def dfs(map_list, x, y):
    if x <= -1 or x >= n and y <= -1 or y >= m:
        return False
    
    if map_list[x][y] == 0:
        map_list[x][y] == 2
        
        for i in range(4):
            dfs(map_list, x+dx[i], y+dy[i])
        
    return map_list



for i in range(n):
    for j in range(m):
        if graph[i][j] == 2:
            virus_list.append([i, j])
        elif graph[i][j] == 0:
            blank.append([i, j])

blank_combination = list(combinations(blank, 3))

result = 0

for walls in blank_combination:
    map_list = graph
    for wall in walls:
        x, y = wall
        map_list[x][y] == 1
    
    for virus in virus_list:
        x, y = virus
        map_list = dfs(map_list, x, y)

    result = map_list.count(0) if map_list.count(0) >= result else result
    print(result)
print(result)

```
  
## 2-2.다른 풀이

```python
from collections import deque
import copy

def bfs():
    queue = deque()
    tmp_graph = copy.deepcopy(graph)
    for i in range(n):
        for j in range(m):
            if tmp_graph[i][j] == 2:
                queue.append((i, j))

    while queue:
        x, y = queue.popleft()

        for i in range(4):
            nx = x + dx[i]
            ny = y + dy[i]

            if nx < 0 or nx >= n or ny < 0 or ny >= m:
                continue
            if tmp_graph[nx][ny] == 0:
                tmp_graph[nx][ny] = 2
                queue.append((nx, ny))

    global answer
    cnt = 0

    for i in range(n):
        cnt += tmp_graph[i].count(0)

    answer = max(answer, cnt)


def makeWall(cnt):
    if cnt == 3:
        bfs()
        return

    for i in range(n):
        for j in range(m):
            if graph[i][j] == 0:
                graph[i][j] = 1
                makeWall(cnt+1)
                graph[i][j] = 0

n, m = map(int, input().split())
graph = []
dx = [0, 0, 1, -1]
dy = [1, -1, 0, 0]

for i in range(n):
    graph.append(list(map(int, input().split())))

answer = 0
makeWall(0)
print(answer)
```

## 2-3.고찰


## 3.경쟁적 전염
NxN크기의 시험관이 있다.  
바이러스 종류는 1~k까지 있고 1초마다 상 하 좌 우로 움직이는데 낮은 번호의 바이러스 부터 증식한다.  
특정칸에 바이러스가 존재할 경우 다른 바이러스가 들어갈 수 없다.
s초가 지났을 때 (x, y)에 존재하는 바이러스의 종류를 구하라  
만약 없다면 0을 출력


## 3-1.내가 푼 답(틀림)

```python
from collections import deque

n, k = map(int, input().split())

graph = [list(map(int, input().split())) for _ in range(n)]

s, x, y = map(int, input().split())


q = deque()

dx = [-1, 1, 0, 0]
dy = [0, 0, -1, 1]

virus_list = [[] for _ in range(k+1)]

def bfs(x, y, result):
    for i in range(4):
        a = x + dx[i]
        b = y + dy[i]

        if a <= -1 or a >= n or b <= -1 or b >= n:
            return False
        
        if graph[a][b] == 0:
            graph[a][b] = result
            virus_list[result].append([a, b])

for i in range(n):
    for j in range(n):
        tmp = graph[i][j]
        virus_list[tmp].append([i, j])

for _ in range(1, s+1):
    for i in range(1, k+1):
        while virus_list[i]:
            x, y = virus_list[i].pop()
            bfs(x, y, i)

print(graph[x-1][y-1])
```
  
## 3-2.다른 풀이

```python

```

## 3-3.고찰


## 4.괄호 변환
(, )의 개수가 같다면 균형잡힌 문자열이라고 한다.  
(, )의 짝이 모두 맞다면 균형잡힌 문자열이라고 한다.  


## 4-1.내가 푼 답(틀림)

```python
balance = []
u = []
v = []

def check(p):
    stack = []

    for i in p:
        if i == '(':
            stack.append(i)
        elif i == ')':
            if stack.top() == '(':
                stack.pop()
            else:
                break
    else:
        return True
    
    return False


def sep(p):
    answer = ''
    stack = []

    check(p)
    
    if p.count('(') == p.count(')'):
        for i in range(p):
            balance.append(p[i])
            if balance.count('(') == balance.count(')'):
                u = p[:i + 1]
                v = p[i + 1:]
                return u, v
        

def reverse(strings):
    r = {"(":")", ")": "("}
    return [r[s] for s in strings]
    
    

def solution(p):
    answer = ''
    #균형

    if p == '':
        return p
    
    if check(p):
        return p

    u = sep(p)[0]
    v = sep(p)[1]

    if check(u):
        u+solution(v)
    
    else:
        answer+='('
        answer+=solution(v)
        answer+=')'
        u=u[1:-1]
        for i in reverse(u):
            answer+=i
        return answer

    return answer
```
  
## 4-2.다른 풀이

```python

def reverse(strings):
    r = {"(":")", ")": "("}
    return [r[s] for s in strings]
    
else:
        answer+='('
        answer+=solution(v)
        answer+=')'
    u=u[1:-1]
    for i in reverse(u):
        answer+=i
    return answer

```

## 4-3.고찰
저 두 부분은 구글링한 것이다. reverse함수로 key value를 썻다는 것이 참신했고 문제가 이해가 힘들었긴했는데 답을 보고 문제를 보니 그대로 하면 되는 것을 조금 섬세하게 읽어야 겠다.

## 5.연산자 끼워 넣기
n개의 수로 이루어진 수열 A1,A2...An이 주어진다.  
또 수와 수 사이에 끼워 넣을 수 있는 n-1개의 연산자가 주어진다.  
식의 계산은 연산자 우선순위를 무시하고 진행한다.  
나눗셈은 정수 나눗셈의 몫만 취한다.  
음수를 양수로 나눌경우 양수로 바꾼뒤 몫을 취하고 음수로 바꾼다.
식의 최대와 최소를 구하라.

## 5-1.내가 푼 답(못품)

```python
from collections import deque

q = deque()

def cal(num_list, arith_list):
    n1 = num_list.popleft()
    n2 = num_list.popleft()

    for i in range(4):
        if arith_list[i] != 0:

n = int(input())

num_list = list(map(int, input().split()))

arith_list = list(map(int, input().split()))



cal(num_list, arith_list)
```
  
## 5-2.다른 풀이

```python
# 백트래킹 (Python3 통과, PyPy3도 통과)
import sys

input = sys.stdin.readline
N = int(input())
num = list(map(int, input().split()))
op = list(map(int, input().split()))  # +, -, *, //

maximum = -1e9
minimum = 1e9


def dfs(depth, total, plus, minus, multiply, divide):
    global maximum, minimum
    if depth == N:
        maximum = max(total, maximum)
        minimum = min(total, minimum)
        return

    if plus:
        dfs(depth + 1, total + num[depth], plus - 1, minus, multiply, divide)
    if minus:
        dfs(depth + 1, total - num[depth], plus, minus - 1, multiply, divide)
    if multiply:
        dfs(depth + 1, total * num[depth], plus, minus, multiply - 1, divide)
    if divide:
        dfs(depth + 1, int(total / num[depth]), plus, minus, multiply, divide - 1)


dfs(1, num[0], op[0], op[1], op[2], op[3])
print(maximum)
print(minimum)
```

## 5-3.고찰
dfs와 순열을 이용하는 방법이 있다.  
depth로 판별하는 방식을 알게되었다.

## 6.감시 피하기
NxN인 복도가 있다. 특정 위치에는 선생님, 학생, 장애물이 있다.  
각 선생님은 자신의 위치에 상하좌우로 감시를 진행한다.  
복도에 장애물이 있으면 장애물 뒷편을 못본다.  

## 6-1.내가 푼 답

```python

```
  
## 6-2.다른 풀이

```python
n = int(input())
graph = []
teacher = 0
for _ in range(n):
  data = list(input().strip().split(' '))
  teacher += data.count('T')
  graph.append(data)

# 상,하,좌,우 움직이는 배열
dx = [1,-1, 0, 0]
dy = [0, 0, 1, -1]

# 직선 방향 확인 함수
def check_S(x,y):
  for i in range(4):
    nx = x + dx[i]
    ny = y + dy[i]
    # 직선 방향으로 확인
    while 0<= nx < n and 0<= ny < n and graph[nx][ny] !='O':
      if graph[nx][ny] == 'S':
        # 감시가능하다
        return True            
      else:        
        # T 나 X으면 계속 탐색
        nx += dx[i]
        ny += dy[i]
  # 감시 불가능하다
  return False

def solution(count):
  global answer
  if count == 3:
    cnt = 0
    for i in range(n):
      for j in range(n):
        if graph[i][j] == 'T':
          if not check_S(i,j):          
            cnt+=1
    # 모든 선생이 감시가 불가능할 때
    if cnt == teacher:
      answer = True
    return

  for i in range(n):
    for j in range(n):
      if graph[i][j] == 'X':
        graph[i][j] = 'O'
        count +=1
        solution(count)
        graph[i][j] = 'X'
        count -=1

answer = False
solution(0)
if answer:
  print('YES')
else:
  print('NO')

```

## 6-3.고찰

## 7.인구이동
NxN인 땅이 있다.
r행 c열에 있는 나라는 A[r][c]명이 산다.
1. 국경선을 공유하는 두 나라의 차이가 L명이상 R명 이하면 국경선을 하루동안 안연다.
2. 국경선이 모두 열리면 인구이동이 시작된다.
3. 국경선이 열려있으면 하루동안 연합이라고 한다.
4. 연합을 이루는 각 칸에 인구수는 연합의 인구수/연합의 칸의 개수가 된다.
5. 연합을 해체하고 국경선을 닫는다.

## 7-1.내가 푼 답(못품)

```python
from curses.ascii import NL
import math

n, l, r = map(int, input().split())

graph = [list(map(int, input().split())) for _ in range(n)]

visited = [[False] * n for _ in range(n)]

count = 0
union = [[0] * n for _ in range(n)]

for i in range(n):
    for j in range(n):
        union[i][j] == count
        count += 1



dx = [-1, 1, 0, 0]
dy = [0, 0, -1, 1]

def bfs(x, y):
    if visited[x][y] == True:
        return False
    else:
        visited[x][y] == True

    for i in range(4):
        a = x + dx[i]
        b = y + dy[i]

        if a < 0 or a >= n or b < 0 or b >= n:
            continue

        if abs(graph[x][y] - graph[a][b]) >= l and abs(graph[x][y] - graph[a][b]) <= r:
            continue
        else:
            union[a][b] = union[x][y]

        bfs(a, b)


bfs(0, 0)

```
  
## 7-2.다른 풀이

```python
import sys
input = sys.stdin.readline
from collections import deque

graph = []
n,l,r = map(int,input().split())
for _ in range(n):
    graph.append(list(map(int,input().split())))

dx = [0,0,1,-1]
dy = [1,-1,0,0]
def bfs(a,b):
    q = deque()
    temp = []
    q.append((a,b))
    temp.append((a,b))
    while q:
        x,y = q.popleft()
        for i in range(4):
            nx = x + dx[i]
            ny = y + dy[i]
            if 0<=nx<n and 0<=ny<n and visited[nx][ny] == 0:
                # 국경선을 공유하는 두 나라의 인구 차이가 L명 이상, R명 이하라면, 두 나라가 공유하는 국경선을 오늘 하루 동안 연다.
                if l<=abs(graph[nx][ny]-graph[x][y])<=r:
                    visited[nx][ny] = 1
                    q.append((nx,ny))
                    temp.append((nx,ny))
    return temp
            
result = 0
while 1:
    visited = [[0] * (n+1) for _ in range(n+1)]
    flag = 0
    for i in range(n):
        for j in range(n):
            if visited[i][j] == 0:
                visited[i][j] = 1
                country = bfs(i,j)
                # 위의 조건에 의해 열어야하는 국경선이 모두 열렸다면, 인구 이동을 시작한다.
                if len(country) > 1:
                    flag = 1
                    # 연합을 이루고 있는 각 칸의 인구수는 (연합의 인구수) / (연합을 이루고 있는 칸의 개수)가 된다. 편의상 소수점은 버린다.
                    number = sum([graph[x][y] for x, y in country]) // len(country)
                    for x,y in country:
                        graph[x][y] = number
    # 연합을 해체하고, 모든 국경선을 닫는다.
    if flag == 0:
        break
    result += 1
print(result)
```

## 7-3.고찰


## 8.블록 이동하기
로봇은 2x1크기로 NxN인 지도에 로봇을 움직여 (n, n)으로 이동할 수 있도록 한다.  
0을 빈칸, 1을 벽이라고 한다. 
(1, 1)에서 (n, n)으로 이동하는 최소 시간을 구하라.  
로봇은 90도씩 회전이 가능하다. 대신 축이 되는 곳에 벽이 있으면 안된다.  



## 8-1.내가 푼 답

```python

```
  
## 8-2.다른 풀이

```python
# 파이썬
from collections import deque

def can_move(cur1, cur2, new_board):
    Y, X = 0, 1
    cand = []
    # 평행이동
    DELTAS = [(-1, 0), (1, 0), (0, 1), (0, -1)]
    for dy, dx in DELTAS:
        nxt1 = (cur1[Y] + dy, cur1[X] + dx)
        nxt2 = (cur2[Y] + dy, cur2[X] + dx)
        if new_board[nxt1[Y]][nxt1[X]] == 0 and new_board[nxt2[Y]][nxt2[X]] == 0:
            cand.append((nxt1, nxt2))
    # 회전
    if cur1[Y] == cur2[Y]: # 가로방향 일 때
        UP, DOWN = -1, 1
        for d in [UP, DOWN]:
            if new_board[cur1[Y]+d][cur1[X]] == 0 and new_board[cur2[Y]+d][cur2[X]] == 0:
                cand.append((cur1, (cur1[Y]+d, cur1[X])))
                cand.append((cur2, (cur2[Y]+d, cur2[X])))
    else: # 세로 방향 일 때
        LEFT, RIGHT = -1, 1
        for d in [LEFT, RIGHT]:
            if new_board[cur1[Y]][cur1[X]+d] == 0 and new_board[cur2[Y]][cur2[X]+d] == 0:
                cand.append(((cur1[Y], cur1[X]+d), cur1))
                cand.append(((cur2[Y], cur2[X]+d), cur2))
                
    return cand

def solution(board):
    # board 외벽 둘러싸기
    N = len(board)
    new_board = [[1] * (N+2) for _ in range(N+2)]
    for i in range(N):
        for j in range(N):
            new_board[i+1][j+1] = board[i][j]

    # 현재 좌표 위치 큐 삽입, 확인용 set
    que = deque([((1, 1), (1, 2), 0)])
    confirm = set([((1, 1), (1, 2))])

    while que:
        cur1, cur2, count = que.popleft()
        if cur1 == (N, N) or cur2 == (N, N):
            return count
        for nxt in can_move(cur1, cur2, new_board):
            if nxt not in confirm:
                que.append((*nxt, count+1))
                confirm.add(nxt)

```

## 8-3.고찰
