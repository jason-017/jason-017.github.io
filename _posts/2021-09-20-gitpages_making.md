---
title: "[github pages] 깃허브 블로그 만들기"
excerpt: "github pages 구축"

categories:
 - github pages
tags:
 - github
 - pages
 - github pages
 - minimal mistakes
typora-root-url: ./..
---
## Github Pages
github에서 제공하는 무료 웹 호스팅 서비스인 github pages가 존재하는데 많은 사람들이 블로그로 해당 서비스를 많이 이용하는 것 같다. 장점은 무료로 웹호스팅을 사용할 수 있다는 점이고, 단점으로는 결국 개인 도메인을 갖기 위해서는 비용이 발생한다는 점과 작성된 모든 파일들이 github를 통해 모두 노출된다는 점이다.
## github repository 생성
github pages를 이용하기 위해서 repository name을 `${username}.github.io`로 입력하고 public으로 설정해야만 이용할 수 있다.<br>

<img src="/assets/create-repo.png" width="50%" height="50%">

## 지킬 테마 적용
원하는 테마를 다운로드 받고 repo와 연동된 local 디렉토리에 압축을 풀어준다. 필자가 사용할 테마는 minimal mistakes 이다.<br>

<center><img src="/assets/minimal-download.png" width="100%" height="100%"></center><br>

## 웹호스팅 테스트
아래 명령어 입력 후 http://127.0.0.1:4000 주소로 접속되는지 확인해보자. 만약 port를 바꾸고 싶다면 -p 를 추가해주면 된다.

```bash
$ bundle exec jekyll serve --trace &
or
$ jekyll serve -p 4000 --trace &
```

## Github 웹호스팅
github로 push 후  `https://${username}.github.io` 주소로 정상적으로 접속된다면 성공!<br>

<img src="/assets/success.jpeg" width="50%" height="50%">

## TMI
assets > css > main.scss 파일에서 문제가 생겼는데, .gitignore 파일에 vendor 라인을 삭제하니 정상 동작한다.