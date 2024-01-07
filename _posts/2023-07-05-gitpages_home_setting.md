---
title: "[github pages] 최초 접속(home) 페이지 설정"
excerpt: "github pages 구축"

categories:
 - github pages
tags:
 - github
 - pages
 - github pages
---

## github pages 대문 페이지 설정하기

minimal mistakes 기준으로 대표 링크로 접속했을 때 recent posts가 표시된다. 말 그대로 최신 포스팅 순으로 보여준다.

필자의 경우 바로 카테고리를 보여주고 싶었고 간단히 해결할 수 있었다. index.html의 layout을 원하는 layout으로 변경해주면 된다. layout 목록의 경우 layouts 라는 디렉토리 아래에서 확인할 수 있다.

```bash
$ vi index.html
---
layout: categories
author_profile: true
permanentlink: /categories/
---
```

## 상단 메뉴 설정하기

_data > navigation.yml 을 만져주면 상단 메뉴를 설정할 수 있다. navigation.yml은 _pages 아래 마크다운 파일을 활용하여 화면을 출력해준다. 주의할 점은 navigation.yml에서 메뉴 목록을 제거하더라도 _pages 내 파일의 permalink가 살아있으면 링크로 접근이 가능하므로 사용하지 않을 page의 경우 permalink도 제거해줘야 한다.

**navigation**

<img src="/assets/image-20240107170618974.png" alt="navigation yaml 내용" style="zoom:50%;" />

**pages**

<img src="/assets/image-20240107171034318.png" alt="pages category 내용" style="zoom:50%;" />
