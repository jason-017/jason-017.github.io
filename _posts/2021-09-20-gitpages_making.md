---
title: "Github pages로 블로그 만들기"
excerpt: "Github pages 간단 설명서"

categories:
 - github pages
tags:
 - github
 - pages
 - github pages
---

### Github 블로그 만들기 간단 설명서

#### 0. Github 블로그

Github 블로그라고 알려진 github pages라는 서비스가 존재한다.

Pages를 이용하기 위해서는 repo name을 'username.github.io' 형식으로 지어줘야 한다.



#### 1. github repository 생성

repository name을 username.github.io로 만들어준다.

저 같은 경우는 username이 jason-017이기 때문에, jason-017.github.io로 등록했다.

username 부분을 다른 걸로 써줘도 되지만, 오류가 나는 경우가 있다고 하니 웬만하면 지켜서 써주는 것을 추천한다.(갑자기 잘 되다가 안될 경우 오류 찾는데 쓸 데 없이 많은 시간을 보낼 가능성이...)



#### 2. remote repository

local 파일과 repo를 연동시켜주는 작업이다.

해당 작업을 통해 local에서 작업하고 commit만 시켜주면 된다.

```bash
cd jason-017.github.io
git init
git remote add origin https://github.com/jason-017/jason-017.github.io.git
git push origin master
```



#### 3. jekyll theme 적용

원하는 테마를 다운로드 받고 파일을 디렉토리에 복붙해주면 된다.

맥북의 경우 모든 겹치는 파일은 대치 처리해주면 된다.

제가 사용한 테마는 가장 언급이 많이 된다고 느낀 minimal mistake입니다.

[minimal mistake 깃허브 페이지](https://github.com/mmistakes/minimal-mistakes)

해당 페이지에서 'Code -> Download ZIP -> 압축해제 후 모든 파일 username.github.io 폴더에 복붙'



#### 4. test

```bash
bundle exec jekyll serve --trace
```

위 명령어를 통해 local 환경에서 웹호스팅을 테스트해볼 수 있다.

웹주소: http://127.0.0.1:4000에서 정상적으로 작동한다.



#### 5. Github 웹호스팅

Github repo > Settings > Github Pages 경로를 통해 들어가서 아래처럼 나오면 성공!

https://username.github.io 웹에 검색하면 아래 사진처럼 나옵니다!



사실 저는 local에서 잘 돌아갔는데 github pages에서 뭔지 모를 오류가 발생해서 구글링을 통해 해결했습니다... ㅎㅎ

(assets > css > main.scss 파일에서 문제가 생겼는데, **.gitignore** 파일에 vendor 라인을 삭제하니 정상적으로 작동하네요!)
