---
title: "github pages 설정"
excerpt: "github pages 설정 draft"

categories:
 - github pages
tags:
 - github
 - pages
 - github pages
---

#### Configuration

_config.yml: github pages 전체 site를 바꿀 수 있는 파일

```
# skin 변경
minimal_mistakes_skin: "dirt" # "air", "aqua", "contrast", "dark", "dirt", "neon", "mint", "plum", "sunrise"
```

기본으로 제공하는 skin이 몇가지 존재한다.

[minimal mistake configuration 공홈](https://mmistakes.github.io/minimal-mistakes/docs/configuration/)에 실제 skin이 적용된 모습이 업로드되어 있으므로 참고해서 적용하면 된다.

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

## github pages에 사진 넣는 방법
- 마크다운 형식을 따르기 때문에 ```![표시할 이미지명](이미지 절대 경로)```로 삽입이 가능하다. local PC에서는 상대 경로를 사용해도 웬만하면 사진이 제대로 출력되는데, github pages의 경우 상대 경로를 입력하면 엉뚱한 경로를 바라볼 수도 있기 때문에 <u>절대 경로를 입력</u>해줘야 한다.
	- 절대 경로 사용: ```![test](/assets/images/test.png)```
	- 상대 경로 사용: ```![test](../assets/images/test.png)```



글자 크기(font size), 프로필 사진, 카테고리 등 상단 메뉴 설정 정리




아래 글들 따라하고 나만의 내용으로 정리하기

https://devinlife.com/howto%20github%20pages/github-pages-settings/

https://eona1301.github.io/github_blog/GithubBlog-Content-Width/

https://danggai.github.io/categories/#github-io
