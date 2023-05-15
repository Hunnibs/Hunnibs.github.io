---
layout: post
title:  "[SWEA] 1859_백만 장자 프로젝트"
description: >
    "구현 문제(큐 활용)"

hide_last_modified: true
---
***
### 문제 해결 과정
***
**문제 해석**
- N일 동안의 매매가가 각각 주어졌을 때 저점에 매수해 고점에 파는 것을 반복해 최대 이익을 구하는 것

**접근 방식**   
1. 이중 for문을 이용한 간단한 구현 방식(Time Error)   

    1일차부터 N-1일차까지 뒤 전부를 돌며 가장 고점에 판매하는 방식으로 구현만 가능할 뿐 시간복잡도가 O(n^2)으로 복잡한 편이다.   

```

    import sys

    # input
    input = sys.stdin.readline

    T = int(input())

    for t in range(1, T+1):
        N = int(input())
        value = list(map(int, input().split()))
        result = 0
        for i in range(N-1):
            tmp = 0
            for j in range(i+1, N):
                if value[i] < value[j]:
                    tmp = max(tmp, value[j]-value[i])
            result += tmp

        print('#' + str(t), result)

```

2. max값을 이용한 구현 방식   

    - 매매가 정보가 리스트로 주어졌을 때 가장 고점인 날을 구한다.
    - 가장 고점인 날을 high라고 했을 때, 1일차부터 high-1일차까지는 전부 매수하고 high에서 전부 매도한다.
    - 큐의 특성을 이용해 1일차부터 순서대로 나아가면서 리스트에서 없애준다. 
    - 계속해서 하락장인 경우 O(N^2)까지 증가할 수 있으나 O(N)에 보통 해결 가능하다.   

```

    import sys
    from collections import deque

    # input
    input = sys.stdin.readline

    T = int(input())

    for t in range(1, T+1):
        N = int(input())
        value = list(map(int, input().split()))

        value = deque(value)
        answer = 0
        while value:
            high = max(value)
            idx = value.index(high)
            for _ in range(idx):
                answer += (high - value.popleft())
            value.popleft()

        print('#' + str(t), answer)

```

### 문제 풀이 후기
***
시간복잡도 생각 안하고 풀지말고 따져보면서 효율적인 코드 작성하려고 노력하자

총 소요시간: 0H 30M