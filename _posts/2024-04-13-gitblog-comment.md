---
title:  "[gitblog] 댓글 기능 구현하기"
categories: Gitblog
tag: [gitblog]
toc: true
author_profile: false
# sidebar:
#     nav: "docs"
---
# 개요

깃 블로그에 댓글 기능을 추가해보자.

# Disqus 사용하기

jekyll에서는 댓글기능을 굉장히 다양하게 지원을 해주고 있는데 그 중에서 Disqus를 사용을 하려고 한다. 우선 아래의 disqus 사이트에 들어가서 회원가입을 진행한다.

[https://disqus.com/](https://disqus.com/)

프로필에 들어간 후 설정에서 'Add Disqus To Site'를 클릭한다.

![1](../../images/2024-04-13-gitblog-comment/1.png)

![2](../../images/2024-04-13-gitblog-comment/2.png)

Get Started를 클릭 후 'I want to install Disqus on my site'를 클릭한다.

![3](../../images/2024-04-13-gitblog-comment/3.png)

Website Name, Category, Language를 입력 후 'Create Site'버튼을 클릭한다. 그 후 아래 사진과 같이 무료 버전을 선택 후 플랫폼을 고르라는 화면에서 'Jekyll'을 선택한다.

![4](../../images/2024-04-13-gitblog-comment/4.png)

Jekyll install instructions 화면에서 'Configure'버튼을 클릭하면 아래와 같이 화면이 나오는데 Website URL에서 깃허브 블로그 URL을 입력 후 'Next'버튼을 클릭하여 세팅을 완료한다.![5](../../images/2024-04-13-gitblog-comment/5.png)

# _config.yml 파일 수정

_confgi.yml 파일에서 `comments: true` 로 수정해준다.

![6](../../images/2024-04-13-gitblog-comment/6.png)

comments의 provider는 'disqus'로 변경해주고, shortname도 변경해준다. shortname은 Disqus에서 만들었던 site에서 'Edit Setting'을 클릭하면 관련한 정보가 나온다.

![7](../../images/2024-04-13-gitblog-comment/7.png)

![8](../../images/2024-04-13-gitblog-comment/8.png)

# 배포하기

댓글기능이 되는지 확인하기 위해서는 배포를해야한다. 수정한 파일들을 github에 push를 하면 댓글기능이 활성화 된 것을 확인 할 수 있다.

![9](../../images/2024-04-13-gitblog-comment/9.png)

# 시행착오

댓글기능을 추가하면서 몇번의 시행착오가 있었다.

1. 잘못된 shortname입력
   * shortname을 정확히 입력하지 않아 안되는 경우가 있었다. 이를 방지하기 위해 disqus의 setting화면에서 정확히 확인하고 입력해야한다.
2. 포스팅 화면 layout 제거
   * 나의 시행착오는 아니었지만, 포스팅 시 화면에서  `layout : post `가 댓글 기능이 나타나지 않는 경우가 있었다고 한다. 이럴경우 layout을 지우고 포스팅하면 댓글 기능이 나타난다고 한다.
