---
layout: post
title:  "[알고리즘] 위상 정렬 알고리즘"
description: >
    "내용 정리"

hide_last_modified: true
---
* TOC
  {:toc}
***
### 위상정렬 알고리즘이란?
***
위상정렬(Topological Sorting) 알고리즘이란 선후 관계가 확실한 유향 그래프에서 선후 관계에 따라 정렬하기 위해 이용하는 알고리즘이다.
위상정렬 알고리즘을 사용하기 위해서는 우선 방향 그래프여야 하며 싸이클이 발생해서는 안된다.   
가장 유명한 예시로는 선수 과목을 정하는 일로 대학에서 특정 과목을 듣기 위해 어떤 선수 과목을 듣고 해당 과목을 들어야되는지 알아내는 것이 있다.

### 구현
***
1. 위상 정렬 구현을 위한 Graph Class.

```java
    private static class Graph {
        List<List<Integer>> graph = new ArrayList<>();

        public Graph(int N) {
            for (int i = 0; i < N; i++) {
                graph.add(new ArrayList<>());
            }
        }

        public void setGraph(int from, int to) {
            graph.get(from).add(to);
        }

        public List<Integer> getGraph(int from) {
            return graph.get(from);
        }
    }
```

2. Graph에서 정점들을 연결할 때 특정 정점에 몇 개의 간선이 연결되는지를 따로 체크

```java
    Graph graph = new Graph(N);
    int[] degree = new int[N];
    for (int i = 0; i < K; i++) {
        st = new StringTokenizer(br.readLine());
        int from = Integer.parseInt(st.nextToken())-1;
        int to = Integer.parseInt(st.nextToken())-1;
    
        graph.setGraph(from, to);
        degree[to]++;
    }
```

3. Main Logic

```java
private static void topologicalSort(Graph graph, int[] degree) {
    Queue<Integer> queue = new ArrayDeque<>();

    for (int i = 0; i < degree.length; i++) {
        if (degree[i] == 0) {
            queue.add(i);
        }
    }

    while (!queue.isEmpty()) {
        int from = queue.poll();
        List<Integer> cur = graph.getGraph(from);
        for (int to : cur) {
            degree[to]--;
            if (degree[to] == 0) {
                queue.add(to);
            }
        }
    }
}
```

- 코드에서 degree 값이 0이라면 해당 정점에 연결된 정점들은 이미 처리 되었거나 시작 정점이라는 의미이다.
- 맨 처음에 시작 정점을 찾아주고 Queue를 이용해 간선들을 하나씩 지워나가면서 연결된 간선이 없는 정점부터 다시 처리해준다.

### 참조
***
[위키백과 위상정렬](https://ko.wikipedia.org/wiki/%EC%9C%84%EC%83%81%EC%A0%95%EB%A0%AC)