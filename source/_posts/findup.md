---
title: 프로젝트, findup (진행중)
date: 2017-05-10 23:27:38
tags:
---

사진이 참 많아졌다. 백업하다 다시 가져오고 하다보니, 중복도 많아지고, 파일을 정리할 수 있는 프로그램을 개발해 보고 싶었다.

프로젝트 명 **find** (d)**up**(lications)
깃헙 주소: https://github.com/sangpire/findup

글을 쓰기로 한 현재 상태는..
특정 폴더(`File` 객체)를 인자로 넘겨주면, 해당 폴더의 파일을 모두 나열하고 있는 상태. 즉, 아래 함수 하나 정도 만들어져 있다.

```java
void lookupDir(Integer depth, File dir) {
    if (depth > 1) {
        return;
    }
    String margin = String.join("", Collections.nCopies(depth, "  "));
    File[] childFileList = dir.listFiles();
    if (childFileList == null) {
        String fileMetaInfos =String.format("%sEmpty!?\n", margin);
        System.out.print(fileMetaInfos);
        return;
    }
    for(File file: childFileList) {

        String fileMetaInfos =String.format("%s%s - %s\n", margin, file.isDirectory() ? "d" : "-" ,  file.getAbsolutePath());
        System.out.print(fileMetaInfos);
        if (file.exists() && file.isDirectory()) {
            lookupDir(depth + 1, file);
        }
    }
}
```

여기까지 - 2017. 5. 10

## 역할에 따른 코드 분리.

추가로 수정하고 싶은 부분
- 파일만 전문적으로 가져오는 부분을 만들어 보자.
- 실제로 뭔가를 할 녀석을 추상화 해서 만들어보자.
  - 메서드로는 다음과 같은게 필요하지 않을까?
    - (필터링) 이 파일 있는데 너 관심 있니?
    - (액션) 이 파일이 그 파일이야. 지지고 볶고 하삼.

적고보니,.. 뭔가 할 녀석은.
  - 이 파일 있어 필요하면 쓰고 관심 밖이면 안써도 괜찮아. 도 충분할 듯.

여기까지 해보자.
그 전에 우선 파일명도 좀 바꾸고, ~~테스트~~ 좀 더 쉽게 실행할 수 있도록 테스트 코드도 추가했다. [커밋](https://github.com/sangpire/findup/commit/7c55b3d4a1f77c708a8318bb3b329f4d742d425f)

적용 완료. [커밋](https://github.com/sangpire/findup/commit/8fc77f40186d3a9678e08dd2c6312408380b0c8e) - 2017. 5. 11
