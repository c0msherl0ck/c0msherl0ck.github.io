---
title: "[Docker] 이미지 풀링 및 실행"
categories:
  - Cloud
tags:
  - Docker
  - Ubuntu
  - MediaWiki
comments: true
---

이전 글에서 생성한 Docker 이미지를 풀링 후 실행하는 방법에 대해 기술한다.

도커가 설치되어 있는 환경이라면 어디에서든 다음의 두 단계를 통해 어플리케이션 운영이 가능하다.

Step 1: Pull
```
$ docker pull [image path of MediaWiki]
```

Step 2: Run
```
$ docker run -d -p 80:80 -p 3306:3306 --privileged [image id] /sbin/init
```

이처럼 어플리케이션을 위한 별도의 환경 구성(프로비져닝) 없이, 
간단히 어플리케이션이 배포될 수 있도록 하는 것이 Docker의 장점이다. 
