---
layout: post
title:  "[Programmers] 실패율"
description: >
    "구현 문제"

hide_last_modified: true
---
> <https://school.programmers.co.kr/learn/courses/30/lessons/42889>   
> 어려운 문제는 아니었지만 Zerodivisionerror를 포스팅 할 겸 정리


### 문제 해결방안
***
**문제 해석**
+ 문제는 스테이지 당 통과 사람과 통과하지 못한 사람의 비율로 실패율을 계산
+ 각 스테이지마다 해당 계산을 반복해 실패율을 계산
+ 실패율이 높은 스테이지 순으로 정리해서 출력   


**접근 방식**
+ 모든 스테이지를 다 깬 사람 수를 먼저 파악
+ 가장 뒷 스테이지부터 내려오는 방식을 택했음
  + 성공한 사람들의 수를 누적합으로 구해주기 위함
+ 스테이지 실패율 리스트를 만들어 스테이지 당 실패율을 저장해줌
+ 리스트 내 max 함수를 통해 실패율이 높은 것 부터 answer에 넣어주고 -1로 값을 변경   

**문제점**   
> ZeroDivisionError   
어떠한 수를 0으로 나눌 때 발생하는 Error Case   

- 해당 문제에서는 마지막 스테이지에 도달한 사람도 없을 경우 0을 0으로 나누게 되어 발생한다.
- 예외처리를 해줌으로써 간단하게 해결 가능하다.

### 전체 코드
***

```

    def solution(N, stages):
        answer = []
        
        clear = stages.count(N+1)
        
        result = [-1 for _ in range(N+1)]
        for stage in range(N, 0, -1):
            fail = stages.count(stage)
            clear += fail
            if clear == 0:
                result[stage] = 0.0
            else:
                result[stage] = fail / clear 
            
        for _ in range(N):
            idx = result.index(max(result))
            answer.append(idx)
            result[idx] = -1
        return answer

```

### 문제 풀이 후기
***
시간복잡도가 끔찍하지만 실제 코딩테스트 기출문제다 보니 우선 시간 안에 푸는 것에 집중했다.   

총 소요시간 : 0H 20M