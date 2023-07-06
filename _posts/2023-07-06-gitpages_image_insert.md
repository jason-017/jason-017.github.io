---
title: "[github pages] 깃허브 블로그 이미지 넣기"
excerpt: "github pages(깃허브 블로그) 구축"

categories:
 - github pages
tags:
 - github
 - pages
 - github pages
 - minimal mistakes
---
github pages(깃허브 블로그)를 사용하면서 가장 불편한 점을 고르자면 클립보드를 사용해서 이미지를 붙일 수 없다는 것이다.

지금에야 조금 익숙해지고 구조를 대강 알게 되면서 어쩔 수 없다는 것을 이해하지만... 처음 시작할 때는... '와 이거 꽤나 불편하다'라고 느꼈다.

주변 지인이나 구글링을 보면 이 때문에 타 블로그로 빠르게 이전하는 사람들도 꽤 볼 수 있는 것을 보면 나만 느낀 불편함은 아닌듯 하다.

지금은 익숙하고 어떻게 보면 간단한 이미지 삽입 방법은 아래와 같다.
### github pages에 사진 넣는 방법
마크다운, html 형식으로 삽입이 가능하다.
- 마크다운: `![표시할 이미지명](이미지 경로)`
- html: `<img src="/images/logo.png" width="50%" height="50%">`

local PC에서는 절대 또는 상대 경로 중 편한 걸 사용하면 되고, github pages에 올릴 때는 repo를 root로 생각하고 절대 경로를 입력해주면 된다.

필자의 경우 대부분의 이미지를 assets 디렉토리에 담아두고 사용하고 있다.

지금은 텍스트 복붙 한방으로 이미지 삽입이 가능하다는 생각에 편리한가 싶다가도, 사람이 적응하면 편하게 느껴진다고... 마치 매일 밤 느끼는 나의 빛번짐 마냥...

![빛번짐](/assets/빛번짐.png)

### TMI
초기에는 이미지 확인하려고 local PC 경로를 입력하여 프리뷰 확인하고 commit 전에 gitub repo에 맞게 수정했는데 지금은 바로 repo 기준으로 작성하고 commit 하고 있다. 어차피 이미지 사이즈는 프리뷰 말고 웹에서 봐야 정확하기 때문이다.
- github pages용 경로<br>`![test](/assets/images/test.png)`

- local PC용 경로<br>보통 posts 디렉토리 아래 있는 md들이 이미지를 참고하는 경우가 많기 때문에 상대경로를 ../ 시작했다.<br>`![test](../assets/images/test.png)`


