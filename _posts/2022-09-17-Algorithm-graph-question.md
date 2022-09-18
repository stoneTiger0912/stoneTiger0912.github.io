**---
title:  "[Algorithm] 알고리즘 정리(17)- 그래프 알고리즘 기출"
excerpt: "코딩 테스트 대비 알고리즘을 정리 하는 글"

categories:
  - algorithm
tags:
  - [Blog, algorithm, python, graph_algorithm]

toc: true
toc_sticky: true
 
date: 2022-09-17
last_modified_at: 2022-09-17
---

# 문제

## 1.여행계획
각 여행지가 1~N개 존재한다.  
여행지의 개수와 정보를 주어졌을때 여행계획이 가능한지 판별하는 프로그램을 만들어라

## 1-1.내가 푼 답

```python

```

## 1-2.다른 풀이

```python
n, m = map(int, input().split())

def find_parent(parent, x):
    if x != parent[x]:
        parent[x] = find_parent(parent, parent[x])
    return parent[x]

def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)
    if a < b:
        parent[b] = a
    else:
        parent[a] = b

parent = [0] * (n+1)

for i in range(1, n+1):
    parent[i] = i

for i in range(n):
    data = list(map(int, input().split()))
    for j in range(n):
        if data[j] == 1:
            union_parent(parent, i+1, j+1)

plan = list(map(int, input().split()))

result = True

for i in range(m-1):
    if find_parent(parent, plan[i]) != find_parent(parent, plan[i+1]):
        result = False

if result:
    print("YES")
else:
    print("NO")

```

## 1-3.고찰


## 2.탑승구
G개의 탑승구가 존재한다.  
p개의 비행기가 존재하며 i번째 비행기를 1번부터 gi번째 탑승구에 도킹을 해야한다.  
이때 비행기는 도킹되지 않은 탑승구만 도킹가능하다.
p개의 비행기가 도킹할 수 없는 경우 운행을 중지한다. 최대한 많이 도킹하려고하면 최대 몇개까지 도킹이 될지 구하라.

## 2-1.내가 푼 답

```python
g = int(input())
p = int(input())

def find_parent(parent, x):
    if x != parent[x]:
        parent[x] = find_parent(parent, parent[x])

    return parent[x]

def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)
    if a < b:
        parent[b] = a
    else:
        parent[a] = b

parent = [0] * (g+1)

input_lst = []

for _ in range(p):
    input_lst.append(int(input()))

count = 0

for gi in input_lst:
    for i in range(gi, 0, -1):
        if parent[i] == 0:
            parent[i] = gi
            count += 1
            break
    else:
        break

print(count)
```
  
## 2-2.다른 풀이

```python
import sys
input = sys.stdin.readline
INF = int(1e9)
n, m = map(int, input().split())

def find_parent(parent, x):
    if x != parent[x]:
        parent[x] = find_parent(parent, parent[x])

    return parent[x]

def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)
    if a < b:
        parent[b] = a
    else:
        parent[a] = b

g = int(input())

p = int(input())

parent = [0] * (g+1)

for i in range(1, g+1):
    parent[i] = i

result = 0

for _ in range(p):
    data = find_parent(parent, int(input()))
    if data == 0:
        break
    union_parent(parent, data, data-1)
    result += 1

print(result)
```

## 2-3.고찰


## 3.어두운 길
n개의 마을의 집과 m개의 도로로 구성되어있다.  
특정한 도로의 가로등을 켜기위한 비용은 해당 도로의 길이와 같다.  
일부 가로등을 절약하여 최대한 많은 금액을 출력하라

## 3-1.내가 푼 답

```python
import sys
input = sys.stdin.readline
INF = int(1e9)
n, m = map(int, input().split())

def find_parent(parent, x):
    if x != parent[x]:
        parent[x] = find_parent(parent, parent[x])

    return parent[x]

def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)
    if a < b:
        parent[a] = b
    else:
        parent[b] = a

parent = [0] * n
for i in range(n):
    parent[i] = i

edges = []
result = 0

for _ in range(m):
    a, b, cost = map(int, input().split())
    edges.append((cost, a, b))

edges.sort()

total = 0

for edge in edges:
    cost, a, b = edge
    total += cost
    if find_parent(parent, a) != find_parent(parent, b):
        union_parent(parent, a, b)
        result += cost

print(total-result)
```
  
## 3-2.다른 풀이

```python
```

## 3-3.고찰
최소 신장 트리

