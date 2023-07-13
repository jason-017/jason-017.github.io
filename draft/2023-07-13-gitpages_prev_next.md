---
title: "[github pages] 이전, 다음 버튼을 동일 카테고리 내에서 이동하게 만들기"
excerpt: "github pages 구축"

categories:
 - github pages
tags:
 - github
 - pages
 - github pages
 - minimal mistakes
---
minimal mistakes는 다양한 레이아웃을 제공하며 `_layouts > html`을 통해 확인할 수 있다. 레이아웃은 `_includes > html` 조합 기반으로 구성되며, `layout` 및 `_includes`의 디자인은 `_sass > scss`에서 처리한다.

먼저 필자는 single.html 레이아웃을 이용하여 글을 포스팅하고 있어 single.html을 탐색해 봤는데, `_includes > post_pagination.html`을 통해 이전, 다음 버튼이 만들어지는 것을 확인할 수 있었다. 또한, post_pagination.html은 `_navigation.scss`을 통해 디자인되고 있는 것도 확인했다. 이를 통해 알 수 있듯이 이전, 다음 버튼 동작 방식 및 디자인 변경을 위해 `post_pagination.html` 및 `_navigation.scss`를 수정하면 된다. 레이아웃 구성은 single.html을 수정하여 적용했는데 각자 사용중인 레이아웃을 수정하여 적용하면 된다. 필자는 레이아웃 footer에 삽입된 `post_pagination.html` 코드를 section에 재배치하였다.

필자의 경우 `공부하는 식빵맘`님이 [[Github 블로그] 같은 카테고리 내에서의 이전글, 다음글 이동](https://ansohxxn.github.io/blog/prevnext/)에 작성해주신 post_pagination.html을 그대로 적용했으며 _navigation.scss는 살을 덧붙여 적용했다.

해당 포스팅은 html 및 scss를 작성하는 법을 설명하는 것이 아니다. 당연히 내가 작성한 코드가 아니기도 하고 잘 알지도 못하기 때문이다. 그저 minimal mistakes의 이전, 다음 버튼 동작 및 디자인을 변경하는 방법에 대해 기술한 것임을 강조한다. 그저 이 포스팅을 보고 간단한 동작 및 디자인 변경을 직접 시도해보기를 기대하며 작성한다.

### 이전, 다음 버튼 동작 변경
minimal mistakes는 전체 포스팅을 시간 기준으로 이전, 다음 버튼이 동작하는 게 default이다. 때문에 동일한 카테고리 내 포스팅을 이전, 다음 버튼을 이용하여 연속적으로 보는 것이 현실적으로 불가능하다고 볼 수 있다. 이를 위해 post_pagination.html 내용을 동일 카테고리 내에서 이전, 다음 버튼이 동작하도록 변경했다.

간단하게 설명하자면, 작성한 포스팅의 카테고리 내 포스팅 목록를 가져와서 이전, 다음 버튼에 포스팅 제목을 출력해주며 다음 글이 없는 경우 '가장 최신 글입니다.'를, 최초 작성된 글인 경우 '첫 번째 글입니다.'가 출력된다.

```html
{% raw %}
{% assign cat = page.categories[0] %}
{% assign cat_list = site.categories[cat] %}
{% for post in cat_list %}
  {% if post.url == page.url %}
  	{% assign prevIndex = forloop.index0 | minus: 1 %}
  	{% assign nextIndex = forloop.index0 | plus: 1 %}
  	{% if forloop.first == false %}
  	  {% assign next_post = cat_list[prevIndex] %}
  	{% endif %}
  	{% if forloop.last == false %}
  	  {% assign prev_post = cat_list[nextIndex] %}
  	{% endif %}
  	{% break %}
  {% endif %}
{% endfor %}

{% if prev_post or next_post %}
  <nav class="pagination_prev_next">
    {% if prev_post %}
      <a href="{{ prev_post.url }}" class="pagination_prev_next--pager"><span class="prev_next">이전 글  &nbsp</span>{{ prev_post.title }}</a>
    {% else %}
      <a href="#" class="pagination_prev_next--pager disabled-first-child">첫 번째 글입니다.</a>
    {% endif %}
    {% if next_post %}
      <a href="{{ next_post.url }}" class="pagination_prev_next--pager"><span class="prev_next">다음 글  &nbsp  </span>{{ next_post.title }}</a>
    {% else %}
      <a href="#" class="pagination_prev_next--pager disabled-last-child ">가장 최근 글입니다.</a>
    {% endif %}
  </nav>
{% else %}
  <a href="#" class="pagination_prev_next--pager disabled-first-child">첫 번째 글입니다.</a>
  <a href="#" class="pagination_prev_next--pager disabled-last-child ">가장 최근 글입니다.</a>
{% endif %}
```

### 버튼 디자인 커스터마이징
_navigation.scss에 아래와 같은 커스터마이징 내용을 삽입하면 된다. 개인 웹 디자인 컨셉에 맞게 수정하여 적용해보자.

```scss
.pagination_prev_next {
  @include clearfix();
  float: left;
  width: 100%;
  
  /* next/previous buttons */
  &--pager {
    margin-top: 0em;
    padding-top: 0em;
    display: block;
    padding: 1em 2em;
    width: 100%;
    font-family: $sans-serif;
    font-size: $type-size-5;
    font-weight: bold;
    text-align: center;
    text-decoration: none;
    color: $muted-text-color;
    border: 1.5px solid mix(#48413a, $border-color, 80%); // 컨셉 유지를 위한 color mix
    border-radius: $border-radius;

    .prev_next {
      font-family: $sans-serif;
      color: #b4a490; // 폰트 색상 설정
    }

    &:hover {
      @include yiq-contrasted($muted-text-color);
    }

    // 이전 버튼에 대한 디자인
    &:first-child {
      border-bottom-left-radius: 0;
      border-bottom-right-radius: 0;
      border-bottom-color: transparent; // border선border 아래 부분 투명화
    }

    // 다음 버튼에 대한 디자인
    &:last-child {
      margin-bottom: 1em;
      border-top-left-radius: 0;
      border-top-right-radius: 0;
    }

    // 기존 disabled 제거 후 first-child, last-child로 나눔.
    &.disabled-first-child {
      border-bottom-left-radius: 0;
      border-bottom-right-radius: 0;
      border-bottom-color: transparent; // 변경, border 아래 부분 투명화 
      color: rgba($muted-text-color, 0.5);
      pointer-events: none;
      cursor: not-allowed;
    }

    &.disabled-last-child {
      margin-bottom: 1em;
      border-top-left-radius: 0;
      border-top-right-radius: 0;
      color: rgba($muted-text-color, 0.5);
      pointer-events: none;
      cursor: not-allowed;
    }
  }
}
```