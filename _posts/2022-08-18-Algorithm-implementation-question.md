---
title:  "[Algorithm] 알고리즘 정리(10)- 구현 알고리즘 기출"
excerpt: "코딩 테스트 대비 알고리즘을 정리 하는 글"

categories:
  - algorithm
tags:
  - [Blog, algorithm, python, Implementation_algorithm]

toc: true
toc_sticky: true
 
date: 2022-08-18
last_modified_at: 2022-08-18
---

# 문제

## 1.럭키 스트레이트
필살기를 사용하기 위해서는 캐릭터의 점수를 N이라고 할 때 자릿수를 기준으로 점수 N을 반으로 나누어 왼쪽 부분의 각 자릿수 합과 오른쪽 부분의 각 자릿수의 합을 더한 값이 동일한 상황일 때 사용가능하다.  
현재점수가 주어졌을 때 필살기를 쓸 수 있는지 구하라.

## 1-1.내가 푼 답

```python
score = list(map(int, input()))

score_size = len(score)

score_size = score_size // 2

left_score = score[0:score_size]
right_score = score[score_size: -1]

if sum(left_score) == sum(right_score):
    print("LUCKY")
else:
    print("READY")

```

## 1-2.다른 풀이

```python

```

## 1-3.고찰


## 2.문자열 재정렬
알파벳 대문자와 숫자로 구성된 문자열이 입력된다. 이때 모든 알파벳을 순서대로 정렬 후 그 뒤에 모든 숫자를 더한 값을 출력하라.

## 2-1.내가 푼 답

```python
input_data = list(input())

alphabet_list = []

result = 0

for data in input_data:
    if ord(data) >= ord('0') and ord(data) <= ord('9'):
        result += int(data)
    else:
        alphabet_list.append(data)

alphabet_list.sort()
for i in alphabet_list:
    print(i, end='')
print(result)
```
  
## 2-2.다른 풀이

```python

```

## 2-3.고찰

## 3.문자열 압축
문자열에서 값은 값이 연속으로 나오면 그 문자의 개수를 표현하여 더 짧은 문자열을 만든다.  
aabbaccc의 경우 2a2ba3c


## 3-1.내가 푼 답

```python
```
  
## 3-2.다른 풀이

```python
def solution(s):
    answer = s[:]
    # le: 압축할 길이
    for le in range(1,len(s)//2+1):
        result = ''
        cnt = 1
        
        # 현재 문자열과 다음 문자열을 비교해서 
        for st in range(0,len(s),le):
        
            # 같으면 cnt만 증가
            if s[st:st+le]==s[st+le:st+2*le]:
                cnt += 1
                
            # 다르면 현재까지 압축한 결과 붙이기
            else:
                if cnt>1:
                    result += (str(cnt)+s[st:st+le])
                else:
                    result += s[st:st+le]
                cnt = 1
                
        if len(result)<len(answer):
            answer = result[:]
    
    return len(answer)
```

## 3-3.고찰
반복문을 할 때 3번째 변수로 le값을 넣어준다는 생각을 하지 못하였고 그 부분이 막히면서 슬라이싱을 하기 힘들었다. 그 부분이 부족했다.

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

weight_list.sort(reverse=True)

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

foods_times = list(map(int, input().split()))

k = int(input())

result = solution(foods_times, k)
print(result)
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