## 4.행성 터널
왕국은 n개의 행성으로 이루어졌다.  
두 행성 a, b를 터널로 연결하는 비용은 min(xa-xb,ya-yb, za-zb)이다.  
터널은 총 n-1개를 건설하여 모든 행성이 서로 연결되게 하려고 한다 이때 최소 비용을 구하라.

## 4-1.내가 푼 답

```python
```
  
## 4-2.다른 풀이

```python
import sys
import math
import heapq
input = sys.stdin.readline

n = int(input())

def find_parent(parent, x):
    if x != parent[x]:
        parent[x] = find_parent(parent, parent[x])

def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)
    if a < b:
        parent[b] = a
    else:
        parent[a] = b

parent = [0] * (n+1)

edges = []
result = 0

for i in range(n):
    parent[i] = i

x = []
y = []
z = []

for i in range(1, n+1):
    data = list(map(int, input().split()))
    x.append((data[0], i))
    x.append((data[1], i))
    x.append((data[2], i))

x.sort()
y.sort()
z.sort()

for i in range(n-1):
    edges.append((x[i+1][0] - x[i][0], x[i][1], x[i+1][1]))
    edges.append((y[i+1][0] - y[i][0], y[i][1], y[i+1][1]))
    edges.append((z[i+1][0] - z[i][0], z[i][1], z[i+1][1]))

edges.sort()

for edge in edges:
    cost, a, b = edge
    if find_parent(parent, a) != find_parent(parent, b):
        union_parent(parent, a, b)
        result += cost

print(result)
```

## 4-3.고찰
x, y, z를 따로 따로 정렬을 하여 모든 간선을 저장한다음에 최소신장트리가 되게 하는 부분만 하므로 x, y, z 를 합쳐서 할 필요가 없다.

## 5.최종 순위
총 n개의 팀이 있다.  
작년에는 팀 순위를 밝혔는데 지금은 13팀이 6팀을 작년에 이겼는데 지금 6팀이 이겻으면 
(6, 13)으로 발표를 하였다.  
이를 토대로 최종순위를 예측하여라

## 5-1.내가 푼 답(못 풂)

```python
import sys
input = sys.stdin.readline

def find_parent(parent, x):
    if x != parent[x]:
        parent[x] = find_parent(parent, parent[x])
    return parent[x]

def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)
    if a < b:
        parent[b] = a
    else:
        parent[a] = b

t = int(input())
for _ in range(t):
    n = int(input())
    rank_list = list(map(int, input().split()))
    parent = [0] * n
    
    
    m = int(input())
    for _ in range(m):
        


```
  
## 5-2.다른 풀이

```python
import collections


from collections import deque
import sys
input = sys.stdin.readline

def find_parent(parent, x):
    if x != parent[x]:
        parent[x] = find_parent(parent, parent[x])
    return parent[x]

def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)
    if a < b:
        parent[b] = a
    else:
        parent[a] = b

t = int(input())
for _ in range(t):
    n = int(input())
    indegree = [0] * (n+1)
    graph = [[False] * (n+1) for i in range(n+1)]
    data = list(map(int, input().split()))

    for i in range(n):
        for j in range(i+1, n):
            graph[data[i]][data[j]] = True
            indegree[data[j]] += 1

    m = int(input())
    for i in range(m):
        a, b = map(int, input().split())
        if graph[a][b]:
            graph[a][b] = False
            graph[b][a] = True
            indegree[a] += 1
            indegree[b] += 1
        else:
            graph[a][b] = True
            graph[b][a] = False
            indegree[a] -= 1
            indegree[b] -= 1


    result = []
    q = deque()

    for i in range(1, n+1):
        if indegree[i] == 0:
            q.append(i)

    certain = True
    cycle = False

    for i in range(n):
        if len(q) == 0:
            cycle = True
            break

    if len(q) >= 2:
        certain = False
        break

    now = q.popleft()
    result.append(now)

    for j in range(1, n+1):
        if graph[now][j]:
            indegree -= 1

            if indegree[j] == 0:
                q.append(j)

if cycle:
    print("impossible")
elif not certain:
    print("?")
else:
    for i in result:
        print(i, end=' ')
    print()
```

## 5-3.고찰
방향그래프로 변동순위를 맞바꾸고 위상정렬을 수행한다.  
사이클이 발생하는 경우와 위상정렬의 결과가 여러개인 경우가 아니면 순위가 한개가 된다는 것이다. 