---
layout: post
title: "Online Schema Change"
categories: posts
excerpt: "Online Schema Change"
tags: [database, mysql, online schema change]
author: myungseokang
comments: true
share: true
ads: true
date: 2019-08-26
---

> 이 글은 MySQL 기준으로 작성되었습니다.

### Online Schema Change 가 뭐야?

**Online Schema Change (이하 OSC)** 는 말 그대로 Online 온라인 상태에서 (중단없이) Schema Change 스키마를 변경하는 것을 의미합니다.

이게 뭔지를 알려면 아래의 왜 필요한지부터 알면 좋을 것 같아서 그 다음에 설명해보도록 하곘습니다.

### Online Schema Change 가 왜 필요해?

OSC 가 왜 필요한지 설명하려면 기존의 MySQL `ALTER TABLE` 의 방식에 대해 알아야해서 잠깐! 짚고 넘어가 보겠습니다.

보통의 `ALTER TABLE` 을 하게 되면 아래와 같이 동작하게 됩니다.

1. 해당 테이블을 `TL_WRITE_ALLOW_READ` 로 잠급니다. 즉, `READ` 는 허용하고 `WRITE` 를 차단합니다.
2. 새로운 테이블의 빈 임시 테이블을 만들고,
3. 이전 테이블에서 새 테이블에 데이터를 복사하고,
4. 새 테이블을 이전 테이블과 같은 이름으로 바꾸고 이전 테이블은 삭제합니다.
5. 새로운 테이블에 차단했던 `WRITE` 를 풉니다.

따라서 위 과정을 온라인 상에서 진행하게 되면 1번 작업 이후 `WRITE` 작업이 있을 경우, 락이 조금만 오래 잡혀있어도 타임아웃 에러가 발생할 겁니다.

`READ`, `WRITE` 작업이 빈번한 WAS 에서는 이러한 중단에 민감할 수 밖에 없습니다.

그래서 이러한 문제를 해결하기 위해 고안된 것이 OSC 방법입니다.

### Online Schema Change 는 어떻게 하는데?

그렇다면 OSC 를 하는 방식이 굼금하실 것 같습니다.

기존 MySQL 의 `ALTER` 방식의 프로세스를 수작업으로 스크립트화 시켜서 해결했습니다.

`WRITE` 락 거는 부분을 제거하고, 그 기능을 Trigger 를 사용해서 원본 테이블의 `READ`, `WRITE` 를 가능하게 했습니다.

> GitHub 에서 사용하는 OSC 도구 <a href="https://github.com/github/gh-ost" target="_blank">gh-ost</a> 에서는 Trigger 를 사용하지 않는다고 합니다.
> 자세한 이야기는 뒤에서!

### Online Schema Change 쉽게 못 쓰나?

당연히 다른 분들이 먼저 고민해보고, 사용할 수 있게 구현해놓은 것들이 있습니다.

유명한 OSC 도구들을 몇 개 소개해볼까 합니다.

1. <a href="#pt-online-schema-change">pt-online-schema-change</a> in Percona Toolkit @ Percona
2. <a href="#onlineschemachange">OnlineSchemaChange</a> @ Facebook
3. <a href="#gh-ost">gh-ost</a> @ GitHub
4. <a href="https://github.com/soundcloud/lhm" target="_blank">LHM</a> @ SoundCloud
5. <a href="https://github.com/shlomi-noach/openarkkit" target="_blank">oak-online-alter-table</a> in OpenArkKit @ Google

아래 4번과 5번은 구현된지 오래 되기도 하였고, `LHM` 은 루비 기반 `oak-online-alter-table` 은 Python2 기반이라 제대로 알아보지는 않았습니다.

중점적으로는 위의 1, 2, 3번을 주로 찾아보았고, 특징들을 가볍게 정리해볼까 합니다.


#### pt-online-schema-change

> <a href="https://www.percona.com/doc/percona-toolkit/LATEST/pt-online-schema-change.html" target="_blank">pt-online-schema-change 공식 페이지</a>

먼저 `pt-online-schema-change`. 이걸 사용하기 위해서는 <a href="https://www.percona.com/doc/percona-toolkit/LATEST/index.html" target="_blank">Percona Toolkit</a>이 설치되어 있어야 합니다.

`Toolkit` 이라는 이름에 걸맞게 `pt-online-schema-change` 말고도 엄청나게 다양한 기능을 사용할 수 있습니다.

사용하게 되면 이것저것 사용해볼 수 있을 것 같아서 편할 것 같지만, 그만큼 무거울 것 같습니다.

처음 MySQL 에서 무중단 ALTER 를 검색하다보니 제일 먼저 보인 게 이 `pt-online-schema-change` 였습니다.

그만큼 레퍼런스도 많을 것 같습니다.

#### OnlineSchemaChange

> <a href="https://engineering.fb.com/production-engineering/onlineschemachange-rebuilt-in-python/" target="_blank">OnlineSchemaChange 소개 페이지</a>

<img src="https://engineering.fb.com/wp-content/uploads/2017/05/GLhAFgHwlR-THYQBAAAAAAAAmTEibj0JAAAB.jpg" alt="Facebook OSC flow image">

보다보니 `Facebook OSC` 를 제일 심도있게 보게 되었는데, 이유는 Python 기반 & 잘 되어있는 <a href="https://github.com/facebookincubator/OnlineSchemaChange/wiki" target="_blank">문서</a> 덕분인 것 같습니다.

Facebook OSC 의 작동 순서 및 방식에 관련된 글을 따로 작성할 것 같아 길게 적진 않겠습니다.

#### gh-ost

> <a href="https://github.com/github/gh-ost" target="_blank">GitHub 주소</a>

<img src="https://github.com/github/gh-ost/raw/master/doc/images/gh-ost-general-flow.png" alt="GitHub gh-ost flow image"/>

다른 솔루션들은 전부 Trigger 로 문제점을 해결했는데, 유일하게 `gh-ost` 만 Triggerless Design 을 사용해서 제일 흥미로웠습니다.

gh-ost 개발진은 <a href="https://github.com/github/gh-ost/blob/master/doc/why-triggerless.md" target="_blank">트리거가 제약이 많고 위험부담이 있다고 말하고 있습니다.</a>

그래서 Trigger 대신 binary log stream 을 이용해서 변경 사항을 감지하고, 비동기적으로 고스트 테이블에 적용한다고 합니다.

하지만, FK 지원이 되지 않고, <a href="https://github.com/github/gh-ost/blob/master/doc/requirements-and-limitations.md#limitations" target="_blank">일부 제약사항</a> 때문에 프로덕션에서는 일부분만 적용해볼 수 있을 것 같습니다.

Golang 으로 작성되었고, 역시나 문서가 엄청 잘 되어있어 보기 좋았습니다.

---

이상 글을 마치겠습니다.

읽어주셔서 감사합니다 :)
