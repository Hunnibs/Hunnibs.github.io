---
layout: post
title:  "[Programmers] 단속 카메라"
description: > 
  "Greedy 알고리즘 활용 문제"

hide_last_modified: true
---
> <https://school.programmers.co.kr/learn/courses/30/lessons/42884>

### 문제 해결 방안
***
- 이전에 비슷한 문제를 풀어봐서 Greedy 알고리즘 사용을 바로 결정
- 카메라를 최대한 많은 루트를 포함하는 지점에 설치하는 것을 반복
+ **고려했던 점 두가지**
  1. 출발점이나 끝나는 지점에 꽂았을 때 나오는 결과가 최적해가 될 수 있는가
  2. 만약 그렇다면 최소한의 계산으로 최적해를 구하는 방법은 무엇인가

### routes 정렬
***
- 주어진 routes 정보를 그대로 이용하기보다 출발점을 기준으로 정렬해주기로 결정
- 정렬식은 람다 함수를 이용

```

  routes = sorted(routes, key = lambda x : x[0])

```

### greedy 알고리즘 적용
***
- 출발점을 설치 지점으로 지정
- 가장 출발점이 나중인 구간부터 탐색 시작
- 출발점에 설치를 했을 때 더 빠르게 시작하는 루트들을 탐색하며 겹치는 루트를 탐색
- 현재 설치된 카메라로 단속이 가능한 구간은 가능하다는 것을 기록
- 단속이 가능한 구간은 제외하면서 남은 구간으로 해당 작업을 반복

+ 해당 방법은 출발점에 설치를 하지 않는다면 그 구간은 단속할 수 있는 방법이 없으므로 **최적해 조건 만족**
+ 부분 문제에 대해서도 해결이 가능한 방법

### 전체 코드
***
```

  def solution(routes):
      answer = 0
      install = [0 for _ in range(len(routes))]
      
      routes = sorted(routes, key = lambda x : x[0])
      
      for i in range(len(routes)-1, -1, -1):
          if install[i]:
              continue
          
          install[i] = 1
          spot = routes[i][0]
          for j in range(i-1, -1, -1):
              if routes[j][0] <= spot <= routes[j][1]:
                  install[j] = 1
              else:
                  break
          print(spot)
          answer += 1
      
      return answer

```

### 문제 풀이 후기
> 람다식을 활용한다는 생각을 하지못해 많은 시간을 소비했다.
> 총 소요 시간 1H 30M