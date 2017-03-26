---
title: Bash
date: 2013/8/11
tags: bash
---

트위터 지인의 리트윗 중, 관심있던 내용을 정리해 놓은 글 [Bash Configurations Demystified](http://dghubble.com/.bashprofile-.profile-and-.bashrc-conventions.html)을 봤습니다.

그 중 제 컴퓨터와 관련이 있는, *OS X* 에 관한 것만 살짝 살펴봤습니다.

## 블로그에서 살펴본 내용,

OS X 을 처음 설치하면, `.bashrc` 파일과, `.bash_profile` 파일이 존재합니다.

OS X 은 SSH로 PC에 접근하던, 터미널 프로그램을 실행하던, 기본적으로 로그인 쉘을 시작합니다.

OS X 은 기본적으로, `.bash_profile`을 먼저 실행합니다.
그리고, `.bash_profile` 에는 아래와 같은 내용이 포함되어 있습니다.

```
[[ -s ~/.bashrc ]] &amp;&amp; source ~/.bashrc
```

즉, `.bashrc`는 `.bash_profile`에 의해 실행된다는 이야기.

{% asset_img bash-config.png os x bash diagram %}

## 하지만,

위 내용으로 블로그에 요약을 해 놓으려고 확인 겸 새로운 계정을 생성해 보았더니, `.bash_profile` 도, `.bashrc` 도 없더군요. 제 짧은 영어 탓일 지도 모릅니다.

## 그래서 좀더 살펴보았습니다.

그래서 *Bash* 매뉴얼 페이지(man)를 살펴 봤습니다. 그 안에는 아래와 같은 내용이 있더군요.

> When bash is invoked as an interactive login shell, or as  a  non-interactive  shell  with  the  `--login` option,  it  first  reads  and executes commands from the file `/etc/profile`, if that file exists.  After reading that file, it looks for `~/.bash_profile`, `~/.bash_login`, and `~/.profile`, in that order, and reads and  executes  commands  from  the first one that exists and is readable.</blockquote>

## 해석해 보면,

Bash 가 대화형(=interactive) 로그인 쉘 이나 `--login` 옵션과 함께 비대화형(=interactive) 쉘로 로 생성되어질 때, 처음으로 `/etc/profile` 파일을 읽고 실행하게 됩니다.
 파일을 실행 한 다음 `~/.bash_profile`, `~/.bash_login` 그리고 `~/.profile` 파일 중 을 순서대로 파일을 찾아보고 그 중 <strong>처음으로 존재하고, 읽을 수 있는 파일만 읽고 실행하게 됩니다.</strong>

 위에 있는 파일을 하나씩 만들고 지우면서 실행해 보니 정말 메뉴얼 페이지 대로 동작하더군요.

```
Last login: Sun Aug 11 00:24:35 on ttys000
Hi This is .bash_profile
rosebook:~ sangpire$
```

그렇더라구요. :)
