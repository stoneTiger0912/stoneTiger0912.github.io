---
title:  "[Algorithm] 알고리즘 정리(1)-그리디 알고리즘"
excerpt: "코딩 테스트 대비 알고리즘을 정리 하는 글"

categories:
  - Algorithm
tags:
  - [Blog, algorithm, python, Greedy_algorithm]

toc: true
toc_sticky: true
 
date: 2022-05-16
last_modified_at: 2022-05-16
---

# 그리디 알고리즘
- 현재 상황에서 가장 최선의 선택을 하는 알고리즘


# 그리디 알고리즘의 예시
동전이 500원, 100원, 50원, 10원이 무한히 존재 할 경우 손님에게 거슬러 줘야 할 돈이 N원일 때 
거슬러야 할 동전의 최소 개수를 구하는 문제가 그리디 알고리즘의 대표적인 문제이다.  

이문제의 해결 방법은 가장 큰 단위의 돈부터 거스르는 것이다.
```python
n = int(input())
count = 0

coin_type = [500, 100, 50, 10]

for coin in coin_type:
    count += n // coin
    n = n % coin
    
print(count)
```

## 1.큰수의 법칙
다양한 수들의 배열이 있을 때 주어진 수를 M번 더하여 가장 큰 수를 만드는 법칙
단 해당 수들이 K번 반복 할 수 없다.
  
예를 들어
2 4 5 4 6이고 M이 8이고 K가 3일 경우 6 6 6 5 6 6 6 5가 된다.  
단 서로 다른 인덱스에 같은 수가 있을 경우 따로 할 수 있다.  

## 1-1.내가 푼 답
```python
n, m, k = map(int, input().split())
num_list = list(map(int,input().split()))
result = 0
num_list.sort(reverse=True)

while m > 0:
    result += num_list[0] * k
    m -= k
    if m >= 1:
        m -= 1
        result += num_list[1]
        
print(result)
```

## 1-2.답안 예시
```python
n, m, k = map(int, input().split())

data = list(map(int, input().split()))

data.sort()
first = data[n - 1]
second = data[n - 2]

result = 0

while True:
    for i in range(k):
        if m == 0:
            break
        result += first
        m -= 1
        
    if m == 0:
        break
    result += second
    m -= 1
    
print(result)
```

## 1-3.답안 예시와 내가 푼 답의 차이점
답안 예시는 순서대로 한번씩 큰 수 k번 작은수 1번 순서대로 했지만
내가 푼 것은 k번을 한번에 한다음에 m에서 k번을 뺐는데 이럴경우 m이 0보다 작아질수도 있기 때문에 
내가 푼것은 적절하지 않다.  
그러므로 답안같이 1번씩 빼고 0이될경우 빠져나가는 break문을 만들어야한다.

## 2.숫자 카드 게임
N X M의 카드가 놓여 있는데 N은 행 M은 열을 의미  
먼저 뽑고자 하는 카드가 포함 되어 있는 행을 선택  
행의 가장 숫자가 낮은 수를 선택      
위 행위에서 가장 큰수를 고를 프로그램을 만들어라

## 2-1.내가 푼 답
```python
n, m = map(int, input().split())

card_list = [list(map(int, input().split())) for _ in range(n)]

result = 0

for card_row in card_list:
    card_row.sort()
    if result <= card_row[0]:
        result = card_row[0]
        
print(result)
```
## 2-2.답안 예시
```python
#한줄씩 입력
n, m = map(int, input().split())

result = 0

for i in range(n):
    data = list(map(int, input().split()))
    min_value = min(data)
    result = max(result, min_value)
```

## 2-3.답안 예시와 내가 푼 답의 차이점
내가 푼 답은 여러줄을 한번에 읽어와 1줄씩 읽은다음 정렬을 해서 가장 작은 답을 구한뒤
그것과 이전에 구했던 result값과 비교해서 크면 result값을 바꾸는 형식으로 했는데 
답안은 한줄을 입력 받는 동시에 가장 작은 값을 min함수로 찾고 이전에 찾았던 수중에 가장 큰수를 찾는데 
sort는 O(nlogn)이고 min(n)이므로 min이 빠르므로 다음번에는 min함수를 쓰는 것이 좋다.


## 3.숫자 1을 만들자
어떤 수를 1이 될 때 까지 두 과정 중 반복을 하는 것의 최소 횟수를 출력
n 에서 1을 뺀.
n 에서 k로 나눈다

## 3-1.내가 푼 답
```python
n, k = map(int, input().split())
count = 0
while n != 1:
    if n % k == 0:
        n = n // k
    
    else:
        n -= 1    
    
    count += 1
    
print(count)
```
## 3-2.답안 예시
```python
n, k = map(int, input().split())
result = 0

while n >= k:
    while n % k != 0:
        n -= 1
        result += 1

    n //= k
    result += 1

while n > 1:
    n -= 1
    result += 1

print(result)
```

## 3-3.답안 예시와 내가 푼 답의 차이점
차례대로 풀면 쉽게 풀리는 것 같다.