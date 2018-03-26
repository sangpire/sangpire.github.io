---
title: 
date: 2018-02-19 00:00:04
---

# Shell Commands

## awk

access log 중 응답 코드가 200 도 304도 302 도 아닌 로그 출력.

```sh
$ tail -f access.log | awk '{if ($9 !~ /200|304|302/) print $0}'
```

## git

git tag 삭제

```
git tag -d 12345
git push origin :refs/tags/12345
```
