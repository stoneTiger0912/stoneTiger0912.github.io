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

# Combination, 
영어 문자나 포함관계를 해결할때 쓰기 쉽다.
```python
from itertools import combination
```

# 비트 마스킹


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