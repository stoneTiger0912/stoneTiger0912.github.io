---
title:  "[Algorithm] 알고리즘 정리(5)- 이진 탐색 알고리즘"
excerpt: "코딩 테스트 대비 알고리즘을 정리 하는 글"

categories:
  - algorithm
tags:
  - [Blog, algorithm, python, binary_search_algorithm]

toc: true
toc_sticky: true
 
date: 2022-08-09
last_modified_at: 2022-08-09
---

# 순차 탐색 알고리즘
- N개의 데이터가 있을 때, 그 데이터를 하나씩 확인하여 어떠한 처리를 한 것
- 예를 들어 탐욕 알고리즘의 동전 계산하기
- 리스트 안에 있는 특정 데이터를 찾기 위해 앞에서부터 데이터를 하나씩 차례대로 확인하는 방법 
- O(N)의 시간 복잡도를 가진다.

# 이진 탐색 알고리즘
- 배열 내부가 정렬 되어 있을 때만 사용가능한 알고리즘
- 3개의 변수를 사용하는데 시작점, 중간점, 끝점을 사용한다.
- 찾으려는 데이터와 중간 데이터를 비교하여 원하는 데이터를 찾는게 이진 탐색 과정이다.
- 탐색 범위가 매우 큰 상황(1000만 단위 이상의 값)이면 이진탐색과 같이 O(logN)의 속도를 생각해야한다.
- 탐색 범위가 매우 크므로 빠르게 입력 받기 위해서는 다음을 사용한다.

  ```python
  import sys

  input_data = sys.stdin.readline().rstrip()

  print(input_data)
  ```

## 이진 탐색 알고리즘을 짜는 예시
1. 재귀 함수로 짜는 경우

```python
def binary_search(array, target, start, end):
    if start > end:
        return None
    mid = (start + end) // 2
    if array[mid] == target:
        return mid
    elif array[mid] > target:
        return binary_search(array, target, start, mid - 1)
    else:
        return binary_search(array, target, mid + 1, end)


n, target = list(map(int, input().split()))

array = list(map(int, input().split()))

result = binary_search(array, target, 0, n - 1)
if result == None:
    print("원소가 없습니다.")
else:
    print(result + 1)

```

1. 반복문으로 구현 하는 경우

```python
def binary_search(array, target, start, end):
    while start <= end:
        mid = (start + end) // 2
        if array[mid] == target:
            return mid
        elif array[mid] > target:
            end = mid - 1
        else:
            start = mid + 1
    
    return None

n, target = list(map(int, input().split()))

array = list(map(int, input().split()))

result = binary_search(array, target, 0, n - 1)
if result == None:
    print("원소가 없습니다.")
else:
    print(result + 1)
```

# 트리 자료구조
- 노드와 노드의 연결로 표현하는 자료구조
- 시스템이나 파일 시스템같은 곳에서 사용한다.
- 트리는 부모 자식 노드의 관계를 가진다.
- 최상단 노드를 루트 노드, 최하단 노드를 단말 노드라 한다.
  
# 이진 탐색 트리
- 부모 노드보다 왼쪽 자식 노드가 작다.
- 부모 노드보다 오른쪽 자식 노드가 크다.



# 문제

## 1.부품 찾기
각 부품에는 정수 형태의 고유한 번호가 있는데 손님이 M개의 부품을 요청했다.  
가게안에 모든 부품이 있는지 확인하라.

## 1-1.내가 푼 답

```python
import sys

def binary_search(array, target, start, end):
    if start >= end:
        return None
    
    mid = (start + end) // 2
    if array[mid] == target:
        return mid
    
    elif array[mid] > target:
        return binary_search(array, target, start, mid - 1)
    
    else:
        return binary_search(array, target, mid + 1, end)


n = int(input())
total_items = list(map(int, sys.stdin.readline().rstrip().split()))

m = int(input())
request_items = list(map(int, sys.stdin.readline().rstrip().split()))

total_items = sorted(total_items)

count = 0

for request_item in request_items:
    result = binary_search(total_items, request_item, 0, n - 1)
    if result == None:
        print("no", end=' ')
    else:
        print("yes", end=' ')

```

## 1-2.답안 예시(반복문 사용)

```python
def binary_search(array, target, start, end):
    while start <= end:
        mid = (start + end) // 2

        if array[mid] == target:
            return mid
        elif array[mid] > target:
            end = mid - 1
        else:
            start = mid + 1
    return None

n = int(input())
array = list(map(int, input().split()))
array.sort()

m = int(input())
x = list(map(int, input().split()))

for i in x:
    result = binary_search(array, i, 0, n - 1)
    if result != None:
        print("no", end=' ')
    else:
        print("yes", end=' ')
```

## 1-3.답안 예시와 내가 푼 답의 차이점
이진탐색이 어떤 형식으로 만들어지는지 생각하면서 풀면 어렵지 않게 해결 할 수 있다.
이외에도 계수정렬이나 집합 자료형 등으로도 구현 할 수 있다는 것 정도만 알면 될 것 같다.  

## 2.떡볶이 떡 만들기
떡의 총 길이는 H로 높이보다 긴떡은 H의 윗부분이 잘리고 낮은 떡은 안잘린다.  
예를 들어 19, 14, 10, 17일 때 15의 절단기로 자르면
15, 14, 10, 15가 된다.  
잘린떡은 차례로 4, 0, 0, 2 일때 손님이 가져가는 떡은 6이다.  
이때 손님이 M을 요청할 때 적어도 잘린 떡이 M이 되려면 절단기의 높이의 최대 길이를 구하라.  

## 2-1.내가 푼 답(못풀음)

```python
import sys

def binary_search(array, target, start, end):
    if start >= end:
        return None
    
    mid = (start + end) // 2
    if array[mid] == target:
        return mid + 1
    
    elif array[mid] > target:
        return binary_search(array, target, start, mid - 1)
    
    else:
        return binary_search(array, target, mid + 1, end)

n, m = map(int, input())

array = list(map(int, sys.stdin.readline().rstrip().split()))
array.sort()

binary_search(array, m, 0, array[-1] + 1)
```
  
## 2-2.답안 예시

```python
n, m = list(map(int, input().split()))

array = list(map(int, input().split()))

start = 0
end = max(array)

result = 0
while(start <= end):
    total = 0
    mid = (start + end) // 2
    for x in array:

        if x >= mid:
            total += x - mid
    
    if total < m:
        end = mid - 1
    
    else:
        result = mid
        start = mid + 1

print(result)
```

## 2-3.답안 예시와 내가 푼 답의 차이점
떡을 자르고 남는 부분을 계산한다는 점에서 어떻게 풀어야할지 감이 안와서 못풀었는데  
답안에서처럼 반복문으로 자른 후에 총 값을 계산하여 이진 탐색을 한다는 부분을 생각을 못하고  
못풀었다.  
이진 탐색 문제들을 좀더 풀어봐야겠다.