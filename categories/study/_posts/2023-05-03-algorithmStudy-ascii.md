---
layout: post
title:  "[기본 지식] Ascii Code"
description: >
    "내용 정리"

hide_last_modified: true
---
* TOC
{:toc}
***
### 아스키 코드란?
***
![ascii](/assets/img/study/ascii.gif)   

컴퓨터에서 기호 혹은 문자를 기억하고 있는 체계로 알아두면 좋은 것들이다.   
대표적으로,   
- 48 ~ 57: 실제 숫자 0~9를 담당
- 65 ~ 90: A ~ Z
- 97 ~ 122: a ~ z

해당 구간은 반드시 외우는 것이 편하다.

### 활용 예시
***
> <https://school.programmers.co.kr/learn/courses/30/lessons/72410>

```

    def solution(new_id):
        answer = ''
        
        for word in new_id:
            if len(answer) == 15:
                break
            
            if 65 <= ord(word) <= 90:
                word = chr(ord(word)+ 32)
                answer += word
            elif 97 <= ord(word) <= 122:
                answer += word
            elif 48 <= ord(word) <= 57:
                answer += word
            elif word == '-' or word == '_':
                answer += word
            elif word == '.':
                if not answer or answer[-1] == '.':
                    continue
                else:
                    answer += word
        
        for i in range(len(answer)-1, -1, -1):
            if answer[i] == '.':
                answer = answer[:i]
                print(answer)
            else:
                break
        
        while len(answer) <= 2:
            if not answer:
                answer += 'a'
            else:
                answer += answer[-1]
        
        return answer

```

해당 문제를 Ascii 코드표를 활용해 풀어본 코드이다.   

대소문자 변환은 파이썬에서 ascii 코드표를 활용하지않고 간단하게 해결할 수 있는 방법이 존재한다.
- String.upper() & String.lower()
- 위를 사용하면 문자열 전체를 대소문자로 자동으로 변환해준다.