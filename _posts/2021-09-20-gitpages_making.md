---
title: "[github pages] 깃허브 블로그 만들기"
excerpt: "github pages(깃허브 블로그) 구축"

categories:
 - github pages
tags:
 - github
 - pages
 - github pages
 - minimal mistakes
---
## Github 블로그
Github 블로그라고 알려진 github pages라는 서비스가 존재한다. 감사하게도 github에서 무료로 웹호스팅까지 지원해주는 블로그 서비스라고 생각하면 될 것 같다.
### github repository 생성
github pages를 이용하기 위해서 repository name을 `username.github.io`로 만들어준다.<br>
그리고 public, private을 선택할 수 있는데 github pages를 사용하기 위해서는 public으로 해야만 한다. 단어 뜻 그대로 공개할건지, 하지 않을건지 결정하는 사항이다.<br>

<img src="/assets/create-repo.png" width="50%" height="50%">
### remote repository
local 디렉토리와 repo를 연동시켜주는 작업이다. 해당 작업을 통해 local에서 작업할 수 있는 환경을 만든다.<br>
이를 통해 인터넷 사용이 불가능한 상황에서도 설정, 포스트 작성 등 모든 것을 할 수 있고 추후에 인터넷이 가능한 상황에 `git push`만 해주면 된다.

```bash
$ cd jason-017.github.io
$ git init
$ git remote add origin https://github.com/jason-017/jason-017.github.io.git
$ git push origin master
```

### 지킬 테마 적용
원하는 테마를 다운로드 받고 repo와 연동된 local 디렉토리에 압축을 풀어준다. 필자가 사용할 테마는 minimal mistakes라는 매우 유명한(아마도 가장 유명한...?) 테마이다.<br>
앞으로 github pages 관련 글은 모두 minimal mistakes 기준으로 작성할 예정이다. 해당 테마 또는 지킬 기반의 테마를 사용하는 유저들에게만 참고 글이 될 것 같다.<br>

<center><img src="/assets/minimal-download.png" width="100%" height="100%"></center><br>
[minimal mistake 깃허브 페이지](https://github.com/mmistakes/minimal-mistakes)
### 웹호스팅 테스트
아래 명령어를 통해 local 환경에서 웹호스팅을 테스트해볼 수 있다.<br>
http://127.0.0.1:4000 주소에 접속하여 정상적으로 웹이 뜨는지 확인하자.

```bash
$ bundle exec jekyll serve --trace
```

### Github 웹호스팅
`https://username.github.io` 주소로 접속하여 에러 없이 웹페이지가 뜨면 성공!<br>

<img src="/assets/success.jpeg" width="50%" height="50%">

### TMI
assets > css > main.scss 파일에서 문제가 생겼는데, .gitignore 파일에 vendor 라인을 삭제하니 정상 동작한다.
