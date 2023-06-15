---
layout: post
title:  "[Java] 입력"
description: >
    "BufferedReader & StringTokenizer"

hide_last_modified: false
---
* TOC
{:toc}
***
### 사용 이유
***
Scanner를 이용한 입력을 받을 시

```

    Scanner sc = new Scanner(System.in);

    for(int i=0; i < 10; i++){
        System.out.println(sc.nextInt());
    }

```

반면, BufferReader와 StringTokenizer를 활용한다면 더 빠르게 가능하다.   
BufferReader와 StringTokenizer를 이용해 입력받을 시

```

    BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    StringTokenizer st = new StringTokenizer(br.readLine());

    int N;
    int M;
    int[] nums;

    N = Integer.parseInt(st.nextToken());
    M = Integer.parseInt(st.nextToken());
    
    nums = new int[N];
    for(int i = 0; i < N; i++){
        nums[i] = Integer.parseInt(st.nextToken());
    }

```

### 참조
***
[devdo님 블로그](https://velog.io/@mooh2jj/Java-%EC%9E%85%EC%B6%9C%EB%A0%A5-BufferedReader-StringTokenizer%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0)