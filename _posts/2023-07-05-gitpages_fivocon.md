---
title: "[github pages] favicon 적용하기"
excerpt: "github pages(깃허브 블로그) 구축 가이드"

categories:
 - github pages
tags:
 - github
 - pages
 - github pages
---

### github pages(깃허브 블로그)에 favicon 적용하기
favicon이라 함은 브라우저의 주소창에 표시되는 웹페이지를 대표하는 이미지이다. favorites와 icon의 합성어이다.

아래와 같이 동그라미 표시한 네이버와 다음 아이콘을 파비콘이라고 한다.

![favicon example](/assets/favicon_ex.png)

필자의 경우 minimal mistakes를 사용하고 있긴 하나 지킬 기반일 경우 적용 방법은 비슷할 것으로 보인다.

적용 방법도 매우 간단하다.

파비콘 생성부터 github pages 적용까지 빠르게 진행해보자.

1. 이미지를 파비콘으로 만들기

   파비콘의 경우 일반적으로 자신 또는 기업을 잘 나타내는 이미지를 사용하지만 그냥 좋아하는 이미지를 사용해도 무방하다. 저작권만 조심하자! [해당 링크](https://www.favicon-generator.org)에서 이미지를 업로드하여 파비콘 만들어준다.

   아래와 같이 다운로드 링크와 함께 html 코드가 같이 나타나면 정상적으로 변경된 것이다. 'Download the generated favicon'을 클릭하면 실제 파비콘 파일들이 다운로드된다. 다운로드 받은 압축 파일을 assets 폴더에 풀어준다.

    ![favicon create](/assets/favicon_create.png)

2. 파비콘 적용하기

   _includes > head > custom.html에 아까 생성된 html 코드를 넣어주면 바로 적용이 가능하다.
    ```bash
    $ vi _includes/head/custom.html
    <!-- start custom head snippets -->
    <link rel="apple-touch-icon" sizes="57x57" href="/assets/favicon.ico/apple-icon-57x57.png">
    <link rel="apple-touch-icon" sizes="60x60" href="/assets/favicon.ico/apple-icon-60x60.png">
    <link rel="apple-touch-icon" sizes="72x72" href="/assets/favicon.ico/apple-icon-72x72.png">
    <link rel="apple-touch-icon" sizes="76x76" href="/assets/favicon.ico/apple-icon-76x76.png">
    <link rel="apple-touch-icon" sizes="114x114" href="/assets/favicon.ico/apple-icon-114x114.png">
    <link rel="apple-touch-icon" sizes="120x120" href="/assets/favicon.ico/apple-icon-120x120.png">
    <link rel="apple-touch-icon" sizes="144x144" href="/assets/favicon.ico/apple-icon-144x144.png">
    <link rel="apple-touch-icon" sizes="152x152" href="/assets/favicon.ico/apple-icon-152x152.png">
    <link rel="apple-touch-icon" sizes="180x180" href="/assets/favicon.ico/apple-icon-180x180.png">
    <link rel="icon" type="image/png" sizes="192x192" href="/assets/favicon.ico/android-icon-192x192.png?">
    <link rel="icon" type="image/png" sizes="32x32" href="/assets/favicon.ico/favicon-32x32.png?">
    <link rel="icon" type="image/png" sizes="96x96" href="/assets/favicon.ico/favicon-96x96.png?">
    <link rel="icon" type="image/png" sizes="16x16" href="/assets/favicon.ico/favicon-16x16.png?">
    <link rel="manifest" href="/assets/favicon.ico/manifest.json">
    <meta name="msapplication-TileColor" content="#ffffff">
    <meta name="msapplication-TileImage" content="/assets/favicon.ico/ms-icon-144x144.png">
    <meta name="theme-color" content="#ffffff">
    <!-- end custom head snippets -->
    ```

3. git push
    git push를 위해선 당연히 로그인 되어 있어야 한다. 이미 github pages를 구축하신 분들은 로그인 과정을 거쳤다고 생각하기에 생략한다.
    ```bash
    $ git add ./ 
    $ git commit -m 'commit msg'
    $ git push 
    ```
    
