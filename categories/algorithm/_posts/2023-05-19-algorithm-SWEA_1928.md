---
layout: post
title:  "[SWEA] 1928_Base64 Decoder "
description: >
    "구현 문제(Ascii code표 활용)"

hide_last_modified: true
---
> <https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV5PR4DKAG0DFAUq>

### 문제 해결 과정
***
**문제 해석**
- 이 부분은 문제에 대한 이해가 가지 않아 *최준아_0847231님*의 댓글을 참고했다.

> 문제요약
> 1. 표1을 보고 입력받은 문자들을 각각 대응하는 숫자로 변경한다.
> 2. 각 숫자들을 6자리 이진수로 표현하고, 이 이진수들을 한 줄로 쭉 이어 붙인다.
> 3. 한 줄로 쭉 이어붙인 이진수들을 8자리씩 끊어서 십진수로 바꾼다.
> 4. 각각의 십진수를 아스키 코드로 변환한다.

**접근 방식**   
- 문제가 어렵다기보다는 아스키 코드를 활용할 수 없다면 풀 수가 없는 문제였다.   

    [[기본 지식] Ascii Code](https://hunnibs.github.io/categories/study/2023-05-03-algorithmStudy-ascii/) 해당 포스트 내용 참조

### 풀이 코드
***

**코드 구성 설명**
1. 주어진 문자열에서 문자 하나하나 당 아스키코드 변환식을 통해 자연수로 바꿔준다.
2. 바꿔준 자연수를 decoding(num) 함수에 넣어 이진수로 변환해준다.
3. 전부 이진수로 변환했다면 translate(sum) 함수에 넣어 8자리씩 슬라이스를 이용해 끊어준 뒤, 다시 정수로 변환시켜준다.
4. 정수로 변환시켜주고 아스키코드 변환을 이용해 다시 문자열로 변환해준다.
5. 3~4 과정을 끝까지 반복하면 완전한 문장이 완성된다.

```

    def decoding(num):
        decode = ''
        for i in range(5, -1, -1):
            if num - (2**i) >= 0:
                decode += '1'
                num = num - (2**i)
            else:
                decode += '0'

        return decode

    def translate(sum):
        result = ''

        a = len(sum) // 8
        for i in range(a):
            b = i * 8
            tmp = sum[b:b+8]

            num = 0
            i = 7
            for t in tmp:
                t = int(t)
                num += t * (2**i)
                i -= 1

            result += chr(num)
        return result

    # input
    T = int(input())
    for t in range(1, T+1):
        base = input()
        sum = ''

    # main
        for word in base:
            if 48 <= ord(word) <= 57:
                num = ord(word)+4
                sum += decoding(num)

            elif 65 <= ord(word) <= 90:
                num = ord(word)-65
                sum += decoding(num)

            elif 97 <= ord(word) <= 122:
                num = ord(word)-71
                sum += decoding(num)

            elif word == '+':
                num = 62
                sum += decoding(num)

            else:
                num = 63
                sum += decoding(num)
        
        result = translate(sum)
        
        print('#' + str(t), result)

```

### 문제풀이 후기
***
오랜만에 풀어보는 재밌는 문제였던 것 같다. 알고리즘을 푼다는 생각보다는 암호해석을 하는 듯 하여 재밌었다. 문제 자체의 난이도는 높지 않은 것 같으나 본인도 아스키코드 표를 참조하지 않고는 풀기가 힘들었던 문제였다. 그만큼 아스키 코드에 대해 다 외우고 있지 못한다면 풀기 어려운 문제라고 생각한다.

총 소요시간: 0H 30M