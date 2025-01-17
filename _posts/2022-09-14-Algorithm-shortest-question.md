---
title:  "[Algorithm] 알고리즘 정리(16)- 최단경로 알고리즘 기출"
excerpt: "코딩 테스트 대비 알고리즘을 정리 하는 글"

categories:
  - algorithm
tags:
  - [Blog, algorithm, python, shortest_algorithm]

toc: true
toc_sticky: true
 
date: 2022-09-14
last_modified_at: 2022-09-14
---

# 문제

## 1.플로이드
n개의 도시가 존재하는데 한 도시에서 다른도시로 가는 버스가 m개가 있다.  
모든 도시 (A, B)에 대하여 가는데 필요한 비용의 최솟값을 구하라.

## 1-1.내가 푼 답

```python
INF = int(1e9)

n = int(input())
m = int(input())

graph = [[INF] * (n+1) for _ in range(n+1)]

for a in range(1, n+1):
    for b in range(1, n+1):
        if a == b:
            graph[a][b] = 0

for _ in range(m):
    a, b, c = map(int, input().split())
    graph[a][b] = min(graph[a][b], c)

for k in range(1, n + 1):
    for a in range(1, n + 1):
        for b in range(1, n + 1):
            graph[a][b] = min(graph[a][b], graph[a][k] + graph[k][b])

for a in range(1, n+1):
    for b in range(1, n+1):
        if graph[a][b] == INF:
            print(0, end=' ')
        else:
            print(graph[a][b], end=' ')
    print()

```

## 1-2.다른 풀이

```python

```

## 1-3.고찰
pypy3로 해야 시간이 줄어들고 input에 같은 경우가 존재하므로 graph[a][b] = min(graph[a][b], c)이렇게 해야한다.

## 2.정확한 순위
n명의 학생의 성적을 비교한 결과를 주어졌을 때 성적순위를 알 수 있는 학생들의 수를 구하라.


## 2-1.내가 푼 답

```python

```
  
## 2-2.다른 풀이

```python
INF = int(1e9)

n, m = map(int, input().split())

graph = [[INF] * (n+1) for _ in range(n+1)]

for i in range(1, n+1):
    graph[i][i] = 0

for _ in range(m):
    a, b = map(int, input().split())
    graph[a][b] = 1

for k in range(1, n+1):
    for a in range(1, n+1):
        for b in range(1, n+1):
            graph[a][b] = min(graph[a][b], graph[a][k]+graph[k][b])

count = 0
for a in range(1, n+1):
    for b in range(1, n+1):
        if graph[a][b] == INF and graph[b][a] == INF:
            break
    else:
        count += 1

print(count)
```

## 2-3.고찰
플로이드로 [a][b] 나 [b][a] 둘중에 하나만 성립되면 성적을 비교할 수 있다는 것을 알아야될거같다.

## 3.화성탐사
화성 탐사 기계가 존재하는 공간은 NxN 크기의 2차원 공간이다. [0][0]에서 [n-1][n-1]로 이동하는 최소비용을 구하라.

## 3-1.내가 푼 답

```python

```
  
## 3-2.다른 풀이

```python
import heapq
import sys
input = sys.stdin.readline
INF = (1e9)

dx = [-1, 1, 0, 0]
dy = [0, 0, -1, 1]

for t in range(int(input())):
    n = int(input())
    graph = []
    for _ in range(n):
        graph.append(list(map(int, input().split())))
    
    distance = [[INF] * n for _ in range(n)]
    x, y = 0, 0
    q = [(graph[x][y], x, y)]
    distance[x][y] = graph[x][y]

    while q:
        dist, x, y = heapq.heappop(q)
        if distance[x][y] < dist:
            continue

        for i in range(4):
            nx = x + dx[i]
            ny = y + dy[i]

            if nx < 0 or nx >= n or ny < 0 or ny >= n:
                continue
            cost  = dist + graph[nx][ny]
            
            if cost < distance[nx][ny]:
                distance[nx][ny] = cost
                heapq.heappush(q, (cost, nx, ny))
    
    print(distance[n-1][n-1])
```

## 3-3.고찰
다익스트라 쓰는 법을 연습해야겠다.

## 4.숨바꼭질
1~n번까지 헛간이 있다. 술래는 항상 1번에 있으며 총 m개의 양방향 통로가 존재한다.  
최단거리가 가장 먼 헛간의 번호를 출력하라

## 4-1.내가 푼 답

```python
```
  
## 4-2.다른 풀이

```python
import heapq
import sys
input = sys.stdin.readline
INF = int(1e9)

n, m = map(int, input().split())

start = 1

graph = [[] * (n+1)]

distance = [INF] * (n+1)

for _ in range(m):
    a, b = map(int, input().split())
    graph[a].append((b, 1))
    graph[b].append((a, 1))

def dijkstra(start):
    q = []
    heapq.heappush(q, (0, start))
    distance[start] = 0
    while q:
        dist, now = heapq.heappop(q)

        if distance[now] < dist:
            continue
        
        for i in graph[now]:
            cost = dist + i[1]
            if cost < distance[i[0]]:
                distance[i[0]] = cost
                heapq.heappush(q, (cost, i[0]))

dijkstra(start)

max_node = 0

max_distance = 0

result = []

for i in range(1, n+1):
    if max_distance < dijkstra[i]:
        max_node = i
        max_distance = distance[i]
        result = [max_node]

    elif max_distance == distance[i]:
        result.append(i)

print(max_node, max_distance, len(result))

```

## 4-3.고찰
