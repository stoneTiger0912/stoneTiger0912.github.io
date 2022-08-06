---
title:  "[Algorithm] 알고리즘 정리(2)- 구현 알고리즘"
excerpt: "코딩 테스트 대비 알고리즘을 정리 하는 글"

categories:
  - Algorithm
tags:
  - [Blog, algorithm, python, implementation_algorithm]

toc: true
toc_sticky: true
 
date: 2022-08-05
last_modified_at: 2022-08-05
---

# 구현 알고리즘
- 알고리즘은 간단하지만 구현하기 쉽지 않은 문제들을 구현 알고리즘이라 한다.
- 완전 탐색: 모든 경우의 수를 다 계산하는 방법
- 시뮬레이션: 문제에서 제시한 알고리즘을 한 단계씩 차례로 직접 수행 해야하는 문제
- 파이썬의 경우 데이터 처리량이 많을 때는 메모리 제한을 고려해야한다.
- 1000만 이상의 리스트의 경우 매모리 제한으로 약 40MB의 용량을 차지한다.


## 구현 알고리즘의 예시
시작 좌표 (1, 1)에서 R, U, L, D 중 하나의 문자가 반복하여 적혀있을 경우 
도착 좌표를 출력하라.

이 알고리즘의 시간 복잡도는 O(N)이다.  
이런 문제들이 구현 알고리즘이다.

# 문제

## 1.시각
정수 N이 입력되면 0시 0분 0초 부터 N시 59분 59초까지의 모든 시각중에 3이 하나라도 포함되는 경우의 수를 구하라.

## 1-1.내가 푼 답
```python
n = int(input())
count = 0
for i in range(n+1):
    for j in range(60):
        for k in range(60):
            sec = str(k)
            min = str(j)
            hour = str(i)
            if sec.find('3') != -1 or min.find('3') != -1 or hour.find('3') != -1:
                count += 1

print(count)

```

## 1-2.답안 예시
```python
h = int(input())

count = 0
for i in range(h+1):
    for j in range(60):
        for k in range(60):
            if '3' in str(i) + str(j) + str(k):
                count += 1
print(count)
```

## 1-3.답안 예시와 내가 푼 답의 차이점
내가 푼 답의 경우 시, 분, 초를 각각 따로따로 해서 파이썬 라이브러리인 find()를 썻는데  
답안은 그냥 셋 다 str로 합친다음에 3이 있다는 것을 in으로 찾았다. 답안이 더 깔끔하고 쉽게 했다.

## 2.왕실의 나이트
체스판이 8x8형태로 있고 나이트는 L형태로 움직을 수 있다.  
나이트의 좌표가 주어졌을 때 나이트가 이동 할 수 있는 경우의 수를 구하라.

## 2-1.내가 푼 답
```python
point = input()
point_x = int(ord(point[0])) - int(ord('a')) + 1
point_y  = int(point[1])
#상 하 좌 우
x = [-2, -2, 2, 2, -1, 1, -1, 1]
y = [-1, 1, -1, 1, -2, -2, 2, 2]
#8방향
count = 0
for i in range(8):
    move_x = point_x + x[i]
    move_y = point_y + y[i]
    if move_x > 0 and move_x <= 8 and move_y > 0 and move_y <= 8:
        count += 1
    
print(count)

```
## 2-2.답안 예시
```python
input_data = input()
row = int(input_data[1])
column = int(ord(input_data[0])) - int(ord('a')) + 1

steps = [(-2, 1), (-1, -2), (1, -2), (2, -1), (2, 1), (1, 2), (-1, 2), (-2, 1)]

result = 0
for step in steps:
    next_row = row + step[0]
    next_column = column + step[1]

    if next_row >= 1 and next_row <= 8 and next_column >= 1 and next_column <= 8:
        result += 1
print(result)
```

## 2-3.답안 예시와 내가 푼 답의 차이점
이동할 위치의 좌표를 나는 x좌표 y좌표 따로따로 구해서 했는데 답안의 steps같이 x, y좌표를 같이 묶어서 하는게  
덜 복잡하고 한눈에 알 수 있다.

## 3.게임 개발
캐릭터가 (1, 1)에 있고 NxM으로 이루어진 맵에서 
- 현재 위치의 왼쪽부터 차례로 이동
- 왼쪽 방향이 가보지 않은 칸이면 왼쪽으로 회전후 왼쪽칸으로 전진, 만약 왼쪽방향으로 갈 수 없으면 왼쪽으로 회전
- 만약 4칸이 모두 가본 칸이나 바다로 이루어져있으면 바라보는 방향을 유지하고 갈 수 없으면 움직임을 멈춘다.
- 첫째줄에 n, m 둘째줄에 캐릭위치(a, b) 바라보는 방향 d 육지 바다 설정
## 3-1.내가 푼 답(틀림)
```python
n, m = map(int, input().split())
A, B, d = mpa(int, input().split())
map_list = [list(map(int, input().split())) for _ in range(4)]

#북 동 남 서
dir = [(-1, 0), (0, 1), (1, 0), (0, -1)]

result = 0
while True:
    next_dir = d - 1
    column = A + dir[next_dir][0]
    row = B + dir[next_dir][1]
    if map_list[column][row] == 0:
        A = column
        B = row
        result += 1
    else:
        next_dir -= 1
        if map_list[column]
        count = 0
        for i in range(4):
            if map_list[A + dir[i][0]][B + dir[i][1]] == 1:
                count += 1
        if count == 4:
            column = A + dir[next_dir - 2][0]
            row = B + dir[next_dir - 2][1]
            if map_list[column][row] == 1:
                break
print(result)
```
## 3-2.답안 예시
```python
n, k = map(int, input().split())

d = [[0] * m for _ in range(n)]
x, y, dir = map(int, input().split())
d[x][y] = 1

array = []
for i in range(n):
    array.append(list(map(int, input().split())))

dx = [-1, 0, 1, 0]
dy = [0, 1, 0, -1]

def turn_left():
    global direction
    direction -= 1
    if direction == -1:
        direction = 3

count = 1
turn_time = 0

while True:
    turn_left()
    nx = x + dx[direction]
    ny = y + dy[direction]

    #회전이후 정면에 가보지 않은 칸 존재
    if d[nx][ny] == 0 and array[nx][ny] == 0:
        d[nx][ny] = 1
        x = nx
        y = ny
        count += 1
        turn_time = 0
        continue
    #회전이후 칸이 없거나 바닥
    else:
        turn_time += 1
    
    #네면이 바다
    if turn_time == 4:
        nx = x - dx[direction]
        ny = y - dy[direction]
        #뒤로 이동가능하면
        if array[nx][ny] == 0:
            x = nx
            y = ny
        else:
            break
        turn_time = 0

print(count)
```

## 3-3.답안 예시와 내가 푼 답의 차이점
내가 풀었던 답은 너무 복잡하고 실제로는 답이 안나왔지만 구현 문제가 확실히 천천히 문제를 읽으면서 하면  
풀린다는 것을 알게 되었다.