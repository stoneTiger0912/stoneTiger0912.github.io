---
title:  "[Algorithm] 알고리즘 정리(14)- 이진탐색 알고리즘 기출"
excerpt: "코딩 테스트 대비 알고리즘을 정리 하는 글"

categories:
  - algorithm
tags:
  - [Blog, algorithm, python, binary_search_algorithm]

toc: true
toc_sticky: true
 
date: 2022-09-01
last_modified_at: 2022-09-01
---

# 문제

## 1.정렬된 배열에서 특정 수의 개수 구하기
n개의 원소를 오른차순으로 정렬되어있다. x가 등장하는 횟수를 계산하라.


## 1-1.내가 푼 답

```python
import sys

input = sys.stdin.readline

n, x = map(int, input().split())

num_list = list(map(int, input().split()))

visited = [False] * n

global count

count = 0

def binary_search(num_list, start, end, target):
    global count
    mid = (end + start) // 2
    if start > end:
        return None
    if num_list[mid] == target and visited[mid] == False:
        count += 1
        visited[mid] == True
        if num_list[mid - 1] == target and visited[mid - 1] == False:
            binary_search(num_list, start, mid - 1, target)
        
        if num_list[mid + 1] == target and visited[mid + 1] == False:
            binary_search(num_list, mid + 1, end, target)

    if num_list[mid] > target:
        binary_search(num_list, start, mid - 1, target)
    
    elif num_list[mid] < target:
        binary_search(num_list, mid + 1, end, target)

binary_search(num_list, 0, len(num_list), x)

if count == 0:
    print(-1)
else:
    print(count)
```

## 1-2.다른 풀이

```python
import bisect

bisect.bisect_left()
bisect.bisect_right()#맨 오른쪽 좌표 +1
```

## 1-3.고찰
 


## 2.고정점 찾기
고정점이란 수열의 원소 중에서 그 값이 인덱스와 동일한 원소를 의미는
수열 n이 오름차순으로 정렬 되어있을때 고정점을 출력 고정점은 최대 1개이다.

## 2-1.내가 푼 답

```python
import sys

input = sys.stdin.readline

n = int(input())

arr = list(map(int, input().split()))

result = 0

def binary_search(arr, start, end):
    if start > end:
        return None
    
    mid = (start + end) // 2

    if arr[mid] == mid:
        return mid

    elif arr[mid] > mid:
        return binary_search(arr, start, mid - 1)
    
    else:
        return binary_search(arr, mid + 1, end)

result = binary_search(arr, 0, n - 1)

if result == None:
    print(-1)
else:
    print(result)
```
  
## 2-2.다른 풀이

```python

```

## 2-3.고찰
처음에 정렬이 안되어있는줄알고 이진탐색을 양옆으로 각각 1번씩 해줬는데 정렬이 되어있으므로 해줄필요가 없다는 것을 빠르게 파악했어야했다.


## 3.공유기 설치
n개가 수직선 위에 존재한다.  
집에 공유기 c개를 설치하려고 한다. 한집에는 공유기 한개만 되고 가장 인접한 두 공유기 사이의 거리를 가능한 크케하여 설치하려고 한다.   
c개의 공유기를 n개의 집에 설치하여 가장 인접한 두 공유기 사이의 거리를 최대로 하는 경우를 구하라.

## 3-1.내가 푼 답(틀림)

```python
import sys

input = sys.stdin.readline


n, c = map(int, input().split())
is_wifi = [False] * n

arr = []

for _ in range(n):
    arr.append(int(input()))

arr.sort()

#어차피 2개는 맨 앞 맨 뒤

c = c - 2
is_wifi[0] = True
is_wifi[n - 1] = True

start = 0
end = max(arr)
mid = (start + end) // 2

while c > 0:
    if start > end:
        break
    
    if is_wifi[mid] == False:
        is_wifi[mid] = True
        c -= 1
    
    if arr[mid] > mid:
        start = mid + 1
    elif arr[mid] < mid:
        end = mid - 1

```
  
## 3-2.다른 풀이

```python
n, c = map(int, input().split())

array = []
for i in range(n):
    array.append(int(input()))

array.sort()


def binary_search(array, start, end):
    while start <= end:
        mid = (start + end) // 2
        current = array[0]
        count = 1

        for i in range(1, len(array)):
            if array[i] >= current + mid:
                count += 1
                current = array[i]

        if count >= c:
            global answer
            start = mid + 1
            answer = mid
        else:
            end = mid - 1


start = 1
end = array[-1] - array[0]
answer = 0

binary_search(array, start, end)
print(answer)
```

## 3-3.고찰
처음 생각 했던 것은 맨 뒤랑 맨앞을 먼저 채우고 이진탐색이 된 곳을 체크해서 그 차이가 가장 작은 값을 구한다고 생각했었는데 틀린것 같다.

## 4.가사 검색
와일드카드인 ?을 사용하여 특정 키워드가 몇개 포합되어 있는지 확인하는 프로그램을 만들어라.  
예를 들어 fro??는 front, frost. frodo등이 매치된다.  
?는 접미사 아니면 접두사중 하나로만 이루어진다.

## 4-1.내가 푼 답(시간 초과)

```python
def solution(words, queries):
    answer = []
    for query in queries:
        count = 0
        for word in words:
            if len(query) == len(word):
                a = query.find("?")
                b = query.count("?")
                if a == 0:
                    if query[b: ] == word[b: ]:
                        count += 1
                        
                else:
                    if query[ :a] == word[ :a]:
                        count += 1
        answer.append(count)
                            
    return answer
```
  
## 4-2.다른 풀이

```python
def solution(words, queries):
    head, head_rev = {}, {}
    wc = []
    
    def add(head,word):
        node = head
        for w in word:
            if w not in node:
                node[w]={}
            node= node[w]
            if 'len' not in node:
                node['len'] = [len_word]
            else:
                node['len'].append(len_word)
        node['end']=True   
    
    for word in words:
        len_word = len(word)
        add(head,word)
        add(head_rev,word[::-1])
        wc.append(len_word)
        
    def search(head, querie):
        count=0
        node = head
        for q in querie:
            if q=='?':
                return node['len'].count(len_qu)
            elif q not in node:
                break
            node = node[q]
        return count

    li=[]
    for querie in queries:
        len_qu = len(querie)
        if querie[0]=='?':
            if querie[-1]=='?': 
                li.append(wc.count(len_qu))
            else: 
                li.append(search(head_rev, querie[::-1]))
        else:
            li.append(search(head, querie))
    return li
```

## 4-3.고찰
이진탐색을 쓰지않고 풀려고 했지만 역시 시간 초과가 났다.  
이진탐색 트리를 사용하면 풀리는 문제이다.