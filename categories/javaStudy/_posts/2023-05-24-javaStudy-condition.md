---
layout: post
title:  "[Java] 제어문"
description: >
    "제어문"

hide_last_modified: false
---
* TOC
{:toc}
***
### If문

```

    if (조건식){
        실행문;
    }
    else if(조건식){
        실행문;
    }
    else{
        실행문;
    }

```

### Switch문

```

    int value = 1;

    switch(value){
        case 1:
            실행문;
            break;  // 반드시 break문을 작성해줘야함
        case 2:  
        case 3:
        case 4:  // 이런식으로 케이스를 묶어서도 가능
            실행문;
            break;

        default:  // 아무것도 해당 안될 때 실행하는 케이스
            실행문;
            break;
    }

```

### while문

```

    while(조건식){
        실행문;
    }

```

###  Do-While문
- while문은 조건이 충족되지 않는다면 한 번도 실행하지 않지만 Do-while문을 사용하면 최소 한 번의 실행문 실행을 보장한다.

```

    do{
        실행문;
    }while(조건문)

```

### for문

```

    for(초기화식; 조건식; 증감식){
        실행문;
    }

```
