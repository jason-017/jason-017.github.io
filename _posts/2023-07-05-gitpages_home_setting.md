---
title: "[github pages] 최초 접속(home) 페이지 설정"
excerpt: "github pages(깃허브 블로그) 구축"

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