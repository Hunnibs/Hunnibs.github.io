---
layout: post
title:  "[Jekyll] GitHub 블로그 개발일지(1)"
description: > 
    "간단하게 적어놓는 초기 세팅 과정"

hide_last_modified: true
---
* TOC
{:toc}
***
### 1.1. 개인 레포지토리 생성
***
- "username.github.io"로 github repository 생성

    <img src="/assets/img/jekyll/createRepo.png" width="450px" height="300px">

- Github Desktop 앱을 이용해 만들어둔 repository를 local 저장소로 가져옵니다.

### 1.2. 루비 설치
***
- <https://jekyllrb.com/> 참조
- DOCS -> Quickstart 순서대로 진행

### 1.3. 블로그 테마 적용
***
- <https://github.com/mmistakes/minimal-mistakes>
- code 전체 Zip 파일 다운로드
- 압축 푼 후, local 저장소에 가져온 레포지토리 파일에 복사 붙여넣기를 해줍니다.

+ gemfile.lock에서 버전 오류가 발생할 수 도 있으므로 삭제후 bundle install 권장

### 1.4. 블로그포스팅
***
1.4.1.
+ _pages 폴더에 카테고리를 추가

```

    ---
    title: "블로그 개발일지"
    layout: archive
    permalink: /blog/
    author_profile: true
    sidebar_main: true
    ---
    
```

+ archive 활용 목적을 title에 적어주고 permalink는 앞으로 연결될 링크로 지정해줄것

1.4.2.
- _posts 폴더를 생성
- markdown 파일들은 여기에 보관할 예정

```

    ---
    title:  "[Jekyll] 블로그 포스팅 테스트"
    excerpt: "업로드 확인용"
    
    categories:
      - blog
    
    tags:
    
      - [Blog, jekyll, Github, Git]
    
    toc: true
    toc_sticky: true
    
    date: 2023-04-14
    last_modified_at: 2023-04-14
    ---
    
```

- categories.blog로 문서저장경로를 지정해준다.

### 1.5. Navigaion.yml 파일 설정
***
- 1.4를 모두 마무리 했다면 _data/navigation.yml 파일로 이동

```

    main:
  - title: "HOME"
    url: "/"
  - title: "ALGORITHM"
    url: /categories/algorithm/
  - title: "BLOG"
    url: /categories/blog/
  - title: "SPRING"
    url: /categories/spring/
        
```

- 위와 같이 지정해준다면 페이지 상단에 해당 카테고리로 이동할 수 있는 네비게이션 바가 생성된다. 

### 참조
***
- [Jekyll 공식 사이트](https://jekyllrb.com/)
- [Minimal mistakes](https://mmistakes.github.io/minimal-mistakes/)
- [공부하는 식빵맘 님 블로그](https://ansohxxn.github.io/)