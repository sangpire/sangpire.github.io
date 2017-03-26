---
title:  When proxying is enabled you need to use interfaces, not implementations.
date:   2016-05-15
tags: java spring @Autowired @Transactional
---

`@Service` 빈을, `WebSecurityConfigurerAdapter` 을 상속한 코드에서 `@Autowired` 하고 있는 상황이였음.

`@Service` 빈에 한 메서드에 `@Transactional` 을 추가하자 `@Autowired` 가 실패하기 시작함. 인터넷 검색해 보니, 스택오버플로에서 처리 방법을 알게 됨.

> When proxying is enabled you need to use interfaces, not implementations.

 즉, Proxying 으로 동작하게 되면, **interface** 를 써야지, **implementation** 을 쓰면 안된다는 말,

 코드를 수정해 보니 우선 잘 된다. 음 자세한건 좀 더 찾아봐야지...

- [Stackoverflow](http://stackoverflow.com/a/32579339/693195)
