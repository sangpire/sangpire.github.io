---
title: socat 명령어
date: 2016/10/7
---

로컬 파일을 서버로 전송하려고 하니 `socat`이 가장 먼저 생각이 났습니다.
socat 은 Multipurpose relay (SOcket CAT) 이라고 합니다. '다목적 중계기' !?

원래 사용한적이 있던 명령어지만 또 까먹어서 사용 방법을 찾아봅니다.

우선 `tldr` 로 사용방법을 살펴봅니다.

```
$ tldr socat

socat

Multipurpose relay (SOcket CAT).

- Listen to a port, wait for an incoming connection and transfer data to STDIO:
    socat - TCP-LISTEN:8080,fork

- Create a connection to a host and port, transfer data in STDIO to connected host:
    socat - TCP4:www.domain.com:80

- Forward incoming data of a local port to another host and port:
    socat TCP-LISTEN:80,fork TCP4:www.domain.com:80
```

난 파일을 전송하고 싶은건데 큰 도움이 안되니, man 페이지를 살펴봅니다.

### 우선 OPTIONS 중

- `-u`

> Uses unidirectional mode. The first address is only used for reading, and the second address is only used for writing  (example).

단방향 모드, 첫번째 주소 -> 두번째 주소

- `-U`

> Uses unidirectional mode in reverse direction. The first address is only used for writing, and the second address  is  only  used for reading.

단방향 모드, 첫번째 주소 <- 두번째 주소

### 다음은, ADDRESS TYPES 중

주소를 작성하는 방식들 우선 '파일' 관련된 녀석들만 읽어봅니다.


#### 첫번째, `CREATE:<filename>` ADDRESS TYPE

> Opens `<filename>` with `creat()` and uses the file descriptor for writing.  This address type requires **write-only context**, because a file opened with creat cannot be read from.

쓰기 전용으로만 쓰일 수 있는 주소인가 봅니다.

> Flags  like  `O_LARGEFILE` cannot be applied. If you need them use `OPEN` with options `create,create`.

`create,create` 오타인가?..

> `<filename>` must be a valid existing or not existing  path. If `<filename>` is a named pipe, `creat()` might block; if `<filename>` refers to a socket, this is an error.

> - Option groups: `FD`,`REG`,`NAMED`
- Useful options: `mode`, `user`,  `group`,  `unlink-early`,  `unlink-late`, `append`
- See also: `OPEN`, `GOPEN`


#### 두번째, `GOPEN:<filename>` ADDRESS TYPE

> (Generic open) This address type tries to handle any file system entry except directories usefully. `<filename>` may be a relative or absolute path.
If it already exists, its type is checked. In case of a UNIX domain socket, socat connects; if connecting fails, socat assumes a datagram socket and uses `sendto()` calls.
If the entry is not a socket, socat opens it applying the `O_APPEND` flag.
If it does not exist, it is opened with flag `O_CREAT` as a regular file(example).
> - Option groups: `FD`,`REG`,`SOCKET`,`NAMED`,`OPEN`
- See also: `OPEN`, `CREATE`, `UNIX-CONNECT`

- directories 를 제외하고 일반적인 파일 시스템 엔트리(= 무엇을 의미하는 걸까요?) 에 유용하다고 하네요.
- `<filename>` 이 존재하면 뒤에 내용을 덧붙히고, 없으면 만들어주나 봅니다.


#### 세번째, `OPEN:<filename>` ADDRESS TYPE

> Opens `<filename>` using the `open()` system call (example). This operation fails on UNIX domain sockets.

> Note: This address type is rarly useful in bidirectional mode.
> - Option groups: `FD`,`REG`,`NAMED`,`OPEN`
- Useful  options:  `creat`,  `excl`, `noatime`, `nofollow`, `append`, `rdonly`, `wronly`, `lock`, `readbytes`, `ignoreeof`
- See also: `CREATE`, `GOPEN`, `UNIX-CONNECT`

- 오타 발견 'rarly'!!
- 양방향 모드에선 거의 유용하지 않다고 합니다.

## 결론

결과적으로 파일을 보내고자 하는 서버와 클라이언트 에서 실행한 명령어는 다음과 같습니다.

서버에서, `GOPEN`을 사용했지만, `OPEN` 을 써도 될 듯.
```
$ socat -u GOPEN:./source.file TCP:192.168.0.1:9999
```

클라이언트에서,
```
$ socat -u TCP-LISTEN:9999 CREATE:target.file
```
