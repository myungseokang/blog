---
layout: post
title: "django South에서 Migration으로 넘어가기"
categories: posts
excerpt: "Moving from django South to Migration"
tags: [django]
author: leop0ld
comments: true
share: true
ads: true
date: 2016-10-05
---

Django 버전을 1.6에서 1.10으로 올리면서 겪었던 가장 큰 문제 중에 하나라서 조금 더 자세히 정리해보려고 합니다.
기존에 PostgreSQL 9.4 버전대를 이용하고 있었고, 데이터를 유지해야 했기 때문에 좀 까다로운 작업이 될 줄은 알았지만 좀이 아니라 결과적으로는 많이 까다로운 작업이 되었습니다.

1. migrations 파일의 변경

저는 django 1.8 버전부터 사용했기에 South 라이브러리를 잘 모릅니다.(주의바람)
south 라이브러리를 사용하지 않아서 잘은 모르겠으나, 기존에 있던 south 라이브러리가 django에 합쳐진 것으로 알고있습니다.
기존에 South를 사용했기 때문에 migrations 폴더를 싹 지우고, 각 앱마다 다시 migrations 폴더를 만들고 __init__.py 까지 만들어주었습니다.

2. 데이터베이스에 migration 파일 적용

그 다음 명령어로 migration 파일을 만들고 데이터베이스에 적용해주어야 했습니다.
그래서 다음과 같은 명령어를 사용했습니다.

```shell
$ python manage.py makemigrations # 모든 앱에 대한 migration 파일 생성
$ python manage.py migrate # 데이터베이스에 변경 사항 적용
```

이렇게 하려고 했으나 IPAddressField가 GenericIPAddressField로 바뀌어서 변경해주었습니다.
SQL문을 확인해보니 inet 으로 type은 같았습니다.

이렇게 입력을 해주게 되면 데이터베이스에 table들이 생성되게 됩니다.
이렇게 생성된 데이터베이스 위에서 dump 파일을 입혀주면 데이터값들이 잘 들어가야하는게 정상입니다.(테이블 변경사항이 없다는 가정하에)

하지만 저는 dump파일을 적용하는 이 부분에서 제일 많이 애를 먹었습니다.
다른 부분은 검색하면 거의 다 해결방안이 나옵니다.(주로 StackOverflow)
하지만 이 부분은 뭐라 검색해야될지도 모르겠고 어떻게 질문해야될지도 모르겠어서 혼자서 dump 파일의 SQL문을 fileMerge라는 MAC App을 이용해서 비교하면서 작업해봤습니다.

이 작업을 진행하면서 특이했던 부분, 황당했던 부분을 정리해보고자 하는게 이번 포스팅의 목푭니다.
결론부터 말하자면 저는 이 부분을 포기했습니다.
도저히 왜 되는지 왜 안되는지 모르겠어서 놔주었습니다...ㅠㅠ

일단 처음에는 dump 파일에 south_migration~~~ 라는 table들이 눈에 띄었습니다.
south가 없어졌으니 이 테이블또한 필요없다고 판단해서 dump 파일에서 제거해주었습니다.

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

그 다음에

```
$ psql DB_NAME < DUMP_NAME
```

으로 적용을 해봤는데 되는듯 싶다가 ERROR를 뱉었습니다.
그래도 걱정반 기대반으로 확인을 해봤는데 아무것도 안나오더라구요...

그래서 시무룩하고 혹시 잘못된 게 있을 수 있다고 생각해서 dump 파일을 다시 적용해보았는데...
변경이 없던 일부분만 적용이 되었습니다...!

그래서 무엇이 문제지 하고 한참 fileMerge를 이용해서 찾아봤는데 바뀐게 순서 외에는 못찾겠었습니다.
포기하는 심정으로 dump 파일을 연달아 한 3번정도 적용을 해버렸습니다.

그리고 runserver를 통해 확인해보니... 왜인지는 모르겠으나 전부 적용이 되었습니다.
빠진 row 하나 없이 깔끔하게.

이게 왜 되는지, 왜 안됬는지 저는 모르겠습니다 ㅠㅠ
혹시 아시는 분이나 좋은 방법이 있으신 분은 알려주시면 감사하겠습니다.
