---
title: "[리버싱] IDA 를 이용하여 Crackme2.exe 크랙하기"
categories:
  - Reversing
tags:
  - IDA
  - Crackme
comments: true
---

IDA 를 이용하여 Crackme2.exe 크랙하는 방법에 대해 기술한다.

> 1. 코드 disassembly
> 2. 코드해석 및 크랙

# 1. 코드 disassembly

> [Shift + F12] 를 통해 String 을 검색한 후, 크랙과 관련이 있는 문자열을 더블 클릭한다.

<center><p><img src="/assets/2019-01-28-post-IDA_CrackMe2/2-1.jpg"></p></center>

> 다음과 같이 노란색 블럭이 된 상태에서 [x] 를 누르면 해당 문자열을 참조하는 함수를 확인할 수 있다.

<center><p><img src="/assets/2019-01-28-post-IDA_CrackMe2/2-2.jpg"></p></center>

> 해당 함수를 확인하면 다음과 같은 assembly graph 화면을 볼 수 있다. 이 상태에서 [F5] 를 눌러 disassembly 한다.

<center><p><img src="/assets/2019-01-28-post-IDA_CrackMe2/2-3.jpg"></p></center>

> 다음은 disassembly 된 pseudo code 화면이며, 변수를 선택한 상태에서 [n] 을 클릭하여 변수명을 재설정할 수 있다.

<center><p><img src="/assets/2019-01-28-post-IDA_CrackMe2/2-4.jpg"></p></center>

# 2. 코드해석 및 크랙

```
for(i=0; i<lengthOfUser; ++i)
  v5 += User[i];
```
<div class="notice">
User 문자열의 각 문자를 Hex 값으로 더한 값이 v5 에 저장된다.<br>
예를 들어, User = "1124" 이라면, v5 = 0x31 + 0x31 + 0x32 + 0x34 = 0xC8 이 된다.
</div>
```
sprintf(&String1, "%d", 1337 * v5);
```
<div class="notice">
String1 에 1337 * v5 값을 십진수형태로 저장한다.<br>
예를 들어, v5 = 0xC8 = 200 이고, String1 = 1337 * 200 = 267400 이 된다.
</div>
```
if ( lstrcmpA(&String1, &Serial) )
  MessageBoxA(hDlg, "Bad Boy!", "Doh!", 0x10u);
else
  MessageBoxA(hDlg, "Good Boy!\n If this key is from your keygen u should write an solution!", "Wee!", 0x40u);
```
<div class="notice">
String1 과 Serial 문자열을 비교하여 같으면 Good Boy 를 띄어준다.
</div>

<center><p><img src="/assets/2019-01-28-post-IDA_CrackMe2/3.jpg"></p></center>

u should write a solution! -> Yea, I did.

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

