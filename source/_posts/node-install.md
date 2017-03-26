---
layout: post
title: nvm 으로 node 설치하기.
date: 2013/8/11
---

**ruby** 에 **rvm** 이 있고, **perl** 에 **perlbrew** 가 있는 것 처럼 **node** 에도 **[nvm](https://github.com/creationix/nvm)** 이 있더군요.

우선 홈페이지에 설명대로, 설치 스크립트를 실행하였습니다.

```sh
curl https://raw.github.com/creationix/nvm/master/install.sh | sh
```

설치하고 나면, `.bash_profile` 에 다음과 같은 명령이 추가된 것을 확인할 수 있습니다.

```sh
[[ -s /Users/sangpire/.nvm/nvm.sh ]] && . /Users/sangpire/.nvm/nvm.sh # This loads NVM
```

그리고, 설정 파일 적용을 위해, 터미널을 재시작

# 본격적인 nvm 실행.

우선 **nvm** 명령을 실행하면, 다음과 같은 도움말이 나타납니다.

```sh
rosebook:Vibration sangpire$ nvm

Node Version Manager

Usage:
    nvm help                    Show this message
    nvm install [-s] <version>  Download and install a <version>
    nvm uninstall <version>     Uninstall a version
    nvm use <version>           Modify PATH to use <version>
    nvm run <version> [<args>]  Run <version> with <args> as arguments
    nvm ls                      List installed versions
    nvm ls <version>            List versions matching a given description
    nvm ls-remote               List remote versions available for install
    nvm deactivate              Undo effects of NVM on current shell
    nvm alias [<pattern>]       Show all aliases beginning with <pattern>
    nvm alias <name> <version>  Set an alias named <name> pointing to <version>
    nvm unalias <name>          Deletes the alias named <name>
    nvm copy-packages <version> Install global NPM packages contained in <version> to current version

Example:
    nvm install v0.4.12         Install a specific version number
    nvm use 0.2                 Use the latest available 0.2.x release
    nvm run 0.4.12 myApp.js     Run myApp.js using node v0.4.12
    nvm alias default 0.4       Auto use the latest installed v0.4.x version

rosebook:Vibration sangpire$
```

이 명령중, `ls-remote` 로 설치 가능한 버전을 확인하고, `install` 로 설치하면 완료.

```sh
nvm install 0.10
```

현재 `ls-remote` 명령을 실행하면, _v0.11.5_ 까지 나타나지만, 설치해서 **npm** 으로 모듈을 설치하려 하니 오류가 좀 있어서, _v0.10_ 을 설치 했습니다. (홈페이지에도, 현재 버전이 _v0.10.15_ 라고 나오더군요.)

설치 하고, _v0.10.x_ 를 사용하겠다고 아래와 같이 입력하면 설정 완료.

```sh
nvm use 0.10
```

설명 마지막에 있는 것처럼 `alias` 도 설정해 보고,

```sh
nvm alias default 0.11
```

재미난 모듈들을 설치해보면 끝!.
