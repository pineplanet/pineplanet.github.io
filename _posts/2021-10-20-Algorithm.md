---
title: 2021-10-20 BFS/DFS
tags: [Python,Algorithm,BFS,DFS]
categories: Algorithm
---

# 문제 
[백준 1260](https://www.acmicpc.net/problem/1260)

문제 이해 하는데만 한참 걸렸습니다....ㅠ
# 문제 설명 
## 문제 
> 그래프를 DFS로 탐색한 결과와 BFS로 탐색한 결과를 출력하는 프로그램을 작성하시오. 단, 방문할 수 있는 정점이 여러 개인 경우에는 정점 번호가 작은 것을 먼저 방문하고, 더 이상 방문할 수 있는 점이 없는 경우 종료한다. 정점 번호는 1번부터 N번까지이다.


여러 숫자가 던져졌을 때 DFS 로 탐색했을 때 각각 탐색 순서대로 출력하라는건데, 입력을 이해를 못하겠었어요. 

## 입력 
> - 첫째 줄에 **정점의 개수 N**(1 ≤ N ≤ 1,000), **간선의 개수 M**(1 ≤ M ≤ 10,000), 탐색을 **시작할 정점의 번호 V**가 주어진다.
> - 다음 M개의 줄에는 간선이 **연결하는 두 정점**의 번호가 주어진다. 
어떤 두 정점 사이에 여러 개의 간선이 있을 수 있다. 입력으로 주어지는 간선은 양방향이다.

- 예제 1

    4 5 1

    1 2

    1 3

    1 4

    2 4

    3 4

    - 정점이 4개 있고, 간선은 5개, 시작 정점은 1이래요. 
    - 1번은 2,3,4 와 연결되어있고,
    - 2번은 4번과 연결되있고 
    - 3번은 4번과 연결 되어있나봐요 


## 출력 
> DFS: 1 2 4 3

> BFS: 1 2 3 4



# 문제 풀이 
## DFS 
먼저 DFS는 자식이 없을 때 까지 일단 내려가면서 자식을 가장 위에 쌓고, 위에 쌓인 걸 다시 꺼내서 연결된 자식 이 있는지 확인하고 자식 먼저 쌓는것을 반복 합니다. 

그런데 여기서 정점 번호가 낮은것부터 방문한다고 하네요.
그러면 **1번**부터 시작해보아요. 
1번의 자식은 2,3,4 가 있습니다. 그중에서 **2번**을 제일 먼저 방문 하고요. 그 밑에 자식이 있는지 봅니다. 
**4번**이 있네요. 이제 4번 아래 자식이 있는지 봤더니 더 추가할 자식이 없어요. 

1번의 자식들 중 2번과 4번은 모두 방문 했으니, 남아있는 것은 **3번** 입니다. 

그러니까 [1,2,4,3]의 순으로 방문 해요. 

## BFS
BFS는 시작점과 가장 가까운 순으로 방문 합니다. 
1번은 아래 2,3,4가 있고요. 그러니까 1번을 기준으로 2,3,4 가 같은 거리에 있어요. 그런데 작은 순으로 먼저 검사 한다고 했으니, 2,3,4의 차례대로 방문 하겠네요. 만약에 2번의 자식이 이미 방문 한 4가 아니라 다른 숫자였다면 4번의 다음에 검색했겠군요. 

## 코드 

양방향인걸 까먹어서 두번 틀렸습니다. 아래는 성공한 코드 입니다. 

```python
from collections import deque
import sys

input = sys.stdin.readline()

# 첫 줄
N,M,V = map(int,input.split())
nodes = {}
# 정점의 연결
for i in range(M):
    # 정점1과 연결된 자식1
    k, v = map(int, sys.stdin.readline().split())
    try: nodes[k].append(v)
    except : nodes[k] = [v]

    if v not in nodes:
        nodes[v] = [k]
    else:
        if k not in nodes[v]:
            nodes[v].append(k)

for key in nodes:
    nodes[key].sort()



#dfs
dfs = [V]
d_visited = []
while dfs:
    # 첫 노드 꺼냄
    node = dfs.pop()
    if node not in d_visited:
        d_visited.append(node)
        try:
            dfs.extend(sorted(nodes[node],reverse=True))
        except:
            pass

#bfs
bfs = deque([V])
b_visited = []
while bfs:
    node = bfs.popleft()
    if node not in b_visited:
        b_visited.append(node)
        try:
            bfs.extend(nodes[node])
        except:
            pass

##convert str
print(' '.join(map(str,d_visited)))
print(' '.join(map(str,b_visited)))
#print(b_visited)
```
 

