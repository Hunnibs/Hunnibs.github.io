---
layout: post
title:  "[Java] 배열"
description: >
    "배열"

hide_last_modified: false
---
* TOC
{:toc}
***
### 1차원 배열 생성

```

    int[] array = new int[100];  // 100개의 int형 변수를 저장해줄 수 있는 배열 생성
    int[] array2 = new int[] {1, 2, 3, 4}  // 배열 생성과 동시에 안에 값을 배정해주는 방식
    int[] array3 = {1, 2, 3, 4}  // 이런 방식으로도 가능하다. 
   
```

### 2차원 배열 생성

```

    int[][] array = new int[3][4]; // 3행 4열짜리 배열 생성
    
    int[][] array2 = new int[3][]; // 3행만 생성
    array2[0] = new int[3];  // 이런 식으로 따로 행 별로 배열 생성

    int[][] array3 = {1, 2, 3}, {4, 5, 6}, {7, 8, 9}  // 3행 3열짜리 값을 배정완료   
    
    // {}로 전체를 싸줘야하는데 그러면 Liquid Error가 발생해서 포스팅이 안된다..

```

### 배열을 활용한 제어문

**foreach문**
- 배열에서 차례대로 하나씩 꺼낸다고 생각하면 이해가 편함

```

    int[] iarr = {10,20,30,40,50};

    for(int value:iarr){
        System.out.println(value);
    }

```