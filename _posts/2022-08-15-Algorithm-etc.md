---
title:  "[Algorithm] 알고리즘 정리(9)- 기타 알고리즘"
excerpt: "코딩 테스트 대비 알고리즘을 정리 하는 글"

categories:
  - algorithm
tags:
  - [Blog, algorithm, python, etc_algorithm]

toc: true
toc_sticky: true
 
date: 2022-08-15
last_modified_at: 2022-08-15
---

# 소수의 판별
- 소수를 판별할 때 무작정 1부터 n까지 나누어 떨어지는지 확이해도 좋지만 다음과 같은 방법으로 효율적으로 사용할 수 있다. 
- 약수는 가운데를 기준으로 대칭을 이루므로 제곱근만 보면 된다.  


## 제곱근 예시
```python
import math

def is_prime_number(x):
    for i in range(2, int(math.sqrt(x)) + 1):
        if x % i == 0:
            return False

        return True
```

## 에라토스테네스 체 알고리즘
1. 2부터 N까지 나열한다
2. 남는 수중에 가장 작은 수를 찾고 그 수에 배수를 모두 지운다.
- 예를 들어 26까지 있다면 처음 2를 선택하고 2의 배수를 모두 지우고 그다음 3의 배수를 지우고를 반복한다.
- 매우 적은 시간이 들지만 높은 메모리를 요구한다.

```python
import math

n = 1000
array = [True for _ in range(n + 1)]

for i in range(2,int(math.sqrt(n)) + 1):
    if array[i] == True:
        j = 2
        while i * j <= n:
            array[i * j] = False
            j += 1

for i in range(2, n + 1):
    if array[i]:
        print(i, end=' ')
```


# 투 포인터
- 리스트에 순차적으로 접근해야 할때 2개의 점의 위치를 기록하면서 처리한다.
- 예를들어 학생 40명을 일렬로 세운다음 2번~7번을 부를때 2번과 7번이 2개의 점이 된다.
- 특정한 합을 가지는 부분 연속 수열 찾기에 사용된다.  

## 예시1

| | | | | |
|---|---|---|---|---|  
|1|2|3|2|5|

- 합계가 5라고 하면 3개의 경우의 수가 존재한다.
- 2 3, 3 2, 5
- 2개의 변수(start, end)를 이용하여 리스트상의 위치를 기록한다.
1. 시작점과 끝점이 첫 번째 원소의 인덱스(0)를 가리키도록 한다.
2. 현재 부분합이 M(5)와 같으면 카운트 한다.
3. 현재 부분합이 5보다 작으면 end를 증가시킨다.
4. 현재 부분합이 5보다 크면 start를 증가시킨다.

```python
n = 5
m = 5
data = [1, 2, 3, 2, 5]

count = 0
interval_sum = 0
end = 0

for start in range(n):
    while interval_sum < m and end < n:
        interval_sum += data[end]
        end += 1
    if interval_sum == m:
        count += 1
        interval_sum -= data[start]
```

## 예시2
- 두 리스트의 합집합같은 문제에서 효율적으로 사용할 수 있다.
1. 정렬된 리스트a와b를 입력받는다.
2. 리스트 a에서 처리되지 않은 원소 중 가장 작은 원소 i를 가리킨다.
3. 리스트 b에서 처리되지 않은 원소 중 가장 작은 원소 j를 가리킨다.
4. A[i]와 B[j]중에서 더 작은 원소를 결과 리스트에 담는다.
5. 리스트 A와 B에서 더 이상 처리할 원소가 없을 때까지 반복한다.

```python
n, m = 3, 4
a = [1, 3, 5]
b = [2, 4, 6, 8]

result = [0] * (n + m)
i = 0
j = 0
k = 0

#리스트 b의 모든 원소가 처리되었거나 리스트 a의 원소가 더 작을 때
while i < n or j < m:
    if j >= m or (i < n and a[i] <= b[j]):
        result[k] = a[i]
        i += 1
    
    else:
        result[k] = b[j]
        j += 1
    k += 1

for i in result:
    print(i, end=' ')
```

