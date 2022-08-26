---
title:  "[Algorithm] 알고리즘 정리(10)- DFS, BFS 알고리즘 기출"
excerpt: "코딩 테스트 대비 알고리즘을 정리 하는 글"

categories:
  - algorithm
tags:
  - [Blog, algorithm, python, DFS_BFS_algorithm]

toc: true
toc_sticky: true
 
date: 2022-08-24
last_modified_at: 2022-08-24
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


## 4.자물쇠와 열쇠
자물쇠가 있는데 한칸이 1x1인 NxN크기의 정사각형이고 열쇠는 MxM크기의 정사각형이다
자물쇠에는 홈이 파여있고 열쇠또한 홈과 돌기가 있다.  
열쇠의 돌기부분에 자물쇠가 딱 맞게 채워지면 열리는 방식이다. 
자물쇠의 모든 홈을 채워 비어있는 곳이 없어야 자물쇠를 열 수 있다.  
열쇠를 나타내는 2차원 배열 key와 2차원 배열 lock가 있을 때
열쇠로 자물쇠를 열 수 있으면 true 없으면 false를 return 하도록 한다.

## 4-1.내가 푼 답

```python

```
  
## 4-2.다른 풀이

```python
#회전
def rotate(k):
    n=len(k)
    result=[[0]*n for _ in range(n)]
    for i in range(n):
        for j in range(n):
            result[j][n-i-1]=k[i][j]
    return result
    
#확인
def check(l):
    n=len(l)//3
    #new_lock의 중간 부분이 모두 1인지 확인
    for i in range(n,n*2):
        for j in range(n,n*2):
            if l[i][j]!=1:
                return False
    return True
                

def solution(key, lock):
    answer = False
    n=len(lock)
    m=len(key)
    
    new_lock = [[0]*(n*3) for _ in range(n*3)]
    #세배 확장된 lock을 만든다.
    for i in range(n):
        for j in range(n):
            new_lock[i+n][j+n]=lock[i][j]
            
    #매 회전
    for r in range(4):
        key=rotate(key)
        #매 이동
        for x in range(1,n*2):
            for y in range(1,n*2):
            
            	#열쇠를 넣어준다.
                for i in range(m):
                    for j in range(m):
                    	#lock에 key를 더해줌
                        new_lock[x+i][y+j]+=key[i][j]
                
                #해당 열쇠가 맞는지 검사
                if check(new_lock)==True:
                    return True
                    
                #맞지 않는다면 되돌린다(열쇠를 뺀다)
                for i in range(m):
                    for j in range(m):
                        new_lock[x+i][y+j]-=key[i][j]
    
    return False
```

## 4-3.고찰
자물쇠를 3배로 확장한다음에 열쇠를 회전하고 움직이면서 거기에 맞는 부분이 있는지 확인하는 완전 탐색으로 푸는 것 같다.

## 5.뱀
뱀이 사과를 먹으면 뱀 길이가 늘어나는데 벽이나 자기자신의 몸과 부딫히면 게임이 끝난다.  
게임은 NxN위에서 하고 뱀이 길이는 처음에 1이다. 뱀은 맨위좌측부터 시작한다.  
뱀은 다음과 같이 이동한다.
1. 뱀은 몸길이늘 늘려 머리를 다음칸에 이동한다
2. 만약 사과가 있다면 꼬리는 그대로다.
3. 사과가 없다면 몸길이를 줄인다.


## 5-1.내가 푼 답(못 풀었음)

```python
n = int(input())

map_list = [[0] * n for _ in range(n)]

apple = int(input())

for _ in range(apple):
    x, y = map(int, input().split())
    map_list[x][y] = 1

moving_snake = int(input())
move_list = []

for _ in range(moving_snake):
    t, dir = input().split()
    move_list.append((t, dir))

snake = [[0, 1]]

dir = ([0, 1], [1, 0], [0, -1], [-1, 0])

cur_dir = dir[0]

for moving in move_list:
    t = int(moving[0])
    for i in t:
        head = snake[-1]
        head[0] = head[0] + cur_dir[0]
        head[1] = head[1] + cur_dir[1]
        if map_list[head[0]][head[1]] == 0:
            snake.pop()
        snake.append(head)
    dir = moving[1]

```
  
## 5-2.다른 풀이

```python
from collections import deque

n = int(input())
k = int(input())

graph = [[0] * n for _ in range(n)]
dx = [0, 1, 0, -1]
dy = [1, 0, -1, 0]

for i in range(k):
    a, b = map(int, input().split())
    graph[a - 1][b - 1] = 2

l = int(input())
dirDict = dict()
queue = deque()
queue.append((0, 0))

for i in range(l):
    x, c = input().split()
    dirDict[int(x)] = c

x, y = 0, 0
graph[x][y] = 1
cnt = 0
direction = 0

def turn(alpha):
    global direction
    if alpha == 'L':
        direction = (direction - 1) % 4
    else:
        direction = (direction + 1) % 4


while True:
    cnt += 1
    x += dx[direction]
    y += dy[direction]

    if x < 0 or x >= n or y < 0 or y >= n:
        break

    if graph[x][y] == 2:
        graph[x][y] = 1
        queue.append((x, y))
        if cnt in dirDict:
            turn(dirDict[cnt])

    elif graph[x][y] == 0:
        graph[x][y] = 1
        queue.append((x, y))
        tx, ty = queue.popleft()
        graph[tx][ty] = 0
        if cnt in dirDict:
            turn(dirDict[cnt])

    else:
        break

print(cnt)
```

