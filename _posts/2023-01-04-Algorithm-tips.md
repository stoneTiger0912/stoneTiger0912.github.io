---
title:  "[Algorithm] 알고리즘 풀때 도움이 될만한 것들"
excerpt: "코딩 테스트 대비 알고리즘을 정리 하는 글"

categories:
  - algorithm
tags:
  - [Blog, algorithm, python, Baekjoon]

toc: true
toc_sticky: true
 
date: 2023-01-04
last_modified_at: 2023-01-04
---
# 시간 복잡도
현재 코테 대부분은 1초 제한이 있으므로 대부분 약 1초에 1억번이 되기만 하면된다.  
다음보다 많이 걸리면 하지 말아야함
O(N): 약 1억
O(N^2): 약 1만
O(2^N): 약 20
O(N!): 10


# 큐
파이썬에서 알고리즘을 풀때 큐대신 deque를 쓴다
```python
from collection import deque
q = deque(1)
```

# 조합, 순열
영어 문자나 포함관계 등을 해결할때 쓰기 쉽다. 대신 O(N^2)이므로 많은 양은 할 수 없다.
```python
from itertools import *
#combinations는 iterator 이므로 바로 해야 메모리 초과 안남
for comb in combinations(range(1, 26), k)

#순열의 조합, 순서 상관 있음
for per in permutations(range(1, 26), k)
```

# 비트 마스킹
어떤 알파벳이 있는지 확인할 경우와 같이 set을 사용한 복잡한 계산을 풀 경우 사용한다.
```python
#백준 1062의 비트마스킹 사용한 부분
for comb in combinations(other, k - 5):
    tmp_res = 0
    comb_word = 0

    #필수 문자인 antic를 comb_word에 마스킹
    for i in need:
        comb_word |= (1 << (ord(i) - ord('a')))
    
    #comb 리스트에 담겨진 문자들을 마스킹 a << b a를 b비트만큼 왼쪽으로 시프트
    for i in comb:

        comb_word |= (1 << (ord(i) - ord('a')))

    #input 이었던 word_list의 것들과 비교
    #만약 input이 1101이고 comb_word가 1101이면 &하면 1101 이므로 포함된 단어가 같으면 비트마스크가 같게 나옴
    #만약 input이 1111이고 comb_word가 1101이면 &하면 3번째 값이 없으므로 1101 포함된 단어가 input에는 없고 comb에는 있으면 비트마스크가 다르게 나옴
    #만약 input이 1101이고 comb_word가 1111이면 &하면 1101 이므로 포함된 단어가 input에 단어가 comb에 포함되면 비트마스크가 같게 나옴
    for word in word_list:
        if comb_word & word == word:
            tmp_res += 1
    
```

# for-else문
for-else문은 for문이 문제없이 다돌경우 else문이 출력된다.
```python
for i in range(10):
    if i == 5:
        break
else:
    print("hello")
output:

for i in range(10):
    if i == 11:
        break
else:
    print("hello")
output: hello
```


# print(*list)
```python
numList = [1,2,3,4,5]
print(*a)

output: 1 2 3 4 5
```


# 힙큐
다익스트라 알고리즘시 사용하여 시간을 절약할 수 있다.
```python
import heapq
heapq.heappush(q, (0, start))
dist, now = heapq.heappop(q)
```


# 입력값의 개수를 모를때
try-except를 사용하여 입력값이 없을 경우 빠져나오도록 함
```python
while True:
    try:
        x, y = map(int, input().split())

    except EOFError:
        break
```
