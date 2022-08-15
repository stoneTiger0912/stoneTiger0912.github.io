---
title:  "[Algorithm] 알고리즘 정리(8)- 그래프 이론"
excerpt: "코딩 테스트 대비 알고리즘을 정리 하는 글"

categories:
  - algorithm
tags:
  - [Blog, algorithm, python, shortest_graph]

toc: true
toc_sticky: true
 
date: 2022-08-12
last_modified_at: 2022-08-12
---

# 그래프 이론
- 클루스칼 알고리즘: 그리디 알고리즘 기반
- 위상 정렬 알고리즘: 큐 혹은 스택 자료구조를 이용
- 인접 행렬과 인접 리스트를 사용하는데 인접행렬은 O(n^2)이고 인접 리스트는 O(E)이다.

# 서로소 집합 자료구조
- 공통 원소가 없는 두 집합을 서로소 집합이라 한다.
- 서로소 부분 집합들로 나누어진 원소들의 데이터를 처리하기 위한 자료구조이다.
- union연산은 2개의 집합을 하나로 연결, find연산은 특정한 원소가 속한 집합이 어떤 것인지 알려주는 연산이다.
- 스택과 큐가 push와 pop을 사용한 것 처럼 union-find를 사용한다.
- 트리 자료구조를 이용하여 집합을 표현한다.
- 시간복잡도는 O(v + M(1+log(2-m/v)v))이다.

## 예시
1, 2, 3, 4, 5, 6가 있다고 치자
다음과 같이 union연산이 4개 있다고 한다.
1. union 1, 4
2. union 2, 4
3. union 2, 3
4. union 5, 6
이것이 뜻하는 의미는 1, 2, 3, 4가 연결되고 5, 6이 연결되어 있다는 의미이다.
알고리즘으로 표현하면 다음과 같다.

| | | | | | | |  
|---|---|---|---|---|---|---|
|노드번호|1|2|3|4|5|6|  
|부모|1|2|3|4|5|6|

union1, 4
노드1과 노드4의 루트 노드를 각각 찾고 각 루트 노드중에서 **큰 번호**에 해당하는 루트 노드4의 부모를 1로 설정한다.  

| | | | | | | |  
|---|---|---|---|---|---|---|
|노드번호|1|2|3|4|5|6|  
|부모|1|2|3|1|5|6|

union2, 3
노드2, 3중에서 각 루트 노드중에서 **큰 번호**인 3에 루트 노드를 2로 바꾼다.  

| | | | | | | |  
|---|---|---|---|---|---|---|
|노드번호|1|2|3|4|5|6|  
|부모|1|2|2|1|5|6|

union2, 4
노드2, 4중에서 각 루트 노드중에서 **큰 번호**인 2에 루트 노드를 1로 바꾼다.  

| | | | | | | |  
|---|---|---|---|---|---|---|
|노드번호|1|2|3|4|5|6|  
|부모|1|1|2|1|5|6|

union5, 6
노드5, 6중에서 각 루트 노드중에서 **큰 번호**인 6에 루트 노드를 5로 바꾼다.  

| | | | | | | |  
|---|---|---|---|---|---|---|
|노드번호|1|2|3|4|5|6|  
|부모|1|1|2|1|5|5|

그리고 나중에 3의 루트 노드가 2인데 2의 루트 노드는 1이므로 최종적으로 다음과 같다. 

| | | | | | | |  
|---|---|---|---|---|---|---|
|노드번호|1|2|3|4|5|6|  
|부모|1|1|1|1|5|5|

소스코드로 하면 다음과 같다.  

## 소스코드

```python
#재귀를 사용하므로 5->4->3->2->1일 경우 매우 비효율적
'''
def find_parent(parent, x):
    #루트 노드가 아니면 루트 노드를 찾을때까지 재귀적으로 호출
    if parent[x] != x:
        return find_parent(parent, parent[x])
    return x
'''
#바로 루트노드를 입력받으므로 위의 것보다 효율적이다.
def find_parent(parent, x):
    if parent[x] != x:
        parent[x] = find_parent(parent, parent[x])
    return parent[x]

#두 원소가 속한 집합 합치기
def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)
    if a < b:
        parent[b] = a
    else:
        parent[a] = b

v, e = map(int, input().split())
parent = [0] * (v+1) #부모 테이블 초기화

for i in range(1, v+1):
    parent[i] = i

#union 연산 각각 수행
for i in range(e):
    a, b = map(int, input().split())
    union_parent(parent, a, b)

print("각 원소가 속한 집합:", end='')
for i in range(1, v+1):
    print(find_parent(parent, i), end=' ')

print()

#부모 테이블 출력
print('부모테이블: ', end='')
for i in range(1, v+1):
    print(parent[i], end=' ')
```

# 서로소 집합의 사이클판별
- 두 노드의 루트노드를 union연산 한다.
- 만약 루트노드가 같다면 cycle이다.

## 소스코드

```python
#재귀를 사용하므로 5->4->3->2->1일 경우 매우 비효율적
'''
def find_parent(parent, x):
    #루트 노드가 아니면 루트 노드를 찾을때까지 재귀적으로 호출
    if parent[x] != x:
        return find_parent(parent, parent[x])
    return x
'''
#바로 루트노드를 입력받으므로 위의 것보다 효율적이다.
def find_parent(parent, x):
    if parent[x] != x:
        parent[x] = find_parent(parent, parent[x])
    return parent[x]

#두 원소가 속한 집합 합치기
def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)
    if a < b:
        parent[b] = a
    else:
        parent[a] = b

v, e = map(int, input().split())
parent = [0] * (v+1) #부모 테이블 초기화

for i in range(1, v+1):
    parent[i] = i

cycle = False

#union 연산 각각 수행
for i in range(e):
    a, b = map(int, input().split())
    if find_parent(parent, a) == find_parent(parent, b):
        cycle = True
        break
    else:
        union_parent(parent, a, b)

if cycle:
    print("사이클 발생")
else:
    print("사이클 발생 x")
```


# 신장 트리
- 하나의 그래프가 모든 노드를 포함하면서 사이클이 존재하지 않는 그래프를 신장 트리라한다.

## 크루스칼 알고리즘
- 모든 도시를 도로로 연결할때 최소한의 비용을 들기 위해서는 신장 트리를 사용한다.
- 이때 사용하는 알고리즘을 **최소 신장 트리 알고리즘**이라하고 대표적인 알고리즘이 클루스칼 알고리즘이다.
- 클루스칼 알고리즘은 그리디 알고리즘이기도 하다. 
- 알고리즘의 원리는 다음과 같다.
  1. 간선 데이터를 비용에 따라 오름차순으로 정렬
  2. 간선을 하나씩 확인한다음 사이클이 발생하는지 확인한다.
- 신장 트리에 포함되는 간선의 개수는 n-1이다.
- O(ElogE)

## 예시

| | | | | | | | | | | 
|---|---|---|---|---|---|---|---|---|---|
|간선|(1, 2)|(1, 5)|(2, 3)|(2, 6)|(3, 4)|(4, 6)|(4, 7)|(5, 6)|(6, 7)|  
|비용|29|75|35|34|7|23|13|53|25|

1. 비용이 적은 순으로 union함수를 한다
7이 가장 적으므로 3, 4를 하고 다음으로 적은 13인 4, 7을 한다.  
3,4,7 은 원래 각각을 부모노드로 하고 있었으므로 연결이 된다. 

| | | | | | | | | | | 
|---|---|---|---|---|---|---|---|---|---|
|간선|(1, 2)|(1, 5)|(2, 3)|(2, 6)|(3, 4)|(4, 6)|(4, 7)|(5, 6)|(6, 7)|  
|비용|29|75|35|34|7|23|13|53|25|
|순서|  |  |  |  |1|  | 2|  |  |

2. 그 다음으로 적은 4, 6을 하면 3, 4, 6, 7이 같이 연결된다.  
3. 그 다음으로 6, 7인데 여기는 이미 연결되어있으므로 넘어간다.
4. 1, 2는 이상없으므로 연결되고 2, 6도 이상없으니 연결이 된다.
5. 그 뒤로 겹치는 부분을 제외한 5, 6을 마지막으로 하면 최소 신장 트리가 되는 것을 알 수 있다.

