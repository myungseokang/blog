---
layout: post
title: "Ubuntu에서 PostgreSQL 사용하기"
categories: posts
excerpt: "Using PostgreSQL in Ubuntu"
tags: [ubuntu, postgresql]
author: leop0ld
comments: true
share: true
date: 2016-07-04
---

# Ubuntu에서 PostgreSQL 사용하기

## 1. PostgreSQL 설치하기

가장 먼저

```shell
$ sudo apt-get update
```

을 실행해줍시다!

그 다음 postgresql을 설치해줍니다.

```shell
$ sudo apt-get install postgresql
```

자 그럼 이제 잘 설치가 되었는지 접속해보도록 하겠습니다.

```shell
$ sudo -u postgres psql
```

이 명령어를 입력해주게 되면 postgres라는 이름의 유저로 postgresql db를 로그인합니다.

잘 들어가졌다면 셸이 다음과 같이 변하게 됩니다.

```sql
postgres=#
```

잘 설치됬고 잘 들어온 상태입니다.

도움말 확인은 \? 로 가능하고 종료는 \q 로 가능합니다.

postgres 는 기본적으로 생성되는 root 계정이라 패스워드를 변경해주어야 합니다.

```sql
ALTER USER postgres with encrypted password '설정할 패스워드';
```

password 다음에 싱글쿼터로 설정할 패스워드를 감싸주는 걸 잊지 마세요!!

그리고 \q로 종료를 한 뒤, postgresql을 재시작해줍니다.

```shell
$ sudo /etc/init.d/postgresql restart
```

위의 명령어로 재시작 시켜줄 수 있습니다.

그 다음에는 패스워드 설정이 잘되었는지 확인하기 위해 아래의 명령어로 접속해봅시다!

```shell
$ psql -U postgres -h localhost
```

-U 옵션으로 접속할 유저를 선택하고 -h 옵션으로 호스트를 선택해서 접속해줍니다.

명령어를 입력하게 되면 패스워드를 입력해달라는 프롬프트가 나오는데 아까 설정해준 패스워드를 입력해줍니다.

이렇게 간단하게 PostgreSQL을 설치해보았습니다.

이제 본격적으로 Database를 만들고 사용해보도록 하겠습니다.

<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-format="fluid"
     data-ad-layout="in-article"
     data-ad-client="ca-pub-1864899826477546"
     data-ad-slot="2703362319"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## 2. PostgreSQL 사용하기

먼저 db를 만들고 그 db 안에서 table을 만들고 데이터를 삽입하는 작업을 해보도록 하겠습니다!

```shell
$ sudo -u postgres createdb 데이터베이스_이름
```

저는 편의상 test라고 만들어보도록 하겠습니다.

그리고 만든 db에 접속하고 싶으면 아래의 명령어를 입력하면 됩니다.

```shell
$ sudo -u postgres psql 데이터베이스_이름
```

잘 접속했다면 프롬프트가 바뀌어있는 것을 확인할 수 있습니다.

```sql
데이터베이스_이름=#
```

처럼 바뀌어있습니다. Ex) test=#



그리고 db에 접속했으니 이제 table을 만들어보도록 하겠습니다!

```sql
test=# create table hello (
test(# id    integer primary key,
test(# val1  char(16),
test(# val2  text
test(# );
CREATE TABLE
```

이런 식으로 만들 수 있습니다.

```sql
test=# insert into hello values (
test(# 1,
test(# 'hello world!',
test(# 'Goooooooood!'
test(# );
INSERT 0 1
```

그리고 데이터는 이런식으로 삽입해줄 수 있습니다.

그리고 select 구문으로 조회를 해보겠습니다.

```sql
test=# select * from hello;
 id |       val1       |     val2     
----+------------------+--------------
  1 | hello world!     | Goooooooood!
(1 row)
```

위와 같이 깔끔하게 출력됩니다!!

이상 읽어주셔서 감사합니다 :)
