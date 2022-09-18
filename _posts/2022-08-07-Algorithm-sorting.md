---
title:  "[Algorithm] 알고리즘 정리(4)- 정렬 알고리즘"
excerpt: "코딩 테스트 대비 알고리즘을 정리 하는 글"

categories:
  - algorithm
tags:
  - [Blog, algorithm, python, sorting_algorithm]

toc: true
toc_sticky: true
 
date: 2022-08-07
last_modified_at: 2022-08-07
---

# 정렬 알고리즘
- 데이터를 특정한 기준에 따라 순서대로 나열
- 선택 정렬: 가장 작은 수를 맨앞으로 스왑하여 정렬
- 삽입 정렬: 첫번째 부터 순서대로 어디에 정렬할지 결정 후 삽입, 한 칸씩 옆으로 이동
- 퀵 정렬: 기준 데이터를 설정하고 그 기준보다 큰데이터와 작은 데이터의 위치 변경
- 계수 정렬: 특정 조건이 부합될때만 사용, 별도의 리스트를 선언후 리스트의 크기는 숫자의 범위로 한다음에 그 숫자가 몇개 있는지 카운트 하는 정렬, 범위가 적을수록 좋음 

 
# 문제

## 1.위에서 아래로
숫자 개수 N을 입력받고 정렬하라

## 1-1.내가 푼 답

```python
n = int(input())
array = []
for _ in range(n):
    array.append(int(input()))

array = sorted(array)

for i in range(array):
    print(i, end=' ')
```

## 1-2.답안 예시
```python
n = int(input())
array = []
for _ in range(n):
    array.append(int(input()))

array = sorted(array)

for i in range(array):
    print(i, end=' ')
```

## 1-3.답안 예시와 내가 푼 답의 차이점
파이썬이여서 쉽게 라이브러리를 사용하여 구했다.

## 2.성적이 낮은 순서로 출력
N명의 학생 정보가 주어졌을 때 성적이 낮은 순으로 학생의 이름을 출력

## 2-1.내가 푼 답

```python
n = int(input())
array = []

for i in range(n):
    name, score = input().split()
    score = int(score)
    array.append((name, score))

array.sort(key=lambda x : x[1])
for i in range(array):
    print(i[0], end=' ')
```

## 2-2.답안 예시

```python
n = int(input())

array = []
for i in range(n):
    input_data = input().split()

    array.append((input_data[0], int(input_data[1])))

array = sorted(array, key=lambda student: student[1])

for student in array:
    print(student[0], end=' ')

```

## 2-3.답안 예시와 내가 푼 답의 차이점
sorted와 sort에서 key로 정렬을 할 수 있다는 점에서 둘다 써도 될 것 같다.
lambda 좀 정리 해야겠다.  