# 구간 합 계산
- 연속적으로 나열된 N개의 숫자가 있을때 특정 구간의 모든 수를 합한 값을 구하는 문제이다.
- 합 계산문제는 여러개의 쿼리로 나오는데 각 쿼리는 [left, right]로 구성된다.
- 만약 매번 구간 합을 구하면 시간복잡도가 O(NM)으로 매우 커진다.
- 즉 미리 구해 저장을 하는 것이 효율적이고 이것을 접두사 합이다.
- 접두사 합이란 리스트의 맨 앞부터 특정위치까지의 합을 구해 놓는 것을 의미한다.
- O(n + m)이 된다.

## 예시
1. N개의 수에 접두사 합을 계산하여 배열 P에 저장한다.
2. 매 m개의 쿼리 정보를 확인 할때 구간 합은 p[r]-p[l-1]이다.

```python
n = 5
data = [10, 20, 30, 40, 50]

sum_value = 0
prefix_sum = [0]

for i in data:
    sum_value += 1
    prefix_sum.append(sum_value)

left = 3
right = 4
print(prefix[right] - prefix[left - 1])
```

# 순열과 조합
- 경우의 수란 한 번의 시행에서 일어날 수 있는 사건의 가지 수
- **순열**이란 서로 다른 n개에서 r개를 선택하여 일렬로 나열하는 경우의 수 nPr
- itertools를 사용한다.
- permutations()함수는 리스트로 안나와서 리스트로 묶어야한다.
- **조합**이란 서로 다른 n개에서 순서에 상관없이 서로 다른 r개를 선택 nCr
- combinations()를 사용한다.

```python
## 순열
import itertools

data = [1, 2, 3, 4]
for x in itertools.permutatons(data, 2):
    print(list(x))


## 조합
import itertools

data = [1, 2, 3]

for x in itertools.combinations(data, 2):
    print(list(x), end=' ')
```

# 문제

## 1.소수 구현하기
M이상 N이하의 소수를 구하시오

## 1-1.내가 푼 답

```python
import math

m, n = map(int, input().split())

array = [True] * (n + 1)

for i in range(m, n + 1):
    if array[i] == True:
        j = 2
        while (i * j <= m):
            array[i * j] = False
            j += 1

for i in range(m, n+1):
    if array[i] == True:
        print(i)
    
```

## 1-2.답안 예시

```python
import math

m, n = map(int, input().split())

array = [True for i in range(1000001)]
array[1] = 0

for i in range(2, int(math.sqrt(n)) + 1):
    if array[i] == True:
        j = 2
        while (i * j <= m):
            array[i * j] = False
            j += 1

for i in range(m, n+1):
    if array[i] == True:
        print(i)
    
```

## 1-3.답안 예시와 내가 푼 답의 차이점
답안은 문제의 범위를 확정을 지었고 나는 나온 값으로 했는데 답안이 더 안정적일거같고  
제곱근까지 구하는 것을 생각해야겠다.


## 2.암호 만들기
암호는 서로 다른 L개의 알파벳 소문자들로 구성되며 최소 한개의 모음과 최소 두개의 자음으로 구성되어있다.  
암호는 증가하는 순서로 배열되었으며 암호에 사용했을 문자의 종류는 C가지가 있다.  
c개의 문자가 주어졌을 때 가능성 있는 암호들을 모두 구하라

## 2-1.내가 푼 답(못풀었음)

```python
import itertools
import sys
input = sys.stdin.readline

l, c = map(int, input().split())

array = input().split()
#자음
co = []
#모음
vo = []
for i in array:
    if i == "a" or i == "e" or i == "i" or i == "o" or i == "u":
        vo.append(i)
    else:
        co.append(i)

co.sort()
vo.sort()
```
  
## 2-2.답안 예시

```python
from itertools import combinations

vowels = ('a', 'e', 'i', 'o', 'u')

l, c = map(int, input().split())

array = input().split()
array.sort()

for password in combinations(array, l):
    count = 0
    #모음 개수 확인
    for i in password:
        if i in vowels:
            count += 1

    if count >= 1 and count <= l - 2:
        print(''.join(password))
```

## 2-3.답안 예시와 내가 푼 답의 차이점
답안에서 답들을 그냥 길이가 l인 것들을 모두 구한다음에 모음 개수를 구한다는 생각을 하지 못하고 자음따로 모음 따로 집어넣어서 각각 1개 2개씩 최소로 구하려고만 생각 했더니 안되었던 것 같다.  
확실히 문제들을 많이 풀면서 비슷한 문제들을 풀어야겠다.
