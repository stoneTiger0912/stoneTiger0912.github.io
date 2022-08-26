---
title:  "[Algorithm] 알고리즘 정리(10)- 그리디 알고리즘 기출"
excerpt: "코딩 테스트 대비 알고리즘을 정리 하는 글"

categories:
  - algorithm
tags:
  - [Blog, algorithm, python, Greedy_algorithm]

toc: true
toc_sticky: true
 
date: 2022-08-17
last_modified_at: 2022-08-18
---

# 문제

## 1.모험가 길드
N명의 모험가가 있는데 각각 공포도를 측정한다.  
공포도가 높으면 위험상황에서 대처가 늦는다.  
공포도가 x인 모험가는 반드시 x명 이상으로 구성한 모험가 그룹에 참여해야한다.  
최대 몇개의 그룹을 만들 수 있을 지 구하시오

## 1-1.내가 푼 답

```python
import sys

n = int(input())

hunter_list = list(map(int, sys.stdin.readline().rstrip().split()))

hunter_list.sort(reverse=True)

count = 0

while hunter_list != []:
    i = 0
    if hunter_list[i] <= len(hunter_list):
        count += 1
        hunter_list = hunter_list[hunter_list[i]:]
    else:
        break

print(count)

```

## 1-2.다른 풀이

```python

```

## 1-3.고찰


## 2.곱하기 혹은 더하기
각 자리 숫자로만 이루어진 문자열 s가 존재할때 
원쪽 부터 오른쪽으로 하나씩 '*' 혹은 '+'를 넣어 가장 큰수를 구하는 프로그램을 작성하라  
단 기존의 *와 +의 계산순서는 무시하고 왼쪽부터 순서대로 한다.

## 2-1.내가 푼 답

```python
cal = list(map(int, input()))

result = cal[0]


for i in range(1, len(cal)):
    if result * cal[i] >= result + cal[i]:
        result = result * cal[i]
    else:
        result = result + cal[i]

print(result)
```
  
## 2-2.다른 풀이

```python

```

## 2-3.고찰

## 3.문자열 뒤집기
0과 1로 이루어진 문자열 s가 있다.  
s에 있는 모든 숫자를 같게 만들려고 한다.  
문자를 뒤집기 위해서는 연속된 하나 이상의 숫자를 잡고 모두 뒤집는 것이다.
예를 들어 00011000이 있다고 하자
전체를 뒤집으면 11100111 --> 4,5번쨔 뒤집기 --> 11111111 2번  
4,5번째 뒤집기 11111111 1번  
최소 값은 1이다.  
문자열이 주어졌을 때 최소로 하는 횟수를 구하라


## 3-1.내가 푼 답

```python
cal = list(map(int, input()))

turn_list = []
turn_list.append(cal[0])

for i in range(1, len(cal)):
    if cal[i-1] != cal[i]:
        turn_list.append(cal[i])
    else:
        continue

result = turn_list.count(1) if turn_list.count(0) > turn_list.count(1)  else turn_list.count(0)

print(result)
```
  
## 3-2.다른 풀이

```python

```

## 3-3.고찰

## 4.만들 수 없는 금액
N개의 동전이 있다.  
이때 N개의 동전으로 만들 수 없는 양의 정수 금액 중 최솟값을 구하라

## 4-1.내가 푼 답(틀림)

```python
n = int(input())

coin_list = list(map(int, input().split()))

coin_sum = sum(coin_list)

array = [False] * (coin_sum + 1)
array[coin_sum] = True
coin_list.sort(reverse=True)


for i in range(n):
    for j in range(i, n):
        tmp = coin_sum - coin_list[j]
        array[tmp] = True

    coin_sum -= coin_list[i]

for i in range(1, sum(coin_list) + 1):
    if array[i] == False:
        print(i)
        break
```
  
## 4-2.다른 풀이

```python

```

## 4-3.고찰
처음에 접근한 것은 동전 총 합의 범위를 가지는 반복문으로 1씩 줄어들면서 True인 경우 거기서 동전들을 각각 빼서 하는 방법으로 했는데 풀어보니까 이렇게 하면 동전의 개수를 신경을 쓰지않고 무한으로 사용 할 수 있다는 문제점을 발견했다.  
조금 더 문제를 정확하게 보는 자세를 가져야겠다.  

## 5.볼링공 구하기
A, B가 볼링을 치고있다.  
볼링공이 N개가 있는데 각 볼링공마다 무게가 있고 공의 번호가 1번부터 순서대로 부여된다.  
같은 무게여도 서로다른 경우로 간주된다. 무게는 M까지 존재한다.  
A, B가 서로 무게가 다른 볼링공을 구하는 경우의 수를 구하라.

## 5-1.내가 푼 답

```python
import sys

n, m = map(int, input().split())

weight_list = list(map(int, sys.stdin.readline().split()))

# weight_list.sort(reverse=True)

count = 0

for i in range(n):
    for j in range(i, n):
        if weight_list[i] == weight_list[j]:
            continue
        else:
            count += 1

print(count)
```
  
## 5-2.다른 풀이

```python
#조합으로 풀기
```

## 5-3.고찰


## 6.무지의 먹방 라이브
회전판에 N개의 음식이 있다. 각 음식에는 1부터 N까지 번호가 존재한다.  
각 음식을 섭취하는 데 일정시간이 소요된다.  
1. 1번 부터 먹고 회전판이 번호가 증가하는 순서대로 음식을 먹는다.
2. 마지막 번호를 먹으면 다시 1번 음식을 먹는다.
3. 음식은 1초만 먹고 다음 음식을 먹는다.
4. 먹방 시작 후 k초 후에 네트워크 장애로 방송이 중단되었다.
5. 네트워크 장애가 발생한 시간이 k초가 존재 할 때 몇 번 음식부터 다시 먹는지 확인하라.
6. 더 쉅치해야 할 음식이 없다면 -1을 반환하면 된다. 


## 6-1.내가 푼 답(런타임 에러)

```python
from collections import deque

def solution(food_times, k):
    q = deque()
    for i in range(len(food_times)):
        q.append([food_times[i], i + 1])
    

    for i in range(1, k + 1):
        food = q.popleft()
        print(food)
        food[0] -= 1
        if food[0] == 0:
            continue
        else:
            q.append(food)

        if i == k:
            break
    last = q.popleft()

    if last == []:
        answer = -1
    else:
        answer = last[1]
    return answer

# foods_times = list(map(int, input().split()))

# k = int(input())

# result = solution(foods_times, k)
# print(result)
```
  
## 6-2.다른 풀이

```python
import heapq


def solution(food_times, k):
    if sum(food_times) <= k:
        return -1

    q = []
    for i in range(len(food_times)):
        heapq.heappush(q, (food_times[i], i + 1))

    sum_value = 0
    previous = 0

    length = len(food_times)

    while sum_value + ((q[0][0] - previous) * length) <= k:
        now = heapq.heappop(q)[0]
        sum_value += (now - previous) * length
        length -= 1
        previous = now
    print(q)
    result = sorted(q, key=lambda x: x[1])
    # (k - sum_value) => 3 
    # length = 2
    # 3초 후에 선택될 데이터를 return 
    # 0  1
    # 2 '3'
    # [(8,1),(6,2)]
    return result[(k - sum_value) % length][1]
```

## 6-3.고찰
답안은 우선순위 큐를 사용하여 큐를 크기가 큰 순서대로 한 다음에 큰 수 부터 차례로 한 것이 런타임 에러를 해결 할 수 있는 것 같다. 
