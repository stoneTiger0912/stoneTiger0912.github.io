---
title:  "[Algorithm] 알고리즘 정리(7)- 최단 경로 알고리즘"
excerpt: "코딩 테스트 대비 알고리즘을 정리 하는 글"

categories:
  - algorithm
tags:
  - [Blog, algorithm, python, shortest_path_algorithm]

toc: true
toc_sticky: true
 
date: 2022-08-11
last_modified_at: 2022-08-11
---

# 최단 경로 알고리즘
- 가장 짧은 경로를 찾는 알고리즘이다.
- 다음과 같은 예시가 있다.
1. 한 지점에서 다른 지점까지의 최단경로
2. 모든 지점까지의 최단 경로를 모두 구하는 경우
- 보통 그래프를 이용하는데 각 지점을 노드 도로를 간선으로 표현한다.
- 학부에서 사용하는 알고리즘은 다익스트라, 플로이드 워셜, 벨만 포드 3가지가 대표적이다.
- 다익스트라와 플로이드 2가지가 가장 많이 나온다.
- 그리디 알고리즘과 다이나믹 프로그래밍이 최단 경로 알고리즘에 사용된다. 

# 다익스트라 알고리즘
- 여러개의 노드중에서 특정 노드에서 출발하여 다른 노드로 가는 각각의 최단 경로를 구해주는 알고리즘이다.
- 음의 간선이 없을 경우에 사용된다.
- 알고리즘의 원리는 다음과 같다
  1. 출발 노드 설정
  2. 최단 거리 테이블 초기화
  3. 방문하지 않는 노드 중 최단 거리가 짧은 노드 선택
  4. 해당 노드를 거쳐 다른 노드로 가는 비용을 계산하여 최단 거리 테이블 갱신
  5. 3, 4번 반복
- 각 노드에 대한 최단 거리 정보를 항상 1차원 리스트에 저장하며 계속 갱신한다.
- 다익스트라 알고리즘 구현 방법
  1. 구현하기 쉽지만 느리게 동작


```python
    import sys
    input = sys.stdin.readline
    INF = int(1e9)

    #노드, 간선의 개수
    n, m = map(int, input().split())

    #시작 노드 입력
    start = int(input())

    graph = [[] for i in range(n + 1)]

    visited = [False] * (n + 1)

    distance = [INF] * (n + 1)

    for _ in range(m):
        a, b, c = map(int, input().split())
        # a에서 b로 가능 비용이 c
        graph[a].append((b, c))

    #방문하지 않은 노드중에서 가장 거리가 짧은 노드의 번호 반환
    def get_smallest_node():
        min_value = INF
        index = 0
        for i in range(1, n + 1):
            if distance[i] < min_value and not visited[i]:
                min_value = distance[i]
                index = i
        return index

    def dijkstra(start):
        #시작 노드에 초기화
        distance[start] = 0
        visited[start] = True
        for j in graph[start]:
            #j[0]은 위에서 구한 b j[1]은 비용 c를 나타냄
            distance[j[0]] = j[1]
        #시작노드를 제외한 전체 n - 1노드에 대해 반복
        for i in range(n - 1):
            #현재 최단거리가 가장짧은 노드를 꺼내 방문처리
            now = get_smallest_node()
            visited[now] = True

            #현재 노드와 연결된 다른 노드 확인
            for j in graph[now]:
                cost = distance[now] + j[1]

                #현재 노드를 거쳐 다른 노드로 이동하는 거리가 더 짧은 경우
                if cost < distance[j[0]]:
                    distance[j[0]] = cost

    dijkstra(start)

    for i in range(1, n + 1):
        if distance[i] == INF:
            print("INFINITY")
        else:
            print(distance[i])


```

  1. 구현하기 어렵지만 빠르게 동작 

```python
    import heapq
    import sys
    input = sys.stdin.readline
    INF = int(1e9)

    n, m = map(int, input().split())

    start = int(input())

    graph = [[] for i in range(n + 1)]

    distance = [INF] * (n + 1)

    for _ in range(m):
        a, b, c = map(int, input().split())

        graph[a].append((b, c))
    
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

    for i in range(1, n + 1):
        if distance[i] == INF:
            print("INFINITY")
        
        else:
            print(distance[i])
```

