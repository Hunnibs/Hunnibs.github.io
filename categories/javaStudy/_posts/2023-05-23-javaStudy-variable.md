---
layout: post
title:  "[Java] 변수"
description: >
    "변수"

hide_last_modified: false
---
* TOC
{:toc}
***
### 기본 데이터 타입
***

![DataType](assets\img\javaStudy\DataTypes.png)

데이터 타입은 크게 논리형, 문자형, 정수형, 실수형으로 나누어진다.

**논리형**
- true, false와 같은 참, 거짓을 판별하는 Data를 저장하는 변수
- 1byte의 크기를 가진다.

**문자형**
- 딱 하나의 문자만을 저장할 수 있는 변수, 하나 이상의 문자를 저장할 시 오류가 발생한다.
- 2byte의 크기를 가진다.

**정수형**
- Data 크기와 저장할 수 있는 값의 범위에 따라 다양하게 나뉜다.
    
    - byte는 1byte로 가장 작은 크기이며 -128 ~ 127까지만을 저장할 수 있다.
    - short와 char는 2byte의 크기를 가지며 short는 음수와 양수를 반반 씩, char는 양수범위만 저장한다.
    - int는 4byte로 더 넓은 범위를 저장할 수 있다.
    - long은 8byte로 웬만하면 모든 범위의 수를 저장할 수 있다.
        - 단, 변수 선언시 long big = 234141515*L*로 L을 붙여줘야한다.

**실수형**
- 소수점 아래 자리까지 저장할 수 있는 type으로 크기는 두 가지로 나뉜다.

    - float은 4byte
    - double 8byte

> 👉 상수   
>
> 기본 데이터 타입 앞에 final을 붙여서 불변성을 가진 data 저장공간을 만들 수 있다.   
> ex) final int number = 3; 이면, number는 3이라는 값으로 고정된다.

### Type 형변환
***
정수형과 실수형 사이에서는 Type 형변환이 가능하다. 이 때 정수는 실수에 포함된다는 사실을 기억해야한다.

1. 묵시적 형변환
- int형 변수를 float형으로 바꾸는 경우
- 실수는 정수를 애초에 포함하고 있기 때문에 암묵적으로 변환을 해준다.

2. 명시적 형변환
- 반대로 float형을 int형으로 바꾸는 경우
- float type 변수에 5와 같은 정수가 저장되어있더라도 변환이 불가능하다.
- 그래서 이런 경우에는 강제로 형변환을 시도해주어야한다.
- ex)

    ```

        float number = 5;
        int number2 = (int)number;

    ```
