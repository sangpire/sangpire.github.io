---
title: 
date: 2018-02-19 00:00:04
---

# Shell Commands

## nc

클라이언트의 파일을 서버로 업로드

```sh
SERVER$ nc -l -p 9999 > file.txt
CLIENT$ nc SERVER-IP-ADDRESS 9999 < file.txt
```

## uname

32bit 인지 64bit 알고자 할 때

```sh
$ uname -i
```

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
