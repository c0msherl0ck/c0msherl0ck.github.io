---
title: "[AWS] Ubuntu 16.04 환경 MediaWiki 구축하기"
categories:
  - Cloud
tags:
  - AWS
  - Ubuntu
  - MediaWiki
comments: true
---

이전글에서 생성한 AWS EC2 (OS: Ubuntu 16.04)에서 MediaWiki를 구축하는 방안에 대해 기술한다.


# 1. APM 설치

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install apache2 mysql-server php php-mysql libapache2-mod-php php-xml php-mbstring
// 주의사항 : mysql root 비밀번호를 기록한다. (이후에 사용)
```

설치 및 버전 확인
```
apahce2 -v
php -v
mysql -V
```

apache2 구동 확인
```
sudo service apache2 start // apache2 실행
sudo systemctl status apache2 // apache2 구동 확인
```

<center><p><img src="/assets/2020-09-16-post-AWS_MediaWiki/1.jpg"><br></p></center>

`http://[주소]` 접속을 통해 구동 확인

<center><p><img src="/assets/2020-09-16-post-AWS_MediaWiki/2.jpg"><br></p></center>



# 2. MediaWiki 다운로드

다운로드
```
cd /tmp/
wget https://releases.wikimedia.org/mediawiki/1.34/mediawiki-1.34.2.tar.gz
```

압축해제 및 이동
```
tar -xvzf /tmp/mediawiki-*.tar.gz
sudo mkdir /var/lib/mediawiki
sudo mv mediawiki-*/* /var/lib/mediawiki
```

mediawiki 파일들을 apache2 웹서버 디렉토리에 연결
```
sudo ln -s /var/lib/mediawiki /var/www/html/mediawiki
```

`http://[주소]/mediawiki`

브라우저를 통해 접속 시 다음과 같은 오류를 볼 수 있으며 최소 지원 사양(php 7.2.9)보다 php 버전이 낮아서 발생한다. 
따라서 최소 사양 이상인 php 7.3으로 업데이트한다.

<center><p><img src="/assets/2020-09-16-post-AWS_MediaWiki/3.jpg"><br></p></center>



# 3. php 버전 업데이트

php 7.3 설치
```
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update
sudo apt install php7.3
```

php 7.3 패키지 설치
```
sudo apt-get install php7.3-fpm php7.3-cli php7.3-mysql php7.3-gd php7.3-imagick php7.3-recode php7.3-tidy php7.3-xmlrpc
```

apache2에서 사용하는 php 모듈 재시작하기
```
sudo a2dismod php7.0 // php7.0 종료
sudo a2enmod php7.3 // php 7.3 시작
sudo service apache2 restart // apache2 재시작
```

`http://[주소]/mediawiki`

브라우저 접속 시 다음의 에러를 확인할 수 있으며 이전 php7.3 패키지 설치 시 mbstring, xml을 설치하지 않아서 발생하는 오류이다. 
따라서 해당 패키지를 추가로 설치해준다.

<center><p><img src="/assets/2020-09-16-post-AWS_MediaWiki/4.jpg"><br></p></center>


php 패키지 mbstring, xml 설치
```
sudo apt-get install php7.3-mbstring php7.3-xml
```

apache2에서 사용하는 php 모듈 재시작하기
```
sudo a2dismod php7.3 // php7.3 종료
sudo a2enmod php7.3 // php 7.3 시작
sudo service apache2 restart // apache2 재시작
```

브라우저 접속 시 다음과 같이 초기 세팅을 위한 준비가 완료되었다. 
세팅 전 MYSQL에서 MediaWiki 프로그램이 사용할 데이터베이스와 사용자를 생성하고 권한을 부여한다.

<center><p><img src="/assets/2020-09-16-post-AWS_MediaWiki/5.jpg"><br></p></center>



# 4. MYSQL

```
sudo mysql -u root -p
[root 비밀번호 입력]
```

데이터베이스 생성, 사용자 생성, 권한 부여 
(데이터베이스 이름과 사용자 이름/비밀번호는 기록한다. 이후에 사용)
```
mysql> CREATE DATABASE mediawiki_db; // 데이터베이스 생성
       Query OK, 1 row affected (0.00 sec)
mysql> use mediawiki_db; // 데이터베이스 활성화
       Database changed
mysql> CREATE USER 'mediawiki_user'@'localhost' IDENTIFIED BY 'password'; // 사용자 생성
       Query OK, 0 rows affected (0.02 sec)
mysql> GRANT ALL ON mediawiki_db.* TO 'mediawiki_user'@'localhost'; // 권한부여
       Query OK, 0 rows affected (0.00 sec)
mysql> quit;
```


# 5. MediaWiki 설치

<center><p><img src="/assets/2020-09-16-post-AWS_MediaWiki/6.jpg"><br></p></center>

<center><p><img src="/assets/2020-09-16-post-AWS_MediaWiki/7.jpg"><br></p></center>

데이터베이스 이름과 사용자 이름/비밀번호 입력

<center><p><img src="/assets/2020-09-16-post-AWS_MediaWiki/8.jpg"><br></p></center>

이후 관리자 계정 설정 등을 완료하면 다음과 같이 데이터베이스가 성공적으로 구축되었다는 메세지를 확인할 수 있다.

<center><p><img src="/assets/2020-09-16-post-AWS_MediaWiki/9.jpg"><br></p></center>

설치 완료 후 `LocalSettings.php`를 다운로드 하여 서버의 `/var/lib/mediawiki/` 경로에 업로드하여야 한다.

<center><p><img src="/assets/2020-09-16-post-AWS_MediaWiki/10.jpg"><br></p></center>

FileZilla SFTP 연결을 통해 `/tmp/` 경로에 업로드

<center><p><img src="/assets/2020-09-16-post-AWS_MediaWiki/11.jpg"><br></p></center>

파일 이동

```
sudo mv /tmp/LocalSettings.php /var/lib/mediawiki/
```

파일 확인
```
ls /var/lib/mediawiki/
```

<center><p><img src="/assets/2020-09-16-post-AWS_MediaWiki/12.jpg"><br></p></center>


웹 접속 후 설치 완료 확인

<center><p><img src="/assets/2020-09-16-post-AWS_MediaWiki/13.jpg"><br></p></center>


# 6. 관련 글

[AWS EC2 생성 및 접속](https://c0msherl0ck.github.io/cloud/post-AWS_EC2/)


