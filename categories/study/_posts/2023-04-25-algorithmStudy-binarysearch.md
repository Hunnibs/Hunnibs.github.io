---
layout: post
title:  "[알고리즘] Binary Search 알고리즘"
description: >
    "내용 정리"

hide_last_modified: true
---
* TOC
{:toc}
***
### 이진탐색이란?
***
데이터가 정렬돼 있는 리스트에서 특정한 값을 찾아내기 위한 알고리즘   
검색이 반복될 때마다 완전탐색과는 다르게 검색 범위를 절반으로 줄일 수 있는 장점이 있다.

### 구현 방법
***
리스트 내 Left, Mid, right 값을 사용하며 찾고자 하는 값에 따라 Mid 값을 계속해서 계산해준다.

1. 리스트 내 첫 인덱스를 Left(= 0)라 지정하고 가장 마지막을 Right(= len(list)-1)라고 지정한다.
2. mid = (left + right) // 2, 계산해서 중간 인덱스값을 구한다.
3. 찾고싶은 값이 list[mid]보다 작으면 right = mid-1, 크다면 left = mid+1로 업데이트 시켜준다.
4. 2,3번 과정을 반복하면 찾고자하는 값을 구할 수 있다.

이런 특징 때문에 이진탐색은 무조건 정렬되어있는 리스트에서만 사용이 가능하다.

### 구현 코드
***
2가지 방법이 존재하며 재귀함수로 구현하는 것과 반복문으로 구현하는 것이 있다.
1. 재귀 함수로 구현한 이진 탐색

```

    def binary_search_recursive(list, target, left, right):
        if left > right:
            return
    
        mid = (left + right) // 2
        
        if list[mid] == target:
            return mid
        elif list[mid] > target:
            return binary_search_recursive(list, target, left, mid-1)
        else:
            return binary_search_recursive(list, target, left, mid - 1)

```

2. 반복문으로 구현한 이진탐색

```

    def binary_search_iter(list, target, left, right):
        while left <= right:
            mid = (left + right) // 2
            
            if list[mid] == target:
                return mid
            elif list[mid] > target:
                right = mid - 1
            else:
                left = mid + 1
        
        return

```