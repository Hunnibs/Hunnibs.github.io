---
layout: post
title:  "[SWEA] 1244_최대상금"
description: >
    "구현 문제"

hide_last_modified: true
---
> <https://swexpertacademy.com/main/code/problem/problemDetail.do?problemLevel=2&problemLevel=3&contestProbId=AV15Khn6AN0CFAYD&categoryId=AV15Khn6AN0CFAYD&categoryType=CODE&problemTitle=&orderBy=RECOMMEND_COUNT&selectCodeLang=ALL&select-1=3&pageSize=10&pageIndex=1>


### 문제 해결 과정
***
**문제 해석**
- 정해진 횟수만큼 두 숫자의 위치를 바꿨을 때(중복 가능), 가장 큰 숫자가 나와야한다.

**접근 방식**   
- Greedy 알고리즘 활용(잘못된 접근)   

    매 번 최선의 선택을 하면 결과값이 최선일거라는 실수를 저질렀다. greedy 방식의 경우는 최적의 선택들을 계속 한다해서 결과가 최적이라는 보장이 없다는 것을 공부할 때 정리하고도 문제를 풀 때 생각없이 사용했다.   

    [[알고리즘] Greedy 알고리즘](https://hunnibs.github.io/categories/study/2023-04-20-algorithmStudy-Greedy/) 해당 포스트 내용 참조

> 조건을 다시 생각   
> 1. max값을 가장 맨 앞에 오게 하는 것이 최대가 되게 하는 방식   
> 2. max값이 여러개라면? 일일히 탐색해보며 결과가 가장 최대가 될 때를 고르자   
>
> 해당 문제는 최대 6자리 교환횟수 10번 이내이므로 DFS 알고리즘과 브루트포스 알고리즘을 함께 사용해 모든 경우를 다 탐색해봐도 가능할 것이라고 생각했다.

- Queue를 활용한 DFS 알고리즘 활용(잘못된 접근)   

    max 값을 가장 앞으로 이동시켰을 때 popleft를 통해 제거해준다면 시간복잡도가 더 단순해질 것으로 생각했다. 하지만 여러가지 조건들을 추가하다보니 재귀문에서 돌아올 때 제거했던 값을 다시 넣어주는 것이 너무 복잡했고 계속해서 index error가 발생해서 포기했다. 

- DFS 알고리즘 활용(실제 풀이)   

    시간이 오래걸리더라도 가장 깔끔하게 답을 구할 수 있는 방식으로 조건이 까다로운 부분만 해결하면 문제없이 구현할 수 있었다.

### 풀이 코드
***

- 코드가 매우 난잡해 최대한 주석을 달아 설명하려함

```

    # cnt는 남은 반복 횟수, 0 ~ C는 이미 정렬된 숫자를 의미한다. 즉, C는 이제 가장 큰 숫자가 와야하는 index값
    def sol(cnt, C):
        # 해당 조건문은 깊이가 최대에 달했을 때를 처리
        if cnt == 0:
            tmp = []
            for i in range(len(answer)):
                tmp.append(str(answer[i]))
            for i in range(C, len(nums)):
                tmp.append(str(nums[i]))

            result.append(''.join(tmp))

            return

        # 해당 루프문은 만약 가장 큰 숫자가 가장 앞에 있는 경우는 변경할 필요가 없어 처리해줌
        while max(nums) == nums[C]:
            answer.append(nums[C])
            nums[C] = -1
            C += 1

            if C == len(nums):
                break

        # cnt는 남았는데 이미 최대금액으로 정렬된 경우
        if C == len(nums):
            if cnt % 2 == 0 or status:  
            # 중복되는 숫자가 있다면 그 숫자들만 자리를 바꿔주거나 
            # or 중복되지않아도 남은 횟수가 짝수면 다시 원래대로 돌려놓을 수 있다.
                tmp = []
                for i in range(len(answer)):
                    tmp.append(str(answer[i]))
                result.append(''.join(tmp))
            # 자리를 바꿔줘야하는 경우
            else:
                tmp = answer[-1]
                answer[-1] = answer[-2]
                answer[-2] = tmp
                tmp = []
                for i in range(len(answer)):
                    tmp.append(str(answer[i]))
                result.append(''.join(tmp))

            return

        maxNum = max(nums)

        # DFS 동작 코드
        for i in range(len(nums)):
            if nums[i] == maxNum:
                nums[i] = nums[C]
                nums[C] = -1
                answer.append(maxNum)
                sol(cnt-1, C+1)
                answer.pop()
                nums[C] = nums[i]
                nums[i] = maxNum

    # input
    T = int(input())
    for t in range(1, T+1):
        result = []
        answer = []

        nums, cnt = input().split(' ')

    # main
        cnt = int(cnt)
        nums = list(nums)
        for i in range(len(nums)):
            nums[i] = int(nums[i])

        status = 0
        # 중복 Status
        if len(set(nums)) != len(nums):
            status = 1

        sol(cnt, 0)

        ans = 0
        while not ans:
            if len(max(result)) != len(nums):
                result[result.index(max(result))] = ''
            else:
                ans = max(result)

        print('#' + str(t), ans)

```

### 문제풀이 후기
***
체감 상 굉장히 까다로운 문제였다. 조건이 많고 시간과 공간복잡도를 생각했을 때 섣불리 코드를 짜기가 어려워 줄이려고 노력하다가 시간이 오래걸렸던 것 같다. 느꼈던 건 뭔가 구현문제에 가까웠던 것 같다. 

총 소요시간: 3H