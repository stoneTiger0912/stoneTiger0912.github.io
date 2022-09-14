---
title:  "[Algorithm] 알고리즘 정리(13)- 정렬 알고리즘 기출"
excerpt: "코딩 테스트 대비 알고리즘을 정리 하는 글"

categories:
  - algorithm
tags:
  - [Blog, algorithm, python, sorting_algorithm]

toc: true
toc_sticky: true
 
date: 2022-08-28
last_modified_at: 2022-08-28
---

# 문제

## 1.국영수
학생 N명이 주어졌을 때 
1. 국어점수 감소하는 순
2. 국어점수가 같으면 영어점수가 증가하는 순서
3. 둘다 같으면 수학점수가 감소하는 순서
4. 다 같으면 이름이 사전순으로 증가하는 순서

## 1-1.내가 푼 답

```python
import sys
input = sys.stdin.readline
n = int(input())

student_list = []

for _ in range(n):
    name, kor, eng, math = input().split()
    student_list.append([name, int(kor), int(eng), int(math)])

sort_list = sorted(student_list, key= lambda x: (-x[1], x[2], -x[3], x[0]))

for i in range(n):
    print(sort_list[i][0])
```

## 1-2.다른 풀이

```python

```

## 1-3.고찰
pypy3로 해야 시간초과가 안난다.  


## 2.안테나
일직선상에 여러채의 집이 있다.   
특정위치에 안테나를 설치하려고 할때 모든 거리의 총합이 최소가 되려고 한다. 
동일한 위치에 집이 여러개 있을 수 있다.

## 2-1.내가 푼 답(틀림)

```python
n = int(input())

arr = list(map(int, input().split()))

result = sum(arr) // n

print(result)
```
  
## 2-2.다른 풀이

```python
n = int(input())
arr = list(map(int, input().split()))
arr.sort()

print(arr[(n - 1) // 2])
```

## 2-3.고찰
그냥 생각 없이 이게 정렬을 이용해야하나 하고 풀다가 틀렸는데 답이 여러개 나오면 가장 작은 것을 하라고 하였으므로 문제를 잘 읽어야한다.  
집들중에서 가장 가운데 있는 지점을 골라야하므로 정렬을 해야한다.  


## 3.실패율
실패율 = 스테이지에 도달하였으나 클리어하지못한 플레이어수/스테이지에 도달한 플레이어수  
전체 스테이지의 수가 n, 사용자가 멈춰있는 스테이지 번호를 stages라 할때 실패율이 높은 스테이지부터 내림차순으로 정렬
## 3-1.내가 푼 답(틀림)

```python

```
  
## 3-2.다른 풀이

```python
def solution(N, stages):
    stages.sort()
    # print(stages)
    failLst = []
    # 1부터 n까지의 스테이지에 대한 실패율 구하기
    for i in range(1, N+1):
        # i의 개수 카운트 
        count = stages.count(i)
        # 분모가 0일 때(ZeroDivisionError) 처리
        failure = count/len(stages) if len(stages) != 0 else 0 
        failLst.append((failure, i))
        stages = stages[count:]
    
    failLst.sort(key=lambda x:-x[0])
    failLst = [x[1] for x in failLst]
    return failLst
```

## 3-3.고찰
분모가 0일때를 생각을 안했고 stages의 값을 슬라이스하여 하는 방법이 더 쉽게 풀 수 있는 것 같다.

## 4.카드 정렬하기
카드의 수가 A,B인 카드뭉치가 있다.  
카드들을 뭉칠때 효율적으로 뭉치는 방법을 구하라  
예를 들어 10 20 40이 있다고 하면 10 20을 묶어서 하면 
10+20+30+40= 100  
10 40을 묶어서 하면
10+40+50+20=120이 된다.  
최소 몇번의 비교가 필요한지 구하라.

## 4-1.내가 푼 답

```python
```
  
## 4-2.다른 풀이

```python
import heapq
import sys

N = int(input())
card_deck = []
for _ in range(N):
    heapq.heappush(card_deck, int(sys.stdin.readline()))


if len(card_deck) == 1: #1개일 경우 비교하지 않아도 된다
    print(0)
else:
    answer = 0
    while len(card_deck) > 1: #1개일 경우 비교하지 않아도 된다
        temp_1 = heapq.heappop(card_deck) #제일 작은 덱
        temp_2 = heapq.heappop(card_deck) #두번째로 작은 덱
        answer += temp_1 + temp_2 #현재 비교 횟수를 더해줌
        heapq.heappush(card_deck, temp_1 + temp_2) #더한 덱을 다시 넣어줌
    
    print(answer)
    
```

## 4-3.고찰
힙큐를 사용하면 편하게 하는 것을 알게 되었다.  
자주 사용해야겠다.