---
layout: post
title:  "[Jekyll] GitHub 블로그 개발일지(4)"
description: >
    "Liquid Exception: Liquid syntax error"

hide_last_modified: true
---
* TOC
{:toc}
### 4.1 상상치도 못한 Error 발생
***
- 로컬 환경 Error 내용
> Liquid Exception: Liquid syntax error (line 23): Variable '{% raw %}{{1, 2, 3}}{% endraw %}' was not properly terminated with regexp: /\}\}/ in C:/Users/cqqud/Documents/GitHub/Hunnibs.github.io/categories/javaStudy/_posts/2023-05-24-javaStudy-array.md

- Github page action Error 내용
<p align="center"><img src="\assets\img\jekyll\buildError.png" width="800px" height="300px" title="px(픽셀) 크기 설정" alt="BuildError"></p>

아무것도 건드린거 없이 포스팅만 추가했는데 Error가 발생했다. 아무것도 하지 않았는데 발생한 Error라 머리 속이 새하얘졌다.

### 4.2 Error 해결과정
***
1. Error message에 나온 해당 파일에서만 문제가 발생하는지 확인부터 진행   
    
    - 3개의 포스팅을 업로드 한 번에 업로드했어서 저 파일을 제외한 파일들을 올려봤다.

2. 문제없이 포스팅이 되는 것을 확인한 후 Error message의 해당 줄을 확인
    - Code 작성과 관련된 부분이 었는데 중괄호를 두 번 쓴 것이 문제라는 것 같았다.
    
3. 해당 Error가 일어나는 이유를 구글링으로 찾아 해결   
        
    - Liquid라는 루비 기반의 템플릿 언어는 {% raw %}{{, }}{% endraw %}를 escape 문자로 활용하기 때문에 Error가 발생하는 것

    참조 -> [iamheesoo 님 깃헙 블로그](https://iamheesoo.github.io/blog/gitblog-sol-jekyll02)

4. 해결법   
    
    ![sentence](\assets\img\jekyll\sentence.png)   
    - 이 해당 문구는 그냥 적으면 없애버려서 이미지 파일로 첨부해야한다.
    - 굉장히 불편한 부분