## 5-3.고찰
뱀을 큐에 넣는 것이 핵심이다. 그리고 내가 푼 답은 뱀을 따로 리스트에 넣었는데 맵에 1로 표현 하는 것이 더 나은 것 같다.

## 6.기둥과 보 설치
2차원 가상 벽면에 기둥과 보를 설치한다.
1. 기둥은 바닥위에 있거나 보의 한쪽 끝부분 위에 있거나 다른 기둥 위에 있어야 한다.
2. 보는 한쪽 끝부분이 기둥 위에 있거나 또는 양쪽 끝부분이 다른 보와 동시에 연결되어있어야한다.

## 6-1.내가 푼 답

```python

```
  
## 6-2.다른 풀이

```python
# 파이썬
def impossible(result):
    COL, ROW = 0, 1
    for x, y, a in result:
        if a == COL: # 기둥일 때
            if y != 0 and (x, y-1, COL) not in result and \
        (x-1, y, ROW) not in result and (x, y, ROW) not in result:
                return True
        else: # 보일 때
            if (x, y-1, COL) not in result and (x+1, y-1, COL) not in result and \
        not ((x-1, y, ROW) in result and (x+1, y, ROW) in result):
                return True
    return False

def solution(n, build_frame):
    result = set()
    
    for x, y, a, build in build_frame:
        item = (x, y, a)
        if build: # 추가일 때
            result.add(item)
            if impossible(result):
                result.remove(item)
        elif item in result: # 삭제할 때
            result.remove(item)
            if impossible(result):
                result.add(item)
    answer = map(list, result)
    
    return sorted(answer, key = lambda x : (x[0], x[1], x[2]))
```

## 4-3.고찰
함수 하나를 쓰는게 효율적인 면에서 좋다

## 7.치킨 배달
크기가 NxN인 도시가 있다. 도시의 칸은 (r, c)같은 형태이다.
0은 빈칸 1은 집 2는 치킨집이라고 할때 치킨집은 최대 m개까지 고를 때 도시의 치킨거리가 가장 적을지 구하라

## 7-1.내가 푼 답

```python
from itertools import combinations

n, m = map(int, input().split())
maps=[]
for i in range(n):
  maps.append(list(map(int, input().split())))

home=[]
chicken=[]
for i in range(n):
  for j in range(n):
    if maps[i][j]==1:
      home.append((i,j))
    elif maps[i][j]==2:
      chicken.append((i,j))

chicken_combinations = list(combinations(chicken,m))
result=[0]*len(chicken_combinations)

for i in home:
  for j in range(len(chicken_combinations)):
    a=100
    for k in chicken_combinations[j]:
      tmp = abs(i[0]-k[0])+abs(i[1]-k[1])
      a=min(a, tmp)
    result[j]+=a

print(min(result))
```
  
## 7-2.다른 풀이

```python

```

## 7-3.고찰


## 8.외벽 점검
내부 공사를 하는데 레스토랑의 구조는 완벽한 원 모양이고 총 n미터 이다.  
내부 공사 도중에 점검을 확인하기 위해서 주기적으로 점검을 해야한다. 단 1시간으로 제한한다.  
편의상 레스토랑의 정북을 0으로 하고 시계방향으로 거리를 나타낸다.  
외벽의 길이를 n, 취약 지점의 위치가 담긴 배열 weak, 각 친구가 1시간 동안 이동할 수 있는 거리가 dist일때 친구 수의 최솟값을 구하라


## 8-1.내가 푼 답

```python

```
  
## 8-2.다른 풀이

```python
from itertools import permutations

def solution(n, weak, dist):
    # 길이를 2배로 늘려서 원형 -> 일자 형태
    length = len(weak)
    for i in range(length):
        weak.append(weak[i] + n)

    answer = len(dist) + 1
    # 0 부터 length-1 까지의 위치를 각각 시작점으로 설정
    for start in range(length):
        # 친구를 나열하는 모든 경우의 수 각각에 대하여 확인
        for friends in list(permutations(dist, len(dist))):
            count = 1 # 투입할 친구의 수
            # 해당 친구가 점검할 수 있는 마지막 위치
            position = weak[start] + friends[count-1]
            # 시작점부터 모든 취약 지점을 확인
            for index in range(start, start+length):
                # 점검할 수 있는 위치를 벗어나는 경우
                if position < weak[index]:
                    count += 1 # 새로운 친구를 투입
                    if count > len(dist): # 더 투입이 불가능하다면 종료
                        break
                    position = weak[index] + friends[count-1]
            answer = min(answer, count) # 최솟값 계산
        if answer > len(dist):
            return -1
    return answer
```

## 8-3.고찰
컴비네이션이 자주 쓰이는 것을 알게 되었고 구현 부분의 문제들을 거의 혼자서 풀지 못했다.  
좀더 공부좀 해야겠다.