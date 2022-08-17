---
title:  "[Algorithm] 알고리즘 정리(10)- 그리디 알고리즘 기출 문제"
excerpt: "코딩 테스트 대비 알고리즘을 정리 하는 글"

categories:
  - algorithm
tags:
  - [Blog, algorithm, python, Greedy_algorithm]

toc: true
toc_sticky: true
 
date: 2022-08-17
last_modified_at: 2022-08-17
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

## 4-1.내가 푼 답

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
