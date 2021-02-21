---
layout: post
title: "pt-online-schema-change 1년 사용 후기 (절망편)"
categories: posts
excerpt: "Review of pt-online-schema-change in last year (despair part)"
tags: [database, mysql, online schema change]
author: myungseokang
comments: true
share: true
ads: true
date: 2021-02-21
---

> 이 글은 MySQL 기준으로 작성되었습니다.

일전에 online schema change (이하 OSC) 에 대해서 다룬 포스팅을 올렸었는데요.

이번에는 그 때 소개했던 툴 중 하나를 프로덕션 환경에서 1년 정도 실 사용하면서 겪었던 경험을 정리해보고자 합니다.

첫 도입 당시에는 되게 만족하면서 사용했다가 최근 여러가지 문제점으로 인해 다른 방식을 고민하게 되었는데, 문제점들을 하나씩 다뤄보겠습니다.

![percona toolkit](https://www.google.com/url?sa=i&url=https%3A%2F%2Fhoing.io%2Farchives%2Ftag%2Fpercona-pt-online-schema-change&psig=AOvVaw2w89TVH9FYbDTmzRoztFOq&ust=1613999568299000&source=images&cd=vfe&ved=0CAIQjRxqFwoTCKjR6omH--4CFQAAAAAdAAAAABAK)

### 첫 도입, 신문물을 접하다

처음 여러 OSC 툴을 접하고, 리서치하면서 아니 이런 툴을 왜 그동안 안 쓰고 있었지? 하면서 정리해봐야겠다! 라고 말한 뒤, 작성한 포스팅이 <a href="/posts/online-schema-change/" target="_blank">이 포스팅</a> 이었습니다.

아무래도 처음 OSC 를 접한 뒤, 저의 반응은 마치 불을 처음 접한 원시인 같았습니다. 실제 프로덕션 환경에서 사용하면서 너무나 만족스러웠던 경험이 이어졌고, 신봉하는 수준까지 갔었습니다.

왜냐면, 기존에는 새벽에 출근해서 해당 테이블에 write 하는 액션을 모두 막고 진행했어야 했기 때문에 굉장히 귀찮고 신경써야할 부분이 많았습니다.

그런데 OSC 를 사용하면서부터는 그런 부분에 신경을 쓰지 않고, 명령어 한줄 잘 써서 실행하면 너무나도 잘 되었기 때문에 아주 만족하는 나날을 보냈습니다.

하지만 그 시간은 얼마 가지 않았고... 이런저런 문제점이 발견되기 시작했습니다.


### 어라...? 이게 아닌데...

여러 문제점들이 나왔는데, 차례대로 정리해볼게요.

1. 테이블 크기가 크면 클수록 dump 시간이 길어진다.

처음은 너무나 당연한 문제점입니다.

OSC 작동 방식 자체가 변경하려는 테이블에서 스키마를 변경한 임시 테이블을 따로 만들고, 원래 테이블 -> 새로운 테이블로 트리거를 걸고 원래 테이블 데이터를 dump 떠서 스키마를 변경한 테이블에 load 하는 방식이기 때문에 relation 테이블 (중간 테이블) 같이 row 수가 급격하게 많아지는 테이블의 경우, 너무나 당연하게 시간이 엄청나게 오래 걸리게 됩니다.

그래서 제가 실행하진 않았지만 팀에서 진행한 OSC 작업 중에서 제일 오래 걸렸던 건 4시간이 넘어가기도 했던 걸로 보았습니다...

2. drop swap 방식을 사용할 경우, 잠깐이긴 하지만 `XXX.xxx table doesn't exists` 오류가 발생한다.

이는 <a href="https://www.percona.com/doc/percona-toolkit/LATEST/pt-online-schema-change.html" target="_blank">pt-online-schema-change 공식 문서</a>에서도 다루고 있는 내용입니다.

짧게 설명하면 pt-online-schema-change 에서 foreign key 가 걸려있는 테이블의 스키마를 변경할 때, `drop_swap` 방식 혹은 `rebuild_constraints` 방식이 있습니다.

`drop_swap` 방식으로 실행될 경우, 원래 테이블을 drop 하고 새로운 테이블이 rename 되기 전의 아주 잠깐의 시간동안 원래 이름에 해당하는 테이블이 존재하지 않아서 위와 같은 오류가 발생하게 됩니다.

3. lock 을 잡는 액션이 많은 경우, 트리거를 생성/삭제할 때 데드락이 발생한다.

이 부분이 최근에 머리를 아프게 한 주범입니다.

트리거를 생성/삭제할 때, 먼저 연결되어있던 connection 이 끊어지는 문제도 있었는데, 이 부분은 제껴두고 insert/update 트리거를 만들거나 지울 때, 데드락 오류가 엄청 나더라구요.

실제로 다른 곳에서 lock 을 잡는 경우에 OSC 에서 트리거를 생성/삭제하는 곳과 동시에 잡게 되어 데드락이 발생하게 되어서, 장애가 있었던 적이 종종 있습니다.

이 부분 때문에 아래와 같은 고민을 시작하게 되었어요.


### 다시 고민

다시 돌아와서, 위 문제점들을 어떻게 보완할 수는 없을까를 고민하고 있습니다.

예전 <a href="/posts/online-schema-change/" target="_blank">OSC 포스팅</a>에서 다뤘던 툴 중 GitHub 에서 만든 <a href="https://github.com/github/gh-ost" target="_blank">gh-ost</a> 라는 게 있습니다.

이 툴이 다른 툴들과 가장 달랐던 점 중 하나가 <a href="https://github.com/github/gh-ost/blob/master/doc/triggerless-design.md" target="_blank">Triggerless Design</a> 을 사용한 점인데요.

하지만 foreign key 는 지원이 안된다고 했던 한계점 때문에 pt-online-schema-change 를 사용하게 되었는데요.

위에서 얘기했던 트리거의 한계점 때문에 앞으로는 gh-ost 의 작동 방식을 고민해보고, 다음 포스팅에서는 작동 방식을 자세하게 정리해보려고 합니다.

읽어주셔서 감사합니다.
