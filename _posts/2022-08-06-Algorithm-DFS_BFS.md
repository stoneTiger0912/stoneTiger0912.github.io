---
title:  "[Algorithm] 알고리즘 정리(3)- DFS,BFS 알고리즘"
excerpt: "코딩 테스트 대비 알고리즘을 정리 하는 글"

categories:
  - Algorithm
tags:
  - [Blog, algorithm, python, DFS_BFS_algorithm]

toc: true
toc_sticky: true
 
date: 2022-08-06
last_modified_at: 2022-08-06
---

# DFS,BFS 알고리즘
- 탐색: 많은 양의 데이터 중에서 원하는 데이터를 찾는 과정
- 자료구조: 데이터를 관리하고 처리하기 위한 구조
- 스택: 선입후출, 파이썬에서 list로 표현
- 큐: 선입선출  
    파이썬에서 from collections import deque
- 재귀 함수: 자기자신을 다시 호출
- DFS: 깊이 우선 탐색으로 그래프에서 깊은 부분을 우선적으로 탐색
  프로그래밍에서 그래프 표현 방법
  1. 인접행렬: 2차원 배열로 그래프의 연결 관계를 표현
  2. 인접리스트: 리스트로 그래프의 연결 관계를 표현(그래프간의 비용을 의미)
0이 1과 7만큼 연결되어있고 2와는 5만큼 연결되어있다고 하면
- 인접리스트의 경우
```python
graph[[] for _ in range(2)]

graph[0].append([1, 7])
graph[0].append([2, 5])

graph[1].append([0, 7])
graph[2].append([0, 5])  
```

- BFS: 너비 우선 탐색으로 가까운 노드부터 탐색

## DFS, BFS 알고리즘의 예시
DFS는 스택에 재귀함수를 사용하고  
BFS는 큐를 사용한다.
 
# 문제

## 1.음료수 얼려먹기
NxM의 얼음틀이 있는데 구멍이 뚫린부분이 0 칸막이부분은 1로 할경우  
상하좌우가 붙어있으면 연결되어있다고 할때 생성되는 총 아이스크림의 개수를 구하라.

## 1-1.내가 푼 답(못풀었음)
```python
n, m = map(int, input().split())

ice = [list(map(int, input().split())) _ in range(n)]

```

## 1-2.답안 예시
```python
n, m = map(int, input().split())

graph = []
for i in range(n):
    graph.append(list(map(int, input())))

def dfs(x, y):
    # 범위 벗어날 경우 종료
    if x <= -1 or x >= n or y <= -1 or y >= m:
        return False
    #방문 하지 않았을 경우
    if graph[x][y] == 0:
        graph[x][y] = 1

        dfs(x - 1, y)
        dfs(x, y - 1)
        dfs(x + 1, y)
        dfs(x, y + 1)
        return True
    return False

result = 0
for i in range(n):
    for j in range(m):
        #현재 위치에서 수행
        if dfs(i, j) == True:
            result += 1

print(result)
```

## 1-3.답안 예시와 내가 푼 답의 차이점
일단 내가 그래프에 대해서 아직 덜 공부했다는게 느껴지고  
보충해서 나중에 답을 안보고 다시 풀어야겠다.

## 2.미로탈출
NxM크기의 미로가 있다. 괴물이 있는곳은 0 없는 곳을 1로 할경우 탈출 하기 위한 최소칸의 미로 칸의 개수를 구하라.
## 2-1.내가 푼 답
```python
from collections import deque

n, m = map(int, input().split())

maze = [list(map(int, input())) for _ in range(n)]

d = [(1, 0), (-1, 0), (0, 1), (0, -1)]

q = deque()
q.append((0, 0))

def bfs():
    while q:
        x, y = q.popleft()
        for k in range(4):
            dx = x + d[k][0]
            dy = y + d[k][1]
            if 0 <= dx < n and 0 <= dy < m:
                if maze[dx][dy] == 1:
                    maze[dx][dy] = maze[x][y] + 1
                    q.append((dx, dy))

maze[0][0] = 1
bfs()
print(maze[n-1])
```
## 2-2.답안 예시
```python
from collections import deque


n, m = map(int, input().split())

graph = []
for i in range(n):
    graph.append(list(map(int, input())))

dx = [-1, 1, 0, 0]
dy = [0, 0, -1, 1]

def bfs(x, y):
    queue = deque()
    queue.append((x, y))

    while queue:
        x, y = queue.popleft()

        for i in range(4):
            nx = x + dx[i]
            ny = y + dy[i]

            if nx < 0 or ny < 0 or nx >= n or ny >= m:
                continue
            if graph[nx][ny] == 0:
                graph[nx][ny] = graph[x][y] + 1
                queue.append((nx, ny))
    return graph[n - 1][m - 1]

print(bfs(0, 0))
```

## 2-3.답안 예시와 내가 푼 답의 차이점
아직까지는 그래프가 익숙하지 않아 조금 보고 풀었는데 이것 역시 다시 한번 나중에 풀어야겠다.