- 2번이 1번보다 O(ElogN)으로 더 빠르다.

# 폴로이드 워셜 알고리즘
- 모든 지점에서 다른 모든 지점 까지의 최단 경로를 구하는 경우
- 다이나믹 프로그래밍
- Dab = min(Dab, Dak+Dkb)

```python
INF = int(1e9)

n = int(input())
m = int(input())

graph = [[INF] * (n+1) for _ in range(n + 1)]

for a in range(1, n + 1):
    for b in range(1, n + 1):
        if a == b:
            graph[a][b] = 0

for _ in range(m):
    a, b, c = map(int, input()split())
    graph[a][b] = c

for k in range(1, n + 1):
    for a in range(1, n + 1):
        for b in range(1, n + 1):
            graph[a][b] = min(graph[a][b], graph[a][k] + graph[k][b])

for a in range(1, n + 1):
    for b in range(1, n + 1):
        if graph[a][b] == INF:
            print("INFINITY", end=" ")
        else:
            print(graph[a][b], end=" ")
    print()
```

# 문제

## 1.미래도시
미래도시에는 1부터 N번까지 회사가 있는데 특정회사는 연결되어있다.  
현재 1번에 위치해 있는데 x번 회사에 물건을 판매해야한다.  
중간에 k번째 회사도 방문해야하는데 1번에서 k번 x번으로 순서대로 이동하는 최소시간을 계산하라.  

## 1-1.내가 푼 답(못풀었음)

```python
```

## 1-2.답안 예시
```python
import heapq
import sys

INF = int(1e9)
input = sys.stdin.readline

n, m = map(int, input().split())

graph = [[INF] * (n + 1) for _ in range(n + 1)]

for a in range(1, n+1):
    for b in range(1, n+1):
        if a == b:
            graph[a][b] = 0


for _ in range(m):
    a, b = map(int, input().split())
    graph[a][b] = 1
    graph[b][a] = 1

x, k = map(int, input().split())

for k in range(1, n + 1):
    for a in range(1, n+1):
        for b in range(1, n+1):
            graph[a][b] = min(graph[a][b], graph[a][k] + graph[k][b])


distance = graph[1][k] + graph[k][x]

if distance >= INF:
    print("-1")
else:
    print(distance)
```

## 1-3.답안 예시와 내가 푼 답의 차이점
최적경로 알고리즘을 아직 완전히 이해하지 못해서 정확하게 구현하는 연습부터 해야할 것같다.


## 2.전보
N개의 도시가 있는데 각 도시는 다른 도시로 메세지를 보낼 수 있다.  
하지만 보낼려고 하는 도시에 통로가 있어야만 한다.  
c도시에서 다른 모든 도시로 최대한 많은 도시로 보내려고할때 받는 도시의 개수와 모든 도시로 보낼때 걸린 시간을 구하라.

## 2-1.내가 푼 답(못풀었음)

```python
import heapq
import sys

INF = int(1e9)
input = sys.stdin.readline

n, m, start = map(int, input().split())

graph = [[INF] * (n + 1) for _ in range(n + 1)]

for a in range(1, n+1):
    for b in range(1, n+1):
        if a == b:
            graph[a][b] = 0

for _ in range(m):
    a, b, c = map(int, input().split())
    graph[a].append((b, c))

for k in range(1, n+1):
    for a in range(1, n+1):
        for b in range(1, n+1):
            graph[a][b] = min(graph[a][b], graph[a][k] + graph[k][b])

```
  
## 2-2.답안 예시

```python
import heapq
import sys
from turtle import distance

INF = int(1e9)
input = sys.stdin.readline

n, m, start = map(int, input().split())

graph = [[INF] * (n + 1) for _ in range(n + 1)]

distance = [INF] * (n + 1)

for a in range(1, n+1):
    for b in range(1, n+1):
        if a == b:
            graph[a][b] = 0

for _ in range(m):
    a, b, c = map(int, input().split())
    graph[a].append((b, c))

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

count = 0

max_distance = 0
for d in distance:
    if d != INF:
        count += 1
        max_distance = max(max_distance, d)

print(count - 1, max_distance)
```

## 2-3.답안 예시와 내가 푼 답의 차이점
알고리즘들을 안보고도 쓸 수 있게 연습을 해야겠다.  