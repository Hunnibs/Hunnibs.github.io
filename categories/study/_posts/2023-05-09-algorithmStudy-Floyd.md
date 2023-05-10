---
layout: post
title:  "[알고리즘] Floyd-Warshall 알고리즘"
description: >
    "내용 정리"

hide_last_modified: true
---
* TOC
{:toc}
***
### 플로이드-워셜 알고리즘이란?
***
플로이드-워셜 알고리즘(Floyd-Warshall Algorithm)은 변의 가중치가 음이거나 양인 (음수 사이클은 없는) 가중 그래프에서 최단 경로들을 찾는 알고리즘이다. All-to-all shortest path라고도 표현하며 해당 알고리즘은 DP 알고리즘을 활용해야한다.   
목적은 모든 노드 간의 최단 경로의 길이를 찾는 것이다.      

**Wikipedia 예시**   
![graph](\assets\img\study\Floyd_Wiki.png)   

아래는 dist 리스트의 변화를 K 값에 따라 보여준다.(K값은 반드시 지나야하는 경로를 뜻함)   
![table](\assets\img\study\Floyd_WikiG.png)   

### 구현 방법
***
- 플로이드-워셜 알고리즘도 다익스트라 알고리즘과 같이 **가중치가 주어진 방향 그래프**에만 사용이 가능
- 시간 복잡도는 복잡하지만 모든 정점들 사이의 최단경로를 구하는 점과 음수 가중치에 대해서도(음수 사이클은 존재하지 않아야한다.) 처리할 수가 있다.

- 단계별 정리
    1. 경유하는 노드가 없을 때 최단거리를 저장해준다.(이 때, 서로 연결된 간선이 없는 노드 간은 inf로 저장한다)
    2. 경유하는 노드를 1부터 n번 노드까지 정해준다.
    3. 각 노드 별로 경유하는 노드를 지나 다른 노드로 갈 때의 거리와 원래 다른 노드로 갈 때의 거리를 비교한다.
    4. 만약, 경유해서 가는 거리가 더 짧다면 최단거리를 업데이트 한다.

### 구현 코드
***
[백준 11404 플로이드 문제](https://www.acmicpc.net/problem/11404)

```

    import sys
    from math import inf

    # input
    input = sys.stdin.readline

    n = int(input())
    m = int(input())

    graph = [[inf for _ in range(n)] for _ in range(n)]
    for _ in range(m):
        a, b, c = map(int, input().split())
        graph[a-1][b-1] = min(graph[a-1][b-1], c)

    # main
    for K in range(n):
        for i in range(n):
            for j in range(n):
                graph[i][j] = min(graph[i][j], graph[i][K] + graph[K][j])

    for i in range(n):
        graph[i][i] = 0

    for i in range(n):
        for j in range(n):
            if graph[i][j] == inf:
                print(0, end=' ')
            else:
                print(graph[i][j], end=' ')
        print()

```

- 실제 Floyd-Warshall 코드는 # main 아래 5줄이 전부이다. 
- 해당 문제에서는 시작도시와 도착도시가 같은 경우는 없어야하므로 0으로 초기화해줬다.

### 참조
***
[신찬수 교수님 유튜브 강의영상](https://www.youtube.com/watch?v=bKa7bfXQi3A&list=PLsMufJgu5932XYejsOwcUDJ2F75f56nrl&index=52&t=1163s)   
[Wikipedia 플로이드-워셜 알고리즘](https://ko.wikipedia.org/wiki/%ED%94%8C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EC%9B%8C%EC%85%9C_%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)