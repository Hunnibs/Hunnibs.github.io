---
layout: post
title:  "[Jekyll] GitHub 블로그 개발일지(2)"
description: >
    "빠르게 찾아온 더 이쁜 테마에 대한 열망"

hide_last_modified: true
---
* TOC
{:toc}

***
### 2.1 새로운 테마 선정
***
- [juxgsiroo 님 Velog](https://velog.io/@juxgsiroo/github-page-pt2) 추천 참조   

> 기존에 쓰던 테마가 너무 단순하고 밋밋하다는 생각이 들던 때에 새로운 테마를 구경하게 되었고 지금까지 했던걸
> 과감히 버리고서라도 변경하기로 결정했다

### 2.2 새로운 테마 적용
- 기존에 저장했던 Markdown 폴더들을 전부 따로 빼준 뒤 레포지토리를 비워주었다
- 이전에 했던 것 처럼 Hydejack을 Zip으로 가져와 레포지토리에 넣어주는 방식을 활용
+ [hydecorp/hydejack Repository](https://github.com/hydecorp/hydejack)
+ 당연히 해당 repository를 가져오면 될 것이라 생각했지만 이 파일에는 gemfile이 존재하지 않는다

    ![Nogem](/assets/img/jekyll/noGem.png)
  
> gemfile이 없다면 'bundle exec jekyll serve' 를 실행 시
> 'Could not locate Gemfile' 이라는 Error가 발생한다.

- 해결방법은 간단하다
- [hydecorp/hydejack-starter-kit](https://github.com/hydecorp/hydejack-starter-kit)
- 해당 레포지토리로 이동하면 스타터킷 답게 다 포함되어있는 것을 확인할 수 있다.
- (추가) masterkit으로 다운받았지만 현재 버전과 다른 점이 많아 버전에 맞춰 다운받는것이 편할 수도 있다.

### 2.3 주의할 점
***
- minimal mistakes 테마를 사용하던 때와는 다르게 조심해줘야할 점들이 많았다.
- 많이 안 쓰는 테마여서 웬만해서는 Guide나 Demo site와 코드를 비교해보며 알아가야했다.

**2.3.1** _config.yml Theme 설정

```
    
    # Theme
    # ---------------------------------------------------------------------------------------
    
    #theme: jekyll-theme-hydejack
    remote_theme: hydecorp/hydejack@v9

```

- 해당 코드를 보면 theme와 remote_theme가 나누어져있다
- local에서 테스트 해볼 때는 theme를 주석처리를 풀고 remote_theme를 주석처리 
- 서버에 올릴 떄는 위의 예시코드처럼 사용해주어야함

> 따로 layout 폴더를 만들지 않고 Jekyll에서 기본으로 주어지는 layout plugin을 사용하기 때문에
> 상황에 맞게 바꿔주지 않으면 해당 부분이 로드되지 않는다.

**2.3.2** 폴더 경로 설정
- 가장 시간을 많이 잡아먹었고 스트레스 받았던 부분이다.
  - 아래와 같이 Local에서는 제대로 동작해도 서버에 올리면 다르게 동작하는 등 많은 문제점이 발생했다.

  ![Error](/assets/img/jekyll/pathError.png)

+ 우선 _featured_categories 폴더에 카테고리 별로 분류를 해주기 위한 Markdown 파일을 작성

```
    
    ---
    # Featured tags need to have either the `list` or `grid` layout (PRO only).
    layout: list
    
    # The title of the tag's page.
    title: Algorithm
    
    # The name of the tag, used in a post's front matter (e.g. tags: [<slug>]).
    slug: algorithm
    
    # (Optional) Write a short (~150 characters) description of this featured tag.
    description: >
        알고리즘 문제 풀이
    
    # (Optional) You can disable grouping posts by date.
    # no_groups: true
    
    sitemap: false
    ---

```

- 다음으로 카테고리에 post를 올리기 위해서는 해당 사진처럼 해준다
    
    <img src="/assets/img/jekyll/path.png" width="450px" height="100px">

- 주의해야할 점은 _config 파일에 해당 포맷대로 파일명을 작성해줘야 인식한다는 점이다.

```

    # Format of the permalinks
    permalink:             /:categories/:year-:month-:day-:title/

```

> 이외에도 필자는 Blog라는 키워드는 CHANGELOG.md 파일의 특정 부분과 문제를 일으켜서 사용 시 버그가 발생하는 문제점이 있었다.
> 누군가는 이글을 보며 조금이라도 편하게 Hydejack 테마를 이용할 수 있기를 바라는 마음에 작성한다.