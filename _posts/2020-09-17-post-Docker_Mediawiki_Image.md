---
title: "[Docker] MediaWiki 이미지 생성 및 배포하기"
categories:
  - Cloud
tags:
  - Docker
  - Ubuntu
  - MediaWiki
comments: true
---

이전글에서 생성한 Ubuntu 16.04 환경에서 구축한 MediaWiki를 Docker 이미지로 만들어 배포하는 방안에 대해 기술한다.


# 1. Docker 개념

## 1.1. VMware vs Docker

VMware 가상머신하고 유사하며 차이는 있지만, 리눅스 환경에서 주로 사용하는 가상머신이라고 생각하면 편리하다. 

||VMware|Docker|
|---|---|---|
|주 사용|개인용 PC Windows/Mac|Linux 서버|
|이미지 확장자|vmdk|-|
|이미지 생성|snapshot|commit|


## 1.2. Docker 필요성 및 장점

이전 글에서 MediaWiki 구축 당시 php 버전이 맞지 않아 에러가 발생하며 php 버전 수정이 필요했다. 
이처럼 개발 환경과 운영 환경의 차이로 인해 어플리케이션 배포 시 환경 구성을 해주어야 하는데 이를 *프로비져닝(Provisioning)*이라고 한다. 
Docker는 어플리케이션 실행 환경을 포함한 이미지 구축 및 배포를 통해 프로비져닝 과정을 용이하게 한다. 


# 2. Docker 설치

Ubuntu 18.04 환경에서 Docker 설치 및 실행

```
$ sudo apt install docker.io
$ docker -v
  Docker version 19.03.6, build 369ce74a3c
$ sudo service docker start
$ sudo systemctl status docker
  ● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; disabled; vendor preset: enabled)
     Active: active (running) 
```

사용자 계정(ubuntu 계정)에서도 도커를 직접 사용할 수 있도록 docker 그룹에 사용자 추가
```
$ sudo usermod -aG docker $USER
$ sudo su - $USER
```
이제 ubuntu 계정에서 sudo 없이 docker 사용이 가능하다.


# 3. Docker 환경 구축

Ubuntu 16.04 도커 이미지 다운로드
```
$ docker pull ubuntu:16.04
```

이미지 실행 및 접속
```
$ docker run -d -p 80:80 -p 3306:3306 --privileged ubuntu:16.04 /sbin/init
// -d : 데몬으로 실행(백그라운드에서 계속해서 실행)
// -p : 포트포워딩
// --privileged: docker 내에서 systemctl 등의 명령어 사용을 위해서 권한 부여
 
$ docker ps
// Container ID 조회
 
$ docker exec -it [컨테이너 ID] /bin/bash
// 실행중인 ubuntu 16.04 컨테이너의 bash 쉘 실행
```

bash 쉘에서 다음의 작업 진행
```
# apt-get update
# apt-get upgrade
# apt-get install wget
# apt-get install software-properties-common
```

이후 과정은 이전 글 [AWS EC2 Ubuntu 16.04 에서 MediaWiki 구축하기](https://c0msherl0ck.github.io/cloud/post-AWS_MediaWiki/)
에서 진행한 작업을 도커에서 동일하게 진행한다.

이전 포스팅과의 차이점은 도커 설치 후 가상화 환경에서 Ubuntu 16.04 도커 이미지를 기반으로 환경을 구축한다는 점이다.

**단, 도커 컨테이너 MYSQL 설치 시 에러가 발생하므로 주의가 필요하다.**


## 3.1. MYSQL 설치 시 에러 처리

우분투 16.04 컨테이너 내 MYSQL 설치 시 다음과 같이 에러가 발생한다.
```
# apt-get install mysql-server
# service mysql start

  Job for mysql.service failed because the control process exited with error code. 
  See "systemctl status mysql.service" and "journalctl -xe" for details.

# systemctl status mysql.service

  systemd[1]: Starting MySQL Community Server...
  mysqld[1109]: mysqld: Can't read dir of '/etc/mysql/conf.d/' (Errcode: 13 - Permission denied)
  mysqld[1109]: mysqld: [ERROR] Fatal error in defaults handling. Program aborted!
  systemd[1]: mysql.service: Main process exited, code=exited, status=1/FAILURE
```

AppArmor 에러로 상세사항은 다음의 링크를 참조한다.

<https://minikube.sigs.k8s.io/docs/drivers/docker/#troubleshooting> 

`Deploying MySql on a linux with AppArmor`

*AppArmor에서 mysqld 예외처리* (이 부분이 가장 중요하다)
```
# ln -s /etc/apparmor.d/usr.sbin.mysqld /etc/apparmor.d/disable/
# apparmor_parser -R /etc/apparmor.d/usr.sbin.mysqld
```

mysql 재시작 후 정상작동 확인
```
# service mysql restart
# systemctl status mysql
  ● mysql.service - MySQL Community Server
     Loaded: loaded (/lib/systemd/system/mysql.service; enabled; vendor preset: enabled)
    Active: active (running)
```

## 3.2. 컨테이너 내로 파일 이동

MediaWiki 설치 완료 후 LocalSettings.php를 다운로드 받으면 stfp를 통해 호스트 서버에 업로드한다. 
그리고 다음의 명령어를 통해 컨테이너 내로 복사 후 작업을 완료한다.
```
$ docker cp /tmp/LocalSettings.php 2167281f67f7:/tmp/
$ docker ps  
$ docker exec -it [컨테이너 id] /bin/bash

# ls -al /tmp/      
# mv /tmp/LocalSettings.php /var/lib/mediawiki/
# ls /var/lib/mediawiki/
```

이로써 컨테이너 내 MediaWiki 설치가 완료되었다.


# 4. 도커 이미지 생성 및 배포

컨테이너 종료 후 이미지로 굽기
```
$ docker ps
$ docker stop [container id]
$ docker commit [container id] [image name]
$ docker images
```

도커 로그인
```
$ docker login
```

자신의 DOCKER HUB ID를 네임스페이스로 하여 이미지 이름 변경
```
$ docker tag [original image name] [new image name]
// new image name = <DOCKER_HUB_ID>/[original image name]
```

업로드
```
$ docker push [new image name]
```

<center><p><img src="/assets/2020-09-17-post-Docker_Mediawiki_Image/docker_hub.jpg"><br></p></center>


# 5. 참고

도커 명령어, <https://nicewoong.github.io/development/2017/10/09/basic-usage-for-docker/>

도커 기초, <https://www.44bits.io/ko/post/easy-deploy-with-docker#%EB%8F%84%EC%BB%A4%EC%99%80-%EB%B2%84%EC%A0%84-%EA%B4%80%EB%A6%AC-%EC%8B%9C%EC%8A%A4%ED%85%9C>

도커 컨테이너 포트 추가, <https://oboki.net/workspace/system/docker/docker-%EC%8B%A4%ED%96%89-%EC%A4%91%EC%9D%B8-container%EC%97%90-%ED%8F%AC%ED%8A%B8-%EC%B6%94%EA%B0%80%ED%95%98%EA%B8%B0/>

도커 MYSQL, <https://minikube.sigs.k8s.io/docs/drivers/docker/#troubleshooting>
