---
layout: post
title:  "[알고리즘] BFS & DFS 알고리즘"
description: >
    "내용 정리"

hide_last_modified: true
---
* TOC
{:toc}
***
## BFS(Breadth-first search)

### 너비 우선 탐색이란?
***
시작 정점을 방문한 후 시작 정점에 인접한 모든 정점들을 우선 방문하는 방법이다. 더 이상 방문하지 않은 정점이 없을 때까지 방문하지 않은 모든 정점들에 대해서 BFS 방식을 적용한다.

List로 구현할 경우 큐  방식을 활용해야만 레벨 순서대로 접근이 가능하다.

- 장점
  - 출발 노드에서 목표 노드까지 최단 길이 경로를 보장한다.

- 단점
  - 경로가 매우 길 경우에는 탐색 가지가 급격히 증가함에 따라 많은 기억 공간을 필요로 하게 된다.

### 구현 방법
***
BFS의 경우 queue를 이용해 구현하게 된다.

1. queue와 방문기록을 남겨줄 visited를 함께 만들어준다.
2. queue에는 우선 루트 노드만을 저장한다.
3. 루프문을 돌면서 큐에서 노드를 꺼내 해당 노드와 연결된 노드들을 검사한다.
4. 만약 연결된 노드가 방문하지 않은 노드라면 결과에 추가해주고 queue에 연결된 노드를 넣어주는 것을 반복한다.
5. 위 3,4번 과정을 반복하면 너비 우선 탐색을 진행한다.

### 구현 코드
***

```

    from collections import deque
    
    def BFS():
        while queue:
            node = queue.popleft()
            result2.append(node)
            for i in range(N+1):
                if arr[node][i] == 1 and visited2[i] == 0:
                    queue.append(i)
                    visited2[i] = 1
    
    # input
    N, M, V = map(int, input().split())
    
    arr = list([0] * (N+1) for _ in range(N+1))
    visited2 = [0] * (N+1)
    for _ in range(M):
        a, b = map(int, input().split())
        arr[a][b] = 1
        arr[b][a] = 1
    
    result2 = []
    visited2[V] = 1
    queue = deque()
    queue.append(V)
    BFS()

```

## DFS(Depth_First Search)

### 깊이 우선 탐색이란?
***
탐색 트리에서 최근에 첨가된 노드를 선택하고, 이 노드에 적용 가능한 다음 노드를 자식 노드로 첨가하며, 첨가된 자식 노드가 목표 노드일 때까지 같은 방식을 반복해 나가는 방식이다.

깊이 제한에 도달한 경우에는 최근에 첨가되었던 부모 노드로 되돌아와서, 다른 적용 가능한 자식 노드를 첨가하며 다시 진행한다.

장점
- 저장공간의 수요가 적다
- 목표 노드가 깊은 단계에 있을 경우 해를 빠르게 구할 수 있다.

단점
- 해가 없는 경로에 들어가도 끝까지 탐색을 하게 된다.
- 얻어진 해가 최단 경로가 된다는 보장은 없다. 목표에 이르는 경로가 다수인 문제에 대해서 DFS는 해를 구하면 탐색을 끝내버리므로, 얻은 해가 최적이 아닐 수도 있다.

### 구현 방법
***
재귀를 이용하여 DFS를 하는 방법을 알아보겠다.

1. 방문기록을 저장해 줄 visited를 만들어준다.
2. 루트 노드부터 탐색을 시작한다.
3. 현재 노드를 결과에 추가한다.
4. 현재 노드와 연결된 노드들을 검사한다.
5. 연결된 노드가 방문하지 않은 노드라면 우선 그 노드로 들어간다.
6. 계속해서 3,4,5번을 반복하다가 더 이상 탐색할 노드가 없다면 트리를 하나씩 올라온다.
7. 만약 다시 올라온 노드에 탐색할 노드가 남아있다면 다시 그 노드를 탐색하러 들어가는 것을 반복한다.

### 구현 코드
***

```

    def DFS(V):
        result.append(V)
        visited[V] = 1
        for i in range(N+1):
            if arr[V][i] == 1 and visited[i] == 0:
                DFS(i)
    
    # input
    N, M, V = map(int, input().split())
    
    arr = list([0] * (N+1) for _ in range(N+1))
    visited = [0] * (N+1)
    for _ in range(M):
        a, b = map(int, input().split())
        arr[a][b] = 1
        arr[b][a] = 1
    
    result = []
    DFS(V)

```