## 코드

```python
def find_parent(parent, x):
    #루트 노드 찾기
    if parent[x] != x:
        parent[x] = find_parent(parent, parent[x])
    return parent(x)

def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)
    if a < b:
        parent[b] = a
    else:
        parent[a] = b
    
v, e = map(int, input().split())

parent = [0] * (v + 1)

edges = []
result = 0

for i in range(1, v + 1):
    parent[i] = i

for _ in range(e):
    a, b, cost = map(int, input().split())
    #비용순으로 하기위해 튜플로 저장
    edges.append((cost, a, b))

edges.sort()

for edge in edges:
    cost, a, b = edge

    if find_parent(parent, a) != find_parent(parent, b):
        union_parent(parent, a, b)
        result += cost

print(result)
```

# 위상 정렬
- 방향 그래프의 모든 노드를 방향성에 거스르지 않도록 순서대로 나열하는 것
- 예를 들어 자료구조를 들은 뒤에 알고리즘을 들어야 하는 학습 순서를 위상정렬의 한 예시로 들 수 있다.
- 진입 차수: 특정한 노드로 들어오는 간선의 개수
- 자료구조를 들은 뒤에 알고리즘을 듣고 고급 알고리즘을 차례대로 듣는 예시에서 고급 알고리즘은 2개의 선수 과목이 있다. 이때 고급 알고리즘의 진입 차수가 2이다.
1. 진입차수가 0인 노드를 큐에 넣는다
2. 큐에서 원소를 꺼내 해당 노드에서 출발하는 간선을 노드에서 제거
3. 새롭게 진입차수가 0이 된 노드를 큐에 넣는다.
- 즉 위의 예시에서 자료구조 -> 알고리즘 -> 고급 알고리즘 순으로 제거
- O(v + e)  

## 코드  

```python  

from collections import deque

v, e = map(int, input().split())

indegree = [0] * (v + 1)

#각 노드에 연결된 간선 정보
graph = [[] for i in range(v + 1)]

for _ in range(e):
    a, b = map(int, input().split())
    graph[a].append(b)
    indegree[b] += 1

#위상 정렬 함수
def topology_sort():
    result = []
    q = deque()

    #진입 차수가 0인 노드를 큐에 삽입
    for i in range(1, v + 1):
        if indegree[i] == 0:
            q.append(i)

    while q:

        now = q.popleft()
        result.append(now)

        for i in graph[now]:
            indegree[i] -= 1

            if indegree[i] == 0:
                q.append(i)

    for i in result:
        print(i, end=' ')

topology_sort()
```

# 문제

## 1.팀 합치기
학생이 0부터 N번까지 있을 때 팀 합치기와 같은 팀 여부 확인 연산을 이용할때 같은 팀 여부 확인 결과를 출력  
같은 팀 여부 확인 출력시 0 a b형태로  
팀 합치기는 1 a b로 표현

## 1-1.내가 푼 답

```python
def find_parent(parent, x):
    if parent[x] != x:
        parent[x] = find_parent(parent, parent[x])

    return parent[x]

def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)

    if a < b:
        parent[b] = a
    else:
        parent[a] = b

n, m = map(int, input().split())

parent = [0] * (n + 1)

for i in range(n+1):
    parent[i] = i

for _ in range(m):
    cal_type, a, b = map(int, input().split())
    if cal_type == 0:
        union_parent(parent, a, b)
    else:
        if find_parent(parent, a) == find_parent(parent, b):
            print("YES")
        else:
            print("NO")

```

## 1-2.답안 예시

```python
def find_parent(parent, x):
    if parent[x] != x:
        parent[x] = find_parent(parent, parent[x])

    return parent[x]

def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)

    if a < b:
        parent[b] = a
    else:
        parent[a] = b

n, m = map(int, input().split())

parent = [0] * (n + 1)

for i in range(n+1):
    parent[i] = i

for _ in range(m):
    order, a, b = map(int, input().split())
    if order == 0:
        union_parent(parent, a, b)
    else:
        if find_parent(parent, a) == find_parent(parent, b):
            print("YES")
        else:
            print("NO")

```

