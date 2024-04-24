---
title: "[gitblog] 예제 "
categories: Gitblog
tag: [gitblog]
toc: true
author_profile: false
# sidebar:
#     nav: "docs"
---
# 타이틀

```
---
title: "[Gitblog] 예제 "	// 타이틀 
categories: Gitblog		// 카테고리
tag: [gitblog]			// 태그
toc: true			// (우측) 목차 (Table Of Content)
author_profile: false		// (좌측) 프로필 노출 여부
sidebar:			// (좌측) 사이드바 노출 여부
    nav: "docs"
---
```

# 공지사항

**[공지사항]** [공지사항 관련 URL](https://mmistakes.github.io/minimal-mistakes/docs/utility-classes/#notices)
{: .notice--info}

```html
**[공지사항]** 안됩니다!!
{: .notice--info}

또는 

<div class="notice--info">
  <h4>공지사항입니다.</h4>
  <ul>
    <li>공지사항1</li>
    <li>공지사항2</li>
    <li>공지사항3</li>
  </ul>
</div>
```

# 버튼

[버튼예시](https://mmistakes.github.io/minimal-mistakes/docs/utility-classes/#buttons){: .btn .btn--danger}

```html
[버튼예시](https://mmistakes.github.io/minimal-mistakes/docs/utility-classes/#buttons){: .btn .btn--danger}
```

# 유튜브

[유튜브 관련 링크](https://mmistakes.github.io/minimal-mistakes/docs/helpers/#responsive-video-embed)

{% include video id="q0P3TSoVNDM" provider="youtube" %}
