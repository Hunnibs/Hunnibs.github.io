---
layout: post
title:  "[알고리즘] Dijkstra 알고리즘"
description: >
    "내용 정리"

hide_last_modified: true
---
* TOC
{:toc}
***
### 다익스트라 알고리즘이란?
***
다익스트라 알고리즘은 그래프에서 두 정점 간의 최단 경로를 찾는 알고리즘이다. 간선에 가중치가 주어진 방향 그래프를 대상으로만 사용할 수 있다는 특징이 있다.   

![graph](/assets/img/study/graph.png)   

이 알고리즘은 최단 경로를 구하는 과정에서 최단 경로 트리가 만들어지는데 최단 경로 트리는 출발점으로 정한 정점에서 각 정점까지의 최단 경로를 저장해 놓은 트리를 뜻한다.   

예를 들어, 위 그림에서 a에서 i까지의 최단경로를 구한다면 다익스트라 알고리즘은 a에서 정점 g 혹은 h까지의 최단거리를 구한 후 다시 i까지를 계산한다.   
식으로 정리하면, 시작 정점에서 x 정점까지의 최단경로를 dist[x], 간선에 가중치를 x->y라 표현   

> dist[i] = min(dist[g] + g->i, dist[h] + h->i)

즉, i까지의 최단경로를 구하려면 저 과정을 거쳐 각 정점마다 최단경로를 구하며 찾아간다. 그래서 우리는 i까지 가는 경로 내에 있는 정점들의 최단경로를 구할 수 있는 것이다.

### 구현 방법
***
- 다익스트라 알고리즘은 우선 **가중치가 주어진 방향 그래프**에만 사용이 가능
- 우선순위 큐를 이용해 최소가 되는 것부터 지워나가는 것이 중요하다.
- 만약 정점에 연결된 다른 모든 정점들의 가중치를 비교해나간다면 벨만-포드 알고리즘으로 시간복잡도가 O(N^3)까지 증가한다.

+ 단계별 정리
  1. 출발 정점과 도착 정점을 정한다.
  2. 정점과 정점 사이의 연결관계와 가중치를 저장해준다.
  3. 우선순위 큐를 활용한다.
  4. 우선순위 큐에 현재 정점에 연결된 정점의 정보와 가중치를 넣어준다.
  5. 가중치가 적은 것부터 꺼내서 비교하고 업데이트 시켜준다.
  6. 위 4,5번 과정을 반복한다.

### 구현 코드
***
[백준 1753 최단경로 문제](https://www.acmicpc.net/problem/1753)

```

    import math
    import sys
    import heapq
    
    def dijkstra():
        heap = []
        heapq.heappush(heap, (0, K))
        while heap:
            currentWeight, currentVertex = heapq.heappop(heap)
    
            for nextVertex, weight in graph[currentVertex]:
                nextWeight = currentWeight + weight
                # 이미 해당 정점까지 계산된 경로가 지금 계산하려는 값보다 작다면 이미 최단경로이다
                if shortPath[nextVertex-1] > nextWeight:  
                    shortPath[nextVertex-1] = nextWeight
                    heapq.heappush(heap, (nextWeight, nextVertex))
    
    # input
    input = sys.stdin.readline
    inf = math.inf
    
    V, E = map(int, input().split())
    K = int(input())
    
    graph = [[] for _ in range(V+1)]
    for _ in range(E):
        u, v, w = map(int, input().split())
        graph[u].append([v, w])
    
    # main
    shortPath = [inf for _ in range(V)]
    shortPath[K-1] = 0  # 시작점은 0
    dijkstra()

```

- 그래프 구현 시 2차원 배열을 생성해 가중치를 저장해주는 방식도 있으나 메모리가 많이 소모된다는 단점이 있다.

### 참조
***
[신찬수 교수님 유튜브 강의영상](https://www.youtube.com/watch?v=0NrlN88D9Fs&list=PLsMufJgu5932XYejsOwcUDJ2F75f56nrl&index=49&pp=iAQB)   
[안경잡이개발자 님 네이버 블로그](https://m.blog.naver.com/ndb796/221234424646)