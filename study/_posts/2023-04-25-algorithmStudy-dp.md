---
layout: post
title:  "[알고리즘] D.P 알고리즘"
description: >
    "내용 정리"

hide_last_modified: true
---
* TOC
{:toc}
***
### DP(Dynamic Programming)이란?
***
문제를 작은 문제로 (중복될 수도 있지만) 분할하여 분할된 문제의 해답을 기록해 놓은 후 더 큰 문제의 해답을 계산할 때 재사용하는 방법을 의미한다.


### 구현 방법 
***
DP 알고리즘의 4단계 분류
1. 해답의 구조를 파악해 어떻게 부문제로 나눌 지 결정
2. 부문제의 해답을 어떻게 조합해 더 큰 문제의 해답을 만들 수 있는지 식을 마련
3. 작은 문제에서 큰 문제로 적당한 순서에 따라 해답을 계산하여 테이블에 저장하고 재사용하여 답을 계산
4. 위의 단계들이 원래 문제의 답을 항상 출력함을 증명하고 답을 리턴

### 주의할 점
***
- DP Table을 만들 때 list index error에 대한 고려 항상 해줄 것

### DP 활용방식
***
**Bottom-up 방식**
- 우리말로 ‘상향식’
- 가장 작은 문제들부터 답을 찾아 가면서 마지막에는 전체 문제의 답을 구하는 방식
- 반복문으로 구현하므로 시간과 메모리의 사용량이 상대적으로 적음

**Top-down 방식**
- 우리말로 ‘하향식’
- 큰 문제를 해결하기 위해 작은 문제를 호출하는 방식
- 재귀호출을 통해 구한다. 함수 호출의 횟수를 줄이기 위해 메모이제이션 기법을 활용

> 메모이제이션 기법   
> -> 컴퓨터 프로그램이 동일한 계산을 반복할 때, 이전에 계산한 값을 메모리에 저장해놓는 방법
DP의 핵심이 되는 기술이다.


**누적 합 활용 방식**
- 배열이 있을 때 해당 배열까지의 총 합을 따로 배열에 저장하는 방식
- 시간복잡도 상 큰 이득을 가져다주는 경우가 많다