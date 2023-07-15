---
layout: post
title:  "[Java] String"
description: >
    "String"

hide_last_modified: false
---
* TOC
{:toc}
***
### String method
***
1. charAt를 활용한 String 한 글자씩 나누기

```

    String words = "안녕하세요";
    char word = words.charAt(0);

    System.out.println(word);

    --> 안

```

String.length()를 활용해서 한글자씩 출력이 가능하다. 

```

    for (int i = 0; i < words.length(); i++){
        System.out.println(words.charAt(i));
    }

```

2. StringTokenizer를 활용한 문자열 나누기

공백을 기준으로 문자열을 토큰화하여 저장해놓는다.

```

    StringTokenizer st = new StringTokenizer(words);
    while(st.hasMoreTokens()){
        System.out.println(st.nextToken());
    }

```

> 👉 StringTokenizer란?   
> StringTokenizer란 어플리케이션이 String 자료형을 토큰화시킬 수 있게 도와주는 클래스이다. 
