---
title: "OllyDbg 를 이용한 CrackMe1.exe 크랙하기"
categories:
  - Reversing
tags:
  - OllyDbg
  - CrackMe
  - Reversing
comments: true
---

OllyDbg 를 이용하여 CrackMe1.exe 크랙하는 방법에 대해 기술한다.

> 1. OllyDbg, x64Dbg, Immunity Dbg 셋 중 하나를 설치한다.
> 2. 파일을 열고, 필요한 코드를 찾아가 Break Point를 설정한다.
> 3. 크랙하기

# 1. OllyDbg, x64Dbg, immunity Dbg 셋 중 하나를 설치한다.

OllyDbg 의 경우 64 비트 환경에서 구동하기 위해서는 별도의 설정들이 필요하기 때문에, x64Dbg 의 압축버전 또는, ImmunityDbg 의 installer 를 통해 설치하는 것이 정신건강에 이롭다. 3개의 디버거 모두 OllyDbg 를 기반으로 하고 있기에 기능상 큰 차이는 없고, x64 Dbg, Immunity Dbg 가 확장팩 정도라 생각하면 된다. x64 Dbg 는 메뉴기능에 한국어를 지원하며, Immunity Dbg 는 python script(2.7.1) 를 지원한다.

도구는 많이 사용해봐야 한다는 마음으로, 3가지 모두를 설치해서 사용해본다.

<center><p><img src="/assets/2019-01-23-post-OllyDbg_CrackMe_1/1.jfif"></p></center>

# 2. 파일을 열고, 필요한 코드를 찾아가 break point 를 설정한다.

> 2.1. 처음 파일을 열게 되면, 다음과 같은 화면이 뜬다.

이때 좌측 상단의 7775CE37 과 같은 메모리 주소는 시스템 영역이다.

<center><p><img src="/assets/2019-01-23-post-OllyDbg_CrackMe_1/2.1.jfif"></p></center>

> 2.2. F9 를 눌러 실행을 하거나, [우클릭]-[View]-[Module 'CrackMe1'] 을 클릭한다.

<center><p><img src="/assets/2019-01-23-post-OllyDbg_CrackMe_1/2.2.jfif"></p></center>

> 2.3. 다음과 같이 메모리 주소 영역이 00401000, 프로그램 영역으로 변경된 것을 알 수 있다.

<center><p><img src="/assets/2019-01-23-post-OllyDbg_CrackMe_1/2.3.jfif"></p></center>

> 2.4. 코드의 원하는 부분을 보기 위하여, [우클릭]-[Search for]-[All referenced text strings] 기능을 이용한다.

<center><p><img src="/assets/2019-01-23-post-OllyDbg_CrackMe_1/2.4.jfif"></p></center>

> 2.5. 크랙이 성공했을 때, 나오는 문자열로 추정되는 "Congratulations" 을 클릭하여 해당 부분으로 이동한다.

<center><p><img src="/assets/2019-01-23-post-OllyDbg_CrackMe_1/2.5.jfif"></p></center>

> 2.6. 해당 코드 부분의 앞뒤 영역을 보면서 break point 를 설정한다.

보통 분기문(JMP, JNZ, 등등)에 설정한다.

<center><p><img src="/assets/2019-01-23-post-OllyDbg_CrackMe_1/2.6.jfif"></p></center>

<div class="notice">
위의 그림에서 JNZ(Jump if not zero) 을 보면, 00401172 로 점프하도록 되어있고, 해당 영역에 해당하는 코드를 보면 틀렸을 경우의 처리임을 알 수 있다. 그러므로, "Congratulations"라는 문자열이 나오는 코드까지 가기 위해서는 CMP, JNZ 구문에서 jump 를 하지 않아야 한다는 것을 알 수 있다.
</div>

# 3. 크랙하기

> 3.1. F9 를 눌러 프로그램을 실행시킨다.

<center><p><img src="/assets/2019-01-23-post-OllyDbg_CrackMe_1/3.1.jfif"></p></center>

break point 에 아직 걸리지 않은 상태이므로, 프로그램 상에서 serial 번호를 입력하여 진행시킨다.

> 3.2. test 문자열로 "1234" 를 입력해본다.

다음과 같이 break point 에 걸린 것을 알 수 있다.

<center><p><img src="/assets/2019-01-23-post-OllyDbg_CrackMe_1/3.2.jfif"></p></center>

> 3.3. F8 (Step Over) 기능을 통해 한줄씩 코드를 실행시키며, 레지스터 값을 분석한다.

<center><p><img src="/assets/2019-01-23-post-OllyDbg_CrackMe_1/3.3.jfif"></p></center>

```
MOV EAX, 31333736  // EAX 레지스터에 0x31333736 을 넣는다.
CMP EAX, DWORD PTR DS:[ESI] // EAX 값에서 ESI 주소에 있는 값을 뺀 값이 0 일 경우, Zero flag 를 1로 설정한다.
JNZ SHORT CrackMe1.00401172 // Zero flag 가 1이 아닐 경우, 해당 주소로 점프한다.
MOV EAX, 37333234  // EAX 레지스터에 0x37333234 을 넣는다.
CMP EAX, DWORD PTR DS:[ESI+4] // EAX 값에서 ESI+4 주소에 있는 값을 뺀 값이 0 일 경우, Zero flag 를 1로 설정한다.
JNZ SHORT CrackMe1.00401172 // Zero flag 가 1이 아닐 경우, 해당 주소로 점프한다.
```

> 3.4. ESI 값을 확인해보기 위해서, 좌측하단의 창에서 Ctrl + G 를 통해, ESI 주소로 이동하면 다음과 같이 값을 확인할 수 있다.

<center><p><img src="/assets/2019-01-23-post-OllyDbg_CrackMe_1/3.4.jfif"></p></center>

> 3.5. 결과 종합

<div class="notice">
`3.3.` `3.4.` 결과에 따르면 HEX DUMP 로 31333736, 37333234 이 serial number 임을 유추할 수 있으며, 이는 ASCII 코드 값으로 6731, 4237 임을 알 수 있다.
(※ Little Endian 임에 주의!!!!)
</div>


> 3.6. "67314237" 을 입력했을 때의 결과 확인

<center><p><img src="/assets/2019-01-23-post-OllyDbg_CrackMe_1/3.6.1.jfif"></p></center>

<center><p><img src="/assets/2019-01-23-post-OllyDbg_CrackMe_1/3.6.2.jfif"></p></center>


# 참고

[](https://)

[인라인 링크](https://)



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
