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
last_modified_at: 2022-08-20
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

## 4.자물쇠와 열쇠
자물쇠가 있는데 한칸이 1x1인 NxN크기의 정사각형이고 열쇠는 MxM크기의 정사각형이다
자물쇠에는 홈이 파여있고 열쇠또한 홈과 돌기가 있다.  
열쇠의 돌기부분에 자물쇠가 딱 맞게 채워지면 열리는 방식이다. 
자물쇠의 모든 홈을 채워 비어있는 곳이 없어야 자물쇠를 열 수 있다.  
열쇠를 나타내는 2차원 배열 key와 2차원 배열 lock가 있을 때
열쇠로 자물쇠를 열 수 있으면 true 없으면 false를 return 하도록 한다.

## 4-1.내가 푼 답

```python

```
  
## 4-2.다른 풀이

```python
#회전
def rotate(k):
    n=len(k)
    result=[[0]*n for _ in range(n)]
    for i in range(n):
        for j in range(n):
            result[j][n-i-1]=k[i][j]
    return result
    
#확인
def check(l):
    n=len(l)//3
    #new_lock의 중간 부분이 모두 1인지 확인
    for i in range(n,n*2):
        for j in range(n,n*2):
            if l[i][j]!=1:
                return False
    return True
                

def solution(key, lock):
    answer = False
    n=len(lock)
    m=len(key)
    
    new_lock = [[0]*(n*3) for _ in range(n*3)]
    #세배 확장된 lock을 만든다.
    for i in range(n):
        for j in range(n):
            new_lock[i+n][j+n]=lock[i][j]
            
    #매 회전
    for r in range(4):
        key=rotate(key)
        #매 이동
        for x in range(1,n*2):
            for y in range(1,n*2):
            
            	#열쇠를 넣어준다.
                for i in range(m):
                    for j in range(m):
                    	#lock에 key를 더해줌
                        new_lock[x+i][y+j]+=key[i][j]
                
                #해당 열쇠가 맞는지 검사
                if check(new_lock)==True:
                    return True
                    
                #맞지 않는다면 되돌린다(열쇠를 뺀다)
                for i in range(m):
                    for j in range(m):
                        new_lock[x+i][y+j]-=key[i][j]
    
    return False
```

## 4-3.고찰
자물쇠를 3배로 확장한다음에 열쇠를 회전하고 움직이면서 거기에 맞는 부분이 있는지 확인하는 완전 탐색으로 푸는 것 같다.

## 5.뱀
뱀이 사과를 먹으면 뱀 길이가 늘어나는데 벽이나 자기자신의 몸과 부딫히면 게임이 끝난다.  
게임은 NxN위에서 하고 뱀이 길이는 처음에 1이다. 뱀은 맨위좌측부터 시작한다.  
뱀은 다음과 같이 이동한다.
1. 뱀은 몸길이늘 늘려 머리를 다음칸에 이동한다
2. 만약 사과가 있다면 꼬리는 그대로다.
3. 사과가 없다면 몸길이를 줄인다.


## 5-1.내가 푼 답(못 풀었음)

```python
n = int(input())

map_list = [[0] * n for _ in range(n)]

apple = int(input())

for _ in range(apple):
    x, y = map(int, input().split())
    map_list[x][y] == 1

moving_snake = int(input())
move_list = []

for _ in range(moving_snake):
    t, dir = input().split()
    move_list.append((t, dir))

snake = [[0, 1]]

dir = ([0, 1], [1, 0], [0, -1], [-1, 0])

cur_dir = dir[0]

for moving in move_list:
    t = int(moving[0])
    for i in t:
        head = snake[-1]
        head[0] = head[0] + cur_dir[0]
        head[1] = head[1] + cur_dir[1]
        if map_list[head[0]][head[1]] == 0:
            snake.pop()
        snake.append(head)
    dir = moving[1]

```
  
## 5-2.다른 풀이

```python
from collections import deque

n = int(input())
k = int(input())

graph = [[0] * n for _ in range(n)]
dx = [0, 1, 0, -1]
dy = [1, 0, -1, 0]

for i in range(k):
    a, b = map(int, input().split())
    graph[a - 1][b - 1] = 2

l = int(input())
dirDict = dict()
queue = deque()
queue.append((0, 0))

for i in range(l):
    x, c = input().split()
    dirDict[int(x)] = c

x, y = 0, 0
graph[x][y] = 1
cnt = 0
direction = 0

def turn(alpha):
    global direction
    if alpha == 'L':
        direction = (direction - 1) % 4
    else:
        direction = (direction + 1) % 4


while True:
    cnt += 1
    x += dx[direction]
    y += dy[direction]

    if x < 0 or x >= n or y < 0 or y >= n:
        break

    if graph[x][y] == 2:
        graph[x][y] = 1
        queue.append((x, y))
        if cnt in dirDict:
            turn(dirDict[cnt])

    elif graph[x][y] == 0:
        graph[x][y] = 1
        queue.append((x, y))
        tx, ty = queue.popleft()
        graph[tx][ty] = 0
        if cnt in dirDict:
            turn(dirDict[cnt])

    else:
        break

print(cnt)
```

