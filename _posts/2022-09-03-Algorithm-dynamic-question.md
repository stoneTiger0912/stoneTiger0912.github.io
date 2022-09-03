---
title:  "[Algorithm] 알고리즘 정리(15)- 다이나믹 프로그래밍 기출"
excerpt: "코딩 테스트 대비 알고리즘을 정리 하는 글"

categories:
  - algorithm
tags:
  - [Blog, algorithm, python, dynamic_programming]

toc: true
toc_sticky: true
 
date: 2022-09-03
last_modified_at: 2022-09-03
---

# 문제

## 1.금광
NxM의 금광이 있다. 
첫번째 줄 어디서부터 
1. 오른쪽 위
2. 오른쪽 
3. 오른쪽 아래
3가지중 하나를 사용하여 금의 최대 크기를 구하라


## 1-1.내가 푼 답

```python
import sys

input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    dp = [[0]* m for _ in range(n)]

    input_list = list((map(int, input().split())))
    
    map_list = []
    
    for i in range(0, len(input_list), m):
       map_list.append(input_list[i: i + m])

    for y in range(m):
        for x in range(n):
            dp[x][y] += map_list[x][y]
            if x-1 < n and y+1 < m:
                dp[x - 1][y + 1] = max(dp[x-1][y+1], dp[x][y])
            
            if y+1 < m:
                dp[x][y+1] = max(dp[x][y+1], dp[x][y])
            
            if x+1 < n and y+1 < m:
                dp[x+1][y+1] = max(dp[x+1][y+1], dp[x][y])
    
    result = 0
    for x in range(n):
        result = max(result, dp[x][m-1])

    print(result)

```

## 1-2.다른 풀이

```python

```

## 1-3.고찰
 n과 m 그리고 x와 y에 좌표를 정확하게 이해해야 헷갈리지 않을 거 같다.


## 2.정수 삼각형
맨 위층부터 시작하여 밑으로 내려갈수록 아래에 있는 수 중 하나를 선택하여 합을 구한 값이 최대가 되는 경로를 구하라.

## 2-1.내가 푼 답

```python
n = int(input())
dp = [[0] * i for i in range(1, n+1)]
map_list = []
for _ in range(n):
    map_list.append(list(map(int, input().split())))

for i in range(n):
    for j in range(i+1):
        dp[i][j] += map_list[i][j]
        if i+1 < n:
            dp[i+1][j] = max(dp[i+1][j], dp[i][j])
        
        if i+1 < n and j+1 < i+2:
            dp[i+1][j+1] = max(dp[i+1][j+1], dp[i][j])

result = 0
for i in range(n):
    result = max(result, dp[n-1][i])

print(result)
```
  
## 2-2.다른 풀이

```python

```

## 2-3.고찰
반복문에서 j의 범위를 정하는게 조금 생각을 할만한 문제였다.

## 3.퇴사
N+1일에 퇴사하기 위해 N일동안 많은 상담을 하려고 한다. 
상담에 걸리는 기간 Ti, 비용 Pi가 주어졌을 때 최대 수익을 구하라.

## 3-1.내가 푼 답

```python
n = int(input())

dp = [0] * 16

day_list = [0]
for _ in range(n):
    day_list.append(list(map(int, input().split())))

for i in range(1, n+1):
    if day_list[i][0] + i <= n+1:
        dp[i] += day_list[i][1]
        tmp = day_list[i][0] 
        for j in range(i+tmp, n+1):
            dp[j] = max(dp[j], dp[i])
print(max(dp))
```
  
## 3-2.다른 풀이

```python
```

## 3-3.고찰
마지막날에 1일 일하는 경우가 있는데 그것까지 생각하여 범위를 정해야한다.

## 4.병사 배치하기
병사를 배치할때 전투력이 높은 병사부터 내림차순으로 배치를 한다.   
병사의 수가 최대가 되려고 할 때를 구하라.

## 4-1.내가 푼 답

```python
n = int(input())

people_list = list(map(int, input().split()))

people_list.reverse()
dp = [1] * n

for i in range(1, n):
    for j in range(0, i):
        if people_list[j] < people_list[i]:
            dp[i] = max(dp[i], dp[j] + 1)

print(n - max(dp))
```
  
## 4-2.다른 풀이

```python
```

## 4-3.고찰

## 5.못생긴 수
오직 2 3 5만을 소인수로 가지는 수를 의미한다.  
1은 못생긴 수이다.
n번째 못생긴 수를 구하라.

## 5-1.내가 푼 답

```python
```
  
## 5-2.다른 풀이

```python
n = int(input())

ugly = [0] * n

ugly[0] = 1

i2 = i3 = i5 = 0

next2, next3, next5 = 2, 3, 5

for i in range(1, n):
    ugly[i] = min(next2, next3, next5)
    if ugly[i] == next2:
        i2 += 1
        next2 = ugly[i2] * 2

    if ugly[i] == next3:
        i3 += 1
        next3 = ugly[i3] * 3

    if ugly[i] == next5:
        i5 += 1
        next5 = ugly[i5] * 2

print(ugly[n - 1])
```

## 5-3.고찰
처음에 힙큐로 풀까 생각 하였는데 중복되는 수가 나와서 해결하지 못하였다.


## 6.편집거리
두 문자열 A,B가 있을 때 A를 편집하여 문자열 B로 만들려고 한다.
1. 삽입
2. 삭제
3. 교체
3가지를 이용하여 최소퍈집거리를 계산하는 프로그램을 작성하라.

## 6-1.내가 푼 답

```python
```
  
## 6-2.다른 풀이

```python
def edit_dist(str1, str2):
    n = len(str1)
    m = len(str2)

    dp = [[0] * (m+1) for _ in range(n+1)]

    for i in range(1, n+1):
        dp[i][0] = i
    for j in range(i, m+1):
        dp[0][j] = j
    
    for i in range(1, n+1):
        for j in range(1, m+1):
            if str1[i - 1] == str2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1]

            else:
                dp[i][j] = 1 + min(dp[i][j - 1], dp[i-1][j], dp[i-1][j-1])
    return dp[n][m] 

str1 = input()
str2 = input()

print(edit_dist(str1, str2))
```

## 6-3.고찰
답지에서는 2차원 테이블에 각각 문자열을 0행과 0열에 각각 집에넣는다.  
같은 위치에 같은 문자면 전 위치의 dp랑 같고 아니면 계산을 해야한다.  
삽입의 경우 왼쪽 즉 dp[i][j - 1]의 값이고  
삭제의 경우 위쪽 즉 dp[i-1][j]이라고  
교체의 경우 p[i-1][j-1]가 되는데  
여기서 최소의 값을 구하여 최종적인 값을 구하면 된다.
