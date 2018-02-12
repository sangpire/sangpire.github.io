---
title: How to obtain a current user locale
date: 2016-05-16
tags: java spring locale
---

처음에는 `ServletRequest` 에서 가져왔다.
<!--
```java
Locale locale = request.getLocale();
```

그런데 이 녀석이, `LocaleResolver` 빈이 설정한 값을 제대로 가져오지 못하더라는;

아래 스택오버플로우 답변처럼 하니 잘 됨.

```java
Locale locale = LocaleContextHolder.getLocale();
```

쓰레드 로컬에서 가져온거라고 하는데,.. 쓰레드 로컬에 대해서도 좀 알아봐야겠다.

[Stackoverflow](http://stackoverflow.com/a/16106304/693195) -->