## 5-3.고찰
뱀을 큐에 넣는 것이 핵심이다. 그리고 내가 푼 답은 뱀을 따로 리스트에 넣었는데 맵에 1로 표현 하는 것이 더 나은 것 같다.

## 6.기둥과 보 설치
2차원 가상 벽면에 기둥과 보를 설치한다.
1. 기둥은 바닥위에 있거나 보의 한쪽 끝부분 위에 있거나 다른 기둥 위에 있어야 한다.
2. 보는 한쪽 끝부분이 기둥 위에 있거나 또는 양쪽 끝부분이 다른 보와 동시에 연결되어있어야한다.

## 6-1.내가 푼 답

```python

```
  
## 6-2.다른 풀이

```python
# 파이썬
def impossible(result):
    COL, ROW = 0, 1
    for x, y, a in result:
        if a == COL: # 기둥일 때
            if y != 0 and (x, y-1, COL) not in result and \
        (x-1, y, ROW) not in result and (x, y, ROW) not in result:
                return True
        else: # 보일 때
            if (x, y-1, COL) not in result and (x+1, y-1, COL) not in result and \
        not ((x-1, y, ROW) in result and (x+1, y, ROW) in result):
                return True
    return False

def solution(n, build_frame):
    result = set()
    
    for x, y, a, build in build_frame:
        item = (x, y, a)
        if build: # 추가일 때
            result.add(item)
            if impossible(result):
                result.remove(item)
        elif item in result: # 삭제할 때
            result.remove(item)
            if impossible(result):
                result.add(item)
    answer = map(list, result)
    
    return sorted(answer, key = lambda x : (x[0], x[1], x[2]))
```

## 4-3.고찰

## 7.치킨 배달
크기가 NxN인 도시가 있다. 도시의 칸은 (r, c)같은 형태이다.
0은 빈칸 1은 집 2는 치킨집이라고 할때 치킨집은 최대 m개까지 고를 때 도시의 치킨거리가 가장 적을지 구하라

## 7-1.내가 푼 답

```python
from itertools import combinations

n, m = map(int, input().split())
maps=[]
for i in range(n):
  maps.append(list(map(int, input().split())))

home=[]
chicken=[]
for i in range(n):
  for j in range(n):
    if maps[i][j]==1:
      home.append((i,j))
    elif maps[i][j]==2:
      chicken.append((i,j))

chicken_combinations = list(combinations(chicken,m))
result=[0]*len(chicken_combinations)

for i in home:
  for j in range(len(chicken_combinations)):
    a=100
    for k in chicken_combinations[j]:
      tmp = abs(i[0]-k[0])+abs(i[1]-k[1])
      a=min(a, tmp)
    result[j]+=a

print(min(result))
```
  
## 7-2.다른 풀이

```python

```

## 7-3.고찰


## 8.외벽 점검
내부 공사를 하는데 레스토랑의 구조는 완벽한 원 모양이고 총 n미터 이다.  
내부 공사 도중에 점검을 확인하기 위해서 주기적으로 점검을 해야한다. 단 1시간으로 제한한다.  
편의상 레스토랑의 정북을 0으로 하고 시계방향으로 거리를 나타낸다.  
외벽의 길이를 n, 취약 지점의 위치가 담긴 배열 weak, 각 친구가 1시간 동안 이동할 수 있는 거리가 dist일때 친구 수의 최솟값을 구하라


## 8-1.내가 푼 답

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
  
## 8-2.다른 풀이

```python
from itertools import permutations


def solution(n, weak, dist):
    # 길이를 2배로 늘려서 원형 -> 일자 형태
    length = len(weak)
    for i in range(length):
        weak.append(weak[i] + n)

    answer = len(dist) + 1
    # 0 부터 length-1 까지의 위치를 각각 시작점으로 설정
    for start in range(length):
        # 친구를 나열하는 모든 경우의 수 각각에 대하여 확인
        for friends in list(permutations(dist, len(dist))):
            count = 1 # 투입할 친구의 수
            # 해당 친구가 점검할 수 있는 마지막 위치
            position = weak[start] + friends[count-1]
            # 시작점부터 모든 취약 지점을 확인
            for index in range(start, start+length):
                # 점검할 수 있는 위치를 벗어나는 경우
                if position < weak[index]:
                    count += 1 # 새로운 친구를 투입
                    if count > len(dist): # 더 투입이 불가능하다면 종료
                        break
                    position = weak[index] + friends[count-1]
            answer = min(answer, count) # 최솟값 계산
        if answer > len(dist):
            return -1
    return answer
```

## 8-3.고찰
컴비네이션이 자주 쓰이는 것을 알게 되었고 구현 부분의 문제들을 거의 혼자서 풀지 못했다.  
좀더 공부좀 해야겠다.