---
title: "[github pages] 깃허브 블로그 config 설정"
excerpt: "github pages를 입맛에 맞게 설정해보자!"

categories:
 - github pages
tags:
 - github
 - pages
 - github pages
 - minimal mistakes
---
## Configuration
github pages(깃허브 블로그)는 _config.yml 파일을 통해 웹페이지 전반을 세팅할 수 있다.
### theme skin
필자는 minimal mistakes 테마에 dirt skin을 사용중이며 스킨 색상을 커스텀하여 사용하고 있다.
```bash
# skin 변경
minimal_mistakes_skin: "dirt" # "air", "aqua", "contrast", "dark", "dirt", "neon", "mint", "plum", "sunrise"
```

[minimal mistake configuration 공홈](https://mmistakes.github.io/minimal-mistakes/docs/configuration/)에 skin 예시를 확인할 수 있으므로 참고해서 적용하면 된다.<br>
skin별 색상 조합은 `_sass/minimal-mistakes/skins`에서 확인 가능하며, background color 등 다양한 부분의 색상을 변경할 수 있다.
### site settings
웹페이지에 기본적으로 보여질 웹페이지 제목, 기본 언어, 이름 등을 설정할 수 있다. 필자의 경우 아래만 설정해도 사용성에 큰 문제가 없다고 생각된다.
```bash
# Site Settings
locale                   : "ko-KR" # 기본 언어
title                    : "jason 테크 백과사전" # 웹페이지 주소창에 표시될 제목
title_separator          : "-"
subtitle                 : "since 2021" # site tagline that appears below site title in masthead
name                     : "jason"
description              : ""
url                      : "https://jason-017.github.io" # the base hostname & protocol for your site e.g. "https://mmistakes.github.io"
baseurl                  : # the subpath of your site, e.g. "/blog"
repository               : # GitHub username/repo-name e.g. "mmistakes/minimal-mistakes"
teaser                   : # path of fallback teaser image, e.g. "/assets/images/500x300.png"
logo                     : # path of logo image to display in the masthead, e.g. "/assets/images/88x88.png"
masthead_title           : "Hello, jason!"# overrides the website title displayed in the masthead, use " " for no title
...
```

![site settings](/assets/site_auth1.png)
### site author
웹페이지에 보여질 작성자 정보를 설정한다고 생각하면 된다.
```bash
# Site Author
author:
  name             : "jason" # 프로필 이름
  avatar           : "/assets/image.jpeg" # 프로필 사진
  bio              : "**지극히 주관적인 테크 백과사전**" # 프로필에 표시하고 싶은 글
  location         : "South Korea" # 나라
  email            : 
  links:
    - label: "Email"
      icon: "fas fa-fw fa-envelope-square"
      url: "mailto:${개인 이메일 주소}"
	  ...
    - label: "Instagram"
      icon: "fab fa-fw fa-instagram"
      url: "${개인 인스타그램 주소}"
	  ...
```

![auth settings](/assets/site_auth2.png)
이외 추가적인 부분은 공식 홈페이지를 참고하여 세팅하면 된다.<br>
참고로 필자는 해당 수준의 세팅만으로도 충분히 만족하며 사용중이다. 추후에 시간적 여유가 생기면 직접 레이아웃 등을 커스터마이징할 예정이다.<br>
만약 페이지를 더 이쁘게 꾸미는 게 목적이면 config.yml에 시간을 들이기 보다는 _includes, _data 등을 만지는 게 더 적합할 것 같다.