---
title: "[github pages] 동일 카테고리 내에서 이전, 다음 버튼 동작시키기"
excerpt: "github pages 구축"

categories:
 - github pages
tags:
 - github
 - pages
 - github pages
 - minimal mistakes
---
## layout, include, scss 간단하게 알아보기
minimal mistakes는 다양한 레이아웃을 제공하며 `_layouts`에서 확인할 수 있다. 레이아웃은 `_includes` 아래 파일들 조합으로 구성되며, `layout` 및 `_includes`의 디자인은 `_sass` 내 파일들이 처리한다.

이전, 다음 버튼 동작 및 디자인 변경을 위해서는 `post_pagination.html` 및 `_navigation.scss`만 수정해주면 된다. 레이아웃 구성은 single.html을 수정하여 적용했는데 각자 사용중인 레이아웃을 입맛에 맞게 수정하면 된다. 필자는 레이아웃 `footer`에 삽입되어 있던 `post_pagination.html`을 `section`에 재배치하였다.

필자의 경우 [[Github 블로그] 같은 카테고리 내에서의 이전글, 다음글 이동](https://ansohxxn.github.io/blog/prevnext/)에 작성된 `post_pagination.html`을 그대로 적용했으며 `_navigation.scss`는 살을 덧붙여 적용했다.

해당 포스팅은 minimal mistakes의 이전, 다음 버튼 동작 및 디자인을 변경하는 방법에 대한 글이다. 그저 이 포스팅을 보고 간단한 동작 및 디자인 변경을 직접 시도해보기를 기대하며 작성한다.

## 이전, 다음 버튼 동작 변경
minimal mistakes의 경우 기본적으로 포스팅 내 이전, 다음 버튼이 전체 포스팅의 시간 기준으로 동작한다. 결국 동일 카테고리 내에서 이전, 다음 버튼이 동작하도록 변경하기 위해서는 `post_pagination.html` 내용을 수정해줘야 한다. 즉, 커스터마이징을 해줘야 한다.

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
{% endraw %}
```

간단하게 버튼의 동작 방식을 설명하자면, 작성한 포스팅의 카테고리 내 포스팅 목록를 가져와서 이전, 다음 버튼에 포스팅 제목을 출력해주며 다음 글이 없는 경우 '가장 최신 글입니다.'를, 최초 작성된 글인 경우 '첫 번째 글입니다.'를 출력해준다.

![prev next first post](/assets/prev_next_first_post.png)

![prev next recent post](/assets/prev_next_recent_post.png)

![prev next post](/assets/prev_next_post.png)

## 이전, 다음 버튼 디자인 변경
_navigation.scss의 경우 minimal mistakes가 제공하는 기본 포멧은 유지하되 필요한 디자인 요소를 수정 및 삽입했다. 필자의 경우 _navigation.scss에 아래와 같은 커스터마이징 내용을 삽입했다.

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
    border: 1px solid mix(#48413a, $border-color, 80%);
    border-radius: $border-radius;

    .prev_next {
      font-family: $sans-serif;
      color: #b4a490;
    }

    &:hover {
      @include yiq-contrasted($muted-text-color);
    }

    // 이전 버튼에 대한 디자인
    &:first-child {
      border-bottom-left-radius: 0;
      border-bottom-right-radius: 0;
      border-bottom-color: transparent; // border 아래 부분 투명화
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
      border-bottom-color: transparent; // border 아래 부분 투명화 
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