---
title: 2021-10-23 BFS/DFS
tags: [Python,Algorithm,BFS]
categories: Algorithm
---
# 문제 설명 

오늘은 [백준 2606 바이러스](https://www.acmicpc.net/problem/2606)문제를 풀어 보도록 하겠습니다. 


## 문제 


신종 바이러스인 웜 바이러스는 네트워크를 통해 전파된다. 한 컴퓨터가 웜 바이러스에 걸리면 그 컴퓨터와 네트워크 상에서 연결되어 있는 모든 컴퓨터는 웜 바이러스에 걸리게 된다.

예를 들어 7대의 컴퓨터가 <그림 1>과 같이 네트워크 상에서 연결되어 있다고 하자. 1번 컴퓨터가 웜 바이러스에 걸리면 웜 바이러스는 2번과 5번 컴퓨터를 거쳐 3번과 6번 컴퓨터까지 전파되어 2, 3, 5, 6 네 대의 컴퓨터는 웜 바이러스에 걸리게 된다. 하지만 4번과 7번 컴퓨터는 1번 컴퓨터와 네트워크상에서 연결되어 있지 않기 때문에 영향을 받지 않는다.



어느 날 1번 컴퓨터가 웜 바이러스에 걸렸다. 컴퓨터의 수와 네트워크 상에서 서로 연결되어 있는 정보가 주어질 때, 1번 컴퓨터를 통해 웜 바이러스에 걸리게 되는 컴퓨터의 수를 출력하는 프로그램을 작성하시오.

- 이번 문제도 DFS 또는 BFS로 탐색하라는 문제인 것 같네요? 

### BFS (Breadth-First search)
- 시작점과 **가까운 지점의 데이터**들을 우선적으로 검사하는 방법입니다. 최단거리(경로)를 찾아줍니다. 
- queue와의 관계 
    - 큐는 먼저들어온걸 먼저 처리 합니다. 
    - 시작노드를 큐에 삽입합니다. 
    - 큐에서 노드를 꺼내서 연결된 (가까운)위치의 노드를 찾아서 큐에 넣어줍니다. 
    - 그리고 stack의 앞에 있는 노드를 다시 꺼내서 위의 과정을 반복 해요. 
    - 그러면 첫번째 삽입했던 노드와 가장 가까운것 부터 검사를 하고 가장 먼 것을 마지막에 처리를 하게 됩니다. 
    - 그러니까 가족 관계도를 BFS로 읽는다고 하면 가장 위에 있는 조상 부터 시작해서 한층 한층 씩 내려가며 왼쪽 (또는 오른쪽) 방향으로 읽는것이랑 같은거에요. 


## 입력
첫째 줄에는 컴퓨터의 수가 주어진다. 컴퓨터의 수는 100 이하이고 각 컴퓨터에는 1번 부터 차례대로 번호가 매겨진다. 

둘째 줄에는 네트워크 상에서 직접 연결되어 있는 컴퓨터 쌍의 수가 주어진다. 

이어서 그 수만큼 한 줄에 한 쌍씩 네트워크 상에서 직접 연결되어 있는 컴퓨터의 번호 쌍이 주어진다.

### 입력 설명 
예제는 아래와 같습니다. 컴퓨터는 7대 가 있고, 컴퓨터끼리 연결은 6개가 있어요. 

1번 컴퓨터는 2번, 5번과 연결 되어있고 
2번 컴퓨터는 3번 과 연결, 
5번 컴퓨터는 2번과 6번으로 연결 되어있네요.

그리고 4번과 7번이 연결되어있는데 얘들은 1번과 어떤 연결도 없으니까 무시하면 되겠어요. 


7

6

1 2

2 3

1 5

5 2

5 6

4 7


## 출력
1번 컴퓨터가 웜 바이러스에 걸렸을 때, 1번 컴퓨터를 통해 웜 바이러스에 걸리게 되는 컴퓨터의 수를 첫째 줄에 출력한다.
### 출력 설명
1번 컴퓨터가 바이러스에 걸리면 2번,5번,3번,6번이 바이러스에 걸려요. 그러니까 출력은 4가 됩니다. 

# 코드 

- 일단 입력을 연결된 컴퓨터쌍으로 받으니까 dict를 만들어서 컴퓨터를 담아봅니다. 
    - 각 줄의 1번 컴퓨터를 Key, 2번을 value 
    - 2번 컴퓨터를 key, 1번을 value로 담을래요. 

- 그리고 1번 컴퓨터를 제일 먼저 소환 한 다음, 
1번과 바로 연결된 컴퓨터들을 리스트에 담아줄거에요. 그리고 연결된 컴퓨터들을 바이러스 걸린 컴퓨터 리스트에 담아요. 

- 1과 연결된 컴퓨터들을 순서대로 하나씩 까봅니다. 여기에 연결된 컴퓨터가 있는지 확인하고, 해당 컴퓨터를 확인할 컴퓨터 리스트에 담아줍시다. 
 - 그러니까 현재 확인할 리스트에 가장 앞에 있는 컴퓨터를 바이러스 걸린 컴퓨터 리스트에 이미 존재하는지 확인하고, 존재 하지 않는다면 바이러스 걸린 컴퓨터 리스트에 담고요. 
- 그 컴퓨터와 연결된 컴퓨터들을 확인할 리스트의 맨 뒤에 추가 해줄거에요.
- 확인할 리스트의 끝에 있는 컴퓨터와 연결된 컴퓨터가 더이상 없다면 확인 작업을 종료 합니다. 
- 그리고 바이러스 걸린 컴퓨터 리스트의 길이를 카운트해서 출력하겠습니다. 

    ```python
    import sys
    from collections import deque

    N = int(input())
    Links = int(input())

    nodes = {}
    for i in range(Links):
        k,v = map(int, sys.stdin.readline().split())
        try: nodes[k].append(v)
        except: nodes[k] = [v]

        if v not in nodes:
            nodes[v]= [k]
        else:
            if k not in nodes[v]:
                nodes[v].append(k)

    bfs = deque([1])
    b_visited = []
    while bfs:
        node = bfs.popleft()
        if node not in b_visited:
            b_visited.append(node)
            try:
                bfs.extend(nodes[node])
            except:
                pass

    print(len(b_visited)-1)

    ```