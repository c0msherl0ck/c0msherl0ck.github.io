---
title: "[리버싱] OllyDbg 를 이용한 Fake.exe 크랙하기"
categories:
  - Reversing
tags:
  - OllyDbg
  - x64Dbg
  - ImmunityDbg
comments: true
---

이전글 "OllyDbg 를 이용한 CrackMe1.exe 크랙하기"에 이어서, Fake.exe 파일을 크랙하는 방법을 기술한다.

[OllyDbg 를 이용한 CrackMe1.exe 크랙하기](https://holywaterkim.tistory.com/59?category=831422)

> 1. "correct" 문자열 찾기
> 2. Break Points 설정
> 3. 코드와 레지스터 값 분석
> 4. 크랙하기
> 5. 참고

# 1. "correct" 문자열 찾기

[Search for]-[all referenced text strings] 에서, "correct" 가 있는 코드로 이동한다.

<center><p><img src="/assets/2019-01-23-post-OllyDbg_Fake/1.jpg"></p></center>

# 2. Break Points 설정

분기점 JNZ 에 Break Points 를 설정하고 F9 를 눌러 프로그램을 실행시킨다.

<center><p><img src="/assets/2019-01-23-post-OllyDbg_Fake/2.jpg"></p></center>

# 3. 코드와 레지스터 값 분석

프로그램 창에서 다음과 같이 sample serial 를 입력한 다음, F8(Step over) 을 통해 코드와 레지스터 값을 분석한다.

<center><p><img src="/assets/2019-01-23-post-OllyDbg_Fake/3.jpg"></p></center>

> 첫번째 분기문까지 F8 을 눌러 이동한다.

<center><p><img src="/assets/2019-01-23-post-OllyDbg_Fake/3-1.jpg"></p></center>

> EBX 에 들어간 값은 403078 주소의 DWORD(32 bits, 4bytes) 만큼 크기 이며 앞서 입력한 "1234" 임을 확인 할 수 있다.

<center><p><img src="/assets/2019-01-23-post-OllyDbg_Fake/3-2.jpg"></p></center>

> 위 그림의 회색블락 라인의 BL 이 의미하는 바는 EBX 의 하위 8 bit 이며, 현재 sample serial 를 입력한 상태에서는 0x31(ASCII "1") 을 의미한다.

**※ Little Endian 주의!!**

<center><p><img src="/assets/2019-01-23-post-OllyDbg_Fake/3-3.jpg"></p></center>

> JNZ 분기문을 뛰어넘기 위해, ZF(zero flag) 를 1로 조작한다. (Z 부분의 0 부분을 더블클릭하면 1로 변환)

[조작전]

<center><p><img src="/assets/2019-01-23-post-OllyDbg_Fake/3-4.jpg"></p></center>

[조작후]

<center><p><img src="/assets/2019-01-23-post-OllyDbg_Fake/3-5.jpg"></p></center>

> 다음 두번째 분기문으로 이동하면, EBX 에 들어가는 값이, 첫번째 EBX 에 들어가는 값의 메모리 주소보다 +1 byte 인 곳을 참조하는 것을 알 수 있으며 EBX에 들어간 값은 0x00343332 이다. 이때, BL의 값을 확인해보면, 현재 EBX 의 하위 8 bit 값인, 0x32(ASCII "2") 임을 알 수 있다.

<center><p><img src="/assets/2019-01-23-post-OllyDbg_Fake/3-6.jpg"></p></center>

> 위의 결과들을 종합하여, 세 개의 JNZ 분기문을 뛰어넘고, "correct"라는 문자열이 있는 코드까지 가기 위해서는 입력하는 serial 문자열이 0x616263 이어야 한다는 것을 알 수 있으며, 이는 ASCII "abc" 이다.

# 4. 크랙하기

> "abc" 를 입력하면 다음과 같은 결과가 나온다.

<center><p><img src="/assets/2019-01-23-post-OllyDbg_Fake/4-1.jpg"></p></center>

> "abcdefg" 와 같은 문자열을 입력하여도, 다음과 같이 코드를 덮어씌운다던가의 문제가 있지 않고, 세 개의 분기문에서는 앞의 한글자씩만을 비교하여 a, b, c 가 있으면 correct 로 처리한다. 

<center><p><img src="/assets/2019-01-23-post-OllyDbg_Fake/4-2.jpg"></p></center>

# 5. 참고

[레지스터-Register-의-이해](https://orang.tistory.com/entry/%EB%A0%88%EC%A7%80%EC%8A%A4%ED%84%B0-Register-%EC%9D%98-%EC%9D%B4%ED%95%B4)

<center><p><img src="/assets/2019-01-23-post-OllyDbg_Fake/5.jpg"></p></center>

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

