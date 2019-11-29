---
layout: post
title: "django 에서 S3에 Static, media 파일 저장하고 사용하기"
categories: posts
excerpt: "Serving media files in django using S3"
tags: [django, aws]
author: myungseokang
comments: true
share: true
ads: true
date: 2016-07-27
---

# django 에서 S3에 Static, media 파일 저장하고 사용하기

**참고한 웹사이트**

[http://sebatyler.github.io/2016/07/16/django-storages-seoul.html](http://sebatyler.github.io/2016/07/16/django-storages-seoul.html)
[http://intersnipe.blogspot.kr/2015/02/django-static-file-local.html](http://intersnipe.blogspot.kr/2015/02/django-static-file-local.html)
[http://intersnipe.blogspot.kr/2015/02/django-media-file-local-s3.html](http://intersnipe.blogspot.kr/2015/02/django-media-file-local-s3.html)


## 1. S3 설정하기

- 버킷을 만드세요.
- 새로운 유저를 만드세요. AWS IAM으로 가서 'create new users'를 클릭하세요. 5개의 칸이 한꺼번에 뜨는데 한 개만 만드셔도 무방합니다. 그리고 하단의 'Generate an access key for each User' 체크 박스를 선택하세요.
- Credentials을 복사 해놓으셔도 좋고, 다운로드 받으셔도 좋습니다.
- 생성하고 나서 Users 카테고리로 가서 새로 생성한 User의 상세 화면으로 들어가세요.
- User ARN(Amazon Resource Name)을 복사 해놓으세요. 세팅에 쓰여요.
- 이제 S3 버킷으로 돌아가세요.
- 버킷을 선택하고 Properties의 Permissions에서 bucket policy를 클릭해서 수정해주세요.
- 아래의 소스를 입력하시고, "BUCKET-NAME", "USER-ARN"만 자신의 상황에 맞게 변경해주시면 됩니다.
- 첫 번째 설정은 컨텐츠를 publicly readable하게 설정. 두번째 설정은 특정 유저가 S3에 작업 할 수 있는 모든 권한을 주는 것입니다.

```json
{
    "Statement": [
        {
          "Sid":"PublicReadForGetBucketObjects",
          "Effect":"Allow",
          "Principal": {
                "AWS": "*"
             },
          "Action":["s3:GetObject"],
          "Resource":["arn:aws:s3:::BUCKET-NAME/*"
          ]
        },
        {
            "Action": "s3:*",
            "Effect": "Allow",
            "Resource": [
                "arn:aws:s3:::BUCKET-NAME",
                "arn:aws:s3:::BUCKET-NAME/*"
            ],
            "Principal": {
                "AWS": [
                    "USER-ARN"
                ]
            }
        }
    ]
}
```

- 만약 특정 누군가에게 추가 권한을 주고 싶다면, 아래와 같이 설정해준다. 아래 설정은 특정인에게 버킷에 있는 아이템을 복제해 갈수 있는 권한을 준느 것이다.

```json
    {
        "Action": "s3:ListBucket",
        "Effect": "Allow",
        "Resource": "arn:aws:s3:::BUCKET-NAME",
        "Principal": {
            "AWS": [
                "USER-ARN"
            ]
        }
    }
```


## 2. Django 설정하기

저는 현재 EC2, S3 모두 서울 리전으로 사용하고 있는데, 아래와 같이 설정해주니 잘 작동했습니다.
(단, template 에서 static 태그를 사용하셔야 합니다. URL에 많이 주의해주세요. 세세한 부분까지 신경써야 합니다.)

settings.py

```python
if DEBUG:
    STATIC_URL = '/static/'
    STATIC_ROOT = os.path.join(BASE_DIR, 'static')
    STATICFILES_STORAGE = 'pipeline.storage.PipelineCachedStorage' # Local, 즉 DEBUG=True 일 경우 pipeline 사용

    MEDIA_URL = '/media/'
    MEDIA_ROOT = os.path.join(BASE_DIR, 'media')

else:
    # AWS Setting
    AWS_REGION = 'ap-northeast-2'
    AWS_STORAGE_BUCKET_NAME = 'BUCKET_NAME'
    AWS_QUERYSTRING_AUTH = False
    AWS_S3_HOST = 's3.%s.amazonaws.com' % AWS_REGION
    AWS_ACCESS_KEY_ID = 'ACCESS_KEY_ID'
    AWS_SECRET_ACCESS_KEY = 'SECRET_ACCESS_KEY'
    AWS_S3_CUSTOM_DOMAIN = '%s.s3.amazonaws.com' % AWS_STORAGE_BUCKET_NAME

    # Static Setting
    STATIC_URL = "https://%s/" % AWS_S3_CUSTOM_DOMAIN
    STATICFILES_STORAGE = 'storages.backends.s3boto.S3BotoStorage'

    #Media Setting
    MEDIA_URL = "https://%s/" % AWS_S3_CUSTOM_DOMAIN
    DEFAULT_FILE_STORAGE = 'storages.backends.s3boto.S3BotoStorage'
```

allowed_hosts 설정을 해주셔야 합니다. => ['\*']

이상 읽어주셔서 감사합니다 :)
