---
title: "[github pages] 깃허브 블로그 이미지 넣기"
excerpt: "github pages에 이미지를 삽입해보자!"

categories:
 - github pages
tags:
 - github
 - pages
 - github pages
 - minimal mistakes
---
### 이미지 삽입하기
github pages(깃허브 블로그)를 사용하면서 가장 불편한 점을 고르자면 클립보드를 사용해서 이미지를 붙일 수 없다는 것이다.<br>
지금에야 조금 익숙해지고 구조를 대강 알게 되면서 어쩔 수 없다는 것을 이해하지만... 처음 시작할 때는... '와 이거 꽤나 불편하다'라고 느꼈다.<br>
주변 지인이나 구글링을 보면 이 때문에 타 블로그로 빠르게 이전하는 사람들도 꽤 볼 수 있는 것을 보면 나만 느낀 불편함은 아닌듯 하다.<br>
지금은 익숙하고 어떻게 보면 간단한 이미지 삽입 방법은 아래와 같다.
### github pages에 사진 넣는 방법
마크다운 형식을 따르므로 `![표시할 이미지명](이미지 경로)`로 삽입이 가능하다.<br>
html 형식 `<img src="/images/logo.png" width="50%" height="50%">`으로도 삽입 가능하다.
local PC에서는 절대 또는 상대 경로 중 편한 걸 사용하면 되고, github pages에 올릴 때는 repo를 root로 생각하고 절대 경로를 입력해주면 된다.<br>
필자의 경우 이미지는 assets 이라는 디렉토리에 담아두고 사용하고 있다. assets 디렉토리는 repo 바로 아래 위치한다.<br>
지금은 이미지 삽입이 텍스트 복붙 한방으로 가능하다는 생각에 편리한 것 같다는 느낌도 있지만 이것은 어디까지나 적응하여 편하다고 생각하는 것일 수도 있다.<br>
또 초기에는 이미지 확인하려고 local PC 경로를 입력하여 프리뷰 확인하고 경로를 gitub repo에 맞게 수정했는데 지금은 바로 repo 기준으로 작성하고 commit 하고 있다. 어차피 이미지 사이즈는 프리뷰 말고 웹에서 봐야 정확하기 때문이다.
- github pages용 경로
  ```![test](/assets/images/test.png)```

- local PC용 경로
  - 보통 posts 디렉토리 아래 있는 md들이 이미지를 참고하는 경우가 많기 때문에 상대경로를 ../ 시작했다.
  ```![test](../assets/images/test.png)```