## 1-3.답안 예시와 내가 푼 답의 차이점
클루스칼을 많이 사용하면서 풀어봐야겠다.   


## 2.도시 분할 계획
마을에 N개의 집과 도로 M개가 있다.  
마을들을 2개의 분리된 마을로 분리하는데 각 분리된 마을에서 집들이 연결되있도록 한다.  
각 분리된 마을을 최소한으로 길을 만들려고 한다. 

## 2-1.내가 푼 답(못풀었음)

```python
def find_parent(parent, x):
    if parent[x] != x:
        parent[x] = find_parent(parent, parent[x])

    return parent[x]

def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)

    if a < b:
        parent[b] = a
    else:
        parent[a] = b

n, m = map(int, input().split())

parent = [0] * (n)


edges = []
result = 0

for i in range(1, n + 1):
    parent[i] = i

for _ in range(e):
    a, b, cost = map(int, input().split())
    #비용순으로 하기위해 튜플로 저장
    edges.append((cost, a, b))

edges.sort()

for edge in edges:
    cost, a, b = edge

    if find_parent(parent, a) != find_parent(parent, b):
        union_parent(parent, a, b)
        result += cost
        
print(result)
```
  
## 2-2.답안 예시

```python
def find_parent(parent, x):
    if parent[x] != x:
        parent[x] = find_parent(parent, parent[x])

    return parent[x]

def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)

    if a < b:
        parent[b] = a
    else:
        parent[a] = b

n, m = map(int, input().split())

parent = [0] * (n)


edges = []
result = 0

for i in range(1, n + 1):
    parent[i] = i

for _ in range(e):
    a, b, cost = map(int, input().split())
    #비용순으로 하기위해 튜플로 저장
    edges.append((cost, a, b))

edges.sort()

for edge in edges:
    cost, a, b = edge

    if find_parent(parent, a) != find_parent(parent, b):
        union_parent(parent, a, b)
        result += cost
        last = cost        
print(result - cost)
```

## 2-3.답안 예시와 내가 푼 답의 차이점
마지막에 가장 큰 노드를 없애 최소 신장 트리에서 2개의 마을로 분리한다는 생각을 하지 않았는데 그러한 부분을 잘 읽고 생각을 해야겠다.

## 3.커리큘럼
온라인 강의를 듣는데 각 온라인 강의는 선수과목이 있다.  
N개의 강의가 있을 때 동시에 여러개의 강의를 들을 수 있다고 한다.  
N개의 강의가 있을 때 수강 하기까지의 최소시간을 구하라.

## 3-1.내가 푼 답(못 풀었음)

```python
```

## 3-2.답안 예시

```python
from collections import deque
import copy

v = int(input())

indegree = [0] * (v + 1)

#각 노드에 연결된 간선 정보
graph = [[] for i in range(v + 1)]

time = [0] * (v + 1)

for i in range(1, v + 1):
    data = list(map(int, input().split()))
    time[i] = data[0]
    for x in data[1: -1]:
        indegree[i] += 1
        graph[x].append(i)

#위상 정렬 함수
def topology_sort():
    result = copy.deepcopy(time)
    q = deque()

    #진입 차수가 0인 노드를 큐에 삽입
    for i in range(1, v + 1):
        if indegree[i] == 0:
            q.append(i)

    while q:

        now = q.popleft()

        for i in graph[now]:
            result[i] = max(result[i], result[now] + time[i])
            indegree[i] -= 1

            if indegree[i] == 0:
                q.append(i)

    for i in range(1, v+1):
        print(result[i])

topology_sort()

```

## 3-3.답안 예시와 내가 푼 답의 차이점
일단 가장 기본이 되는 코드를 계속 써보면서 연습하는 것이 중요하다.  
그리고 사소하지만 파이썬에서 [1:-1]로 마지막 부분빼고 모든 것을 가져온다는 것도 다른 곳에서 유용하게 쓸 수 있을거같다. 