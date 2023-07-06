---
title: "[github pages] 깃허브 블로그 설정(_config.yml)"
excerpt: "github pages를 입맛에 맞게 설정해보자!"

categories:
 - github pages
tags:
 - github
 - pages
 - github pages
 - minimal mistakes
---
#### Configuration
github pages(깃허브 블로그)는 _config.yml 파일을 이용하여 웹페이지 전반을 세팅을 할 수 있다.<br>
필자는 minimal mistakes 테마를 사용중이다.
```
# skin 변경
minimal_mistakes_skin: "dirt" # "air", "aqua", "contrast", "dark", "dirt", "neon", "mint", "plum", "sunrise"
```

기본으로 제공하는 skin이 몇가지 존재한다.<br>
[minimal mistake configuration 공홈](https://mmistakes.github.io/minimal-mistakes/docs/configuration/)에 실제 skin이 적용된 모습이 업로드되어 있으므로 참고해서 적용하면 된다.<br>
skin 목록은 _sass/minimal-mistakes/skins에서 확인 가능하며, background color 등 다양한 부분의 color를 변경할 수 있다.
```
# Site Settings
title                    : "Hello, jason!" # website 이름(tab에 보이는 이름)
title_separator          : "-"
subtitle                 : "since 2021" # masthead_title 아래 sub 형태로 보여짐.
name                     : "jason" # 프로필 사진 밑에 보여질 이름
description              : ""
url                      : "https://jason-017.github.io" # the base hostname & protocol for your site e.g. "https://mmistakes.github.io", 기본 주석 그대로 이해
baseurl                  : # the subpath of your site, e.g. "/blog"
repository               : # GitHub username/repo-name e.g. "mmistakes/minimal-mistakes"
masthead_title           : "Hello, jason!"# overrides the website title displayed in the masthead, use " " for no title, 왼쪽 상단의 가장 첫줄
```

주석은 초기 설정일 경우 UI를 통해 보이는 위치를 기반으로 설명되었습니다.
