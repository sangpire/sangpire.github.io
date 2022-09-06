---
layout: drafts
title: About Perl
date: 2013-08-10 00:00:01
tags:
---

Links
=====

About Perl
----------
- [펄로 파폭을 제어하는 모듈](https://metacpan.org/module/WWW::Mechanize::Firefox)
- [mojocasts](http://mojocasts.com/)
- [Perl 5.18 릴리즈](http://aero2blog.blogspot.kr/2013/05/perl-5180.html)
- [딸기 펄](http://strawberryperl.com/)
- [펄 불린에 대한 정리](http://ko.perlmaven.com/boolean-values-in-perl)
- [현승님 샘플 코드들](https://gist.github.com/sng2c)
- [유닛테스트 작성법](http://sng2c.tumblr.com/post/51435547761/tpt-2013-03-27)
- [마플 봇 관련](https://metacpan.org/release/KHS/Net-MyPeople-Bot-IPUpdator-0.001)

DB 관련
------
- [Postgresapp](http://postgresapp.com)
- Maria DB
- DBIx::Class


그리고 이야기
----------

### $my 스코프 에 관하여.
`my` 는 스코프가 블럭 단위라서 아래와 같이 사용하면, 문제가 생깁니다.

```
for(my $i =0 ; $i<10; $i++){
}
print $i;
```

### cpanm 설치 관련

`.bashrc` 에 아래 코드 추가

	cpanm --local-lib=~/perl5 local::lib && eval $(perl -I ~/perl5/lib/perl5/ -Mlocal::lib)

그냥 소스에 아래 줄을 추가하면 로컬 `~/perl5` 를 보개 됨

	use local::lib;

- [관련 링크](http://search.cpan.org/dist/local-lib/lib/local/lib.pm)

그리고 `cpanm` 은 `/usr/lib` 에 접근권한이 없으면 자동으로 `~/perl5` 에 설치를 하구요

쉘에서 `perl -Mlocal::lib` 하면 환경변수가 출력되요
`perl -Mlocal::lib >> ~/.bashrc` 로 추가해주면
한방에 되죠 그러면 use local::lib 안해도 됨


### 펄 변수값을 Dump 할때 유용한 팁.

아래 각 모듈들은 펄 변수의 값을 dump해볼때 유용합니다.

- `Data::Dumper` : 그냥 보통
- `JSON::encode_json` : json 형태 
- `YAML::Dump` : `yaml` 형태. 위 두개보다 알아보기 쉽고, `Moose` 등으로 만든 클래스들은 `un-serialize` 도 가능.( `new()` 에서 `property` 를 받아 처리할 수 있으면) 
- `Data::Printer` : Full Color 덤프라서 굉장히 직관적.



### 모듈 Moose

객체지향 관련 모듈로 기억(확인 필요)

사용 예

- https://metacpan.org/source/KHS/Template-Reverse-0.04
- https://metacpan.org/source/KHS/Gearman-SlotManager-0.3/lib/Gearman/SlotWorker.pm
- https://github.com/aero/perl_docs/blob/master/hatena_perl_oop.md


### curl 로 데이터를 가져와서 5번째 줄을 출력하고자 할때.

#### 코드 1 by 현승님.

	curl 'URL' | perl -e '@line = <STDIN>; print $line[4]'

####  코드 2 by 현승님.

	curl 'URL' | perl -e 'print [<STDIN>]->[4]'

####  코드 3 by 승희님.

	my $batchid= `curl -s $url | head -5 | tail -1 `;

####  코드 4 by 현승님

	curl 'URL' | perl -ne '$i++==4 and print $_'

####  코드 5 by 현승님

	chomp( $batchid = [`curl $url`]->[4] );

	- 줄바꿈 때주는 코드

####  코드 6 by 현승님

	chomp( my $batchid = [`curl $url`]->[4] );

####  코드 7 by 현승님

	use LWP::Simple;
	my $batchid = [split(/\n/,get($URL))]->[4];

- curl이 없는 윈도우에서 실행 가능
- 근데 5.8.8에는 `LWP::Simple` 이 기본모듈이 아닌거 같아서 curl 을 쓰는게 더 낳아 보임
- `split(/\n/)` 로 나누면 구분자가 포함되지 않으므로 `chomp` 를 사용하지 안하도 됨.

#### 코드 8 기본 입력이 너무 긴 경우 by 현승님

`while(<STDIN>){` 로 한줄씩 읽게 해서 4밴째 줄을 찍음 되겠군요. 볼일 다보면 `last` (다른데서는 `break` ) 때려주면 됨.

#### 코드 9 딱 5째 줄만 처리한다면,

이런 코드도 가능.

```
my $line = <STDIN>;
$line = <STDIN>;
$line = <STDIN>;
$line = <STDIN>;
$line = <STDIN>;
```

#### 기타

- `명령` 을 _$스칼라_ 에 넣으면 단일 스트링으로, _@배열_ 에 넣으면 한줄씩 배열에 들어가지만, `\n` 이 포함되어있습니다.
