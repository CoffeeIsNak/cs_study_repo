# DFS & BFS

- 그래프 알고리즘으로, 문제를 풀 때 상당히 많이 사용된다고 함. => 경로를 찾는 문제 시, 상황에 맞게 DFS와 BFS를 활용한다고 함.


### 1. DFS(Depth-First-Search)

- 루트 노드 혹은 임의 노드에서 다음 브랜치로 넘어가기 전에, 해당 브랜치를 모두 탐색하는 방법 => **스택 or 재귀함수** 를 통해 구현한다.

- 모든 경로를 방문해야 할 경우 사용에 적합하다고 함.

- 시간 복잡도
    - 인접 행렬 : O(V^2)
    - 인접 리스트 : O(V+E)
    - V(ertex), E(dge)


- 한 경로를 끝까지 탐색한 후 백트래킹(되돌아감)하며, 그래프 탐색, 경로 찾기, 백트래킹 문제에서 자주 사용됨

- 예제 코드

```python
def dfs_recursive(graph, node, visited):
    """재귀 함수 이용하는 dfs"""
    # 현재 노드 방문 처리(set)
    visited.add(node)

    # 연결된 다른 노드 재귀적 방문
    for neighbor in graph[node]:
        if neighbor not in visited:
            dfs_recursive(graph, node, visited)

# 그래프 (인접 리스트)
graph = {
    1: [2, 3],
    2: [4, 5],
    3: [6],
    4: [],
    5: [],
    6: []
}

# 방문 기록 (set 사용)
visited = set()

# DFS 실행
print("DFS 탐색 순서:", end=" ")
dfs_recursive(graph, 1, visited)  # 시작 노드: 1


#############################################################################


def dfs_stack(graph, start):
    """stack을 이용하는 dfs"""
    visited = set()  # 방문 노드를 저장, 체크할 예정
    stack = [start]  # 앞으로 방문할 노드를 보관

    while stack:
        node = stack.pop()  # 노드를 방문
        
        if node not in visited:
            visited.add(node)

            # 인접 노드 스택에 추가, reversed는 노드 순서를 보장해주기 위함(스택이라서 거꾸로가 됨)
            for neighbor in reversed(graph[start]):
                if neighbor not in visited:
                    stack.append(neighbor)

# DFS 실행
print("\nDFS 탐색 순서 (스택 방식):", end=" ")
dfs_stack(graph, 1)  # 시작 노드: 1
```






### 2. BFS(Breadth-First-Search)

- 루트 노드 또는 임의 노드에서 인접한 노드부터 먼저 탐색하는 방법 => 보통 큐를 통해 선입 선출로 구현한다. (해당 노드의 주변부터 탐색해야하기 때문)

- 최소 비용(즉, 모든 곳을 탐색하는 것보다 최소 비용이 우선일 때)에 적합

- 시간 복잡도
    - 인접 행렬 : O(V^2)
    - 인접 리스트 : O(V+E)

- DFS가 한 경로를 끝까지 탐색한 후 백트래킹하는 방식이라면, BFS는 모든 노드를 같은 깊이(depth)에서 탐색한 후 다음 깊이로 이동하는 방식이야.

- 예제 코드

```python
from collections import deque

def bfs(graph, start):
    visited = set()  # 방문한 노드 저장
    queue = deque([start])  # BFS는 큐(Queue)를 사용
    visited.add(start)

    while queue:
        node = queue.popleft()  # 큐에서 노드를 꺼냄
        print(node, end=" ")  # 방문한 노드 출력

        # 현재 노드와 연결된 노드들을 방문
        for neighbor in graph[node]:
            if neighbor not in visited:
                queue.append(neighbor)
                visited.add(neighbor)  # 방문 처리

# BFS 실행
print("BFS 탐색 순서:", end=" ")
bfs(graph, 1)  # 시작 노드: 1
```

- DFS처럼 백트래킹이 필요 없음!

### 3. BFS vs DFS 비교

- BFS (너비 우선 탐색) / DFS (깊이 우선 탐색)
    - 탐색 방식: 가까운 노드부터 탐색 / 한 경로를 끝까지 탐색
    - 구현 방식: 큐(Queue) / 재귀(Recursion) 또는 스택(Stack)
    - 경로 탐색: 최단 거리 문제에 적합 / 백트래킹, 특정 경로 찾기에 적합
    - 사용 예시: 최단 거리 문제, 미로 찾기 / 백트래킹, 경로 찾기, 사이클 탐지
