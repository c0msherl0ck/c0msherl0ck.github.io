---
title: "Zone.Identifier"
categories:
  - File System
tags:
  - Alternative Data Stream
  - Zone.Identifier
comments: true
---

포렌식 도구에서 확인 가능한 파일명의 끝에 Zone.Identifier가 붙는 파일들은 무엇인가? Windows 탐색기에서는 확인이 되지 않는데, 해당 파일들은 언제 생성되었고, 어떠한 데이터를 저장하는 것일까?

> 1. 파일 확인(EnCase)
> 2. Zone Identifier
> 3. Zone ID


# 1. 파일 확인(EnCase)

EnCase에서는 다음과 같이 Zone.Identifier 파일들을 확인할 수 있다. 해당 파일들은 **사실 파일이 아니며** NTFS 파일 시스템의 `ADS`(Alternative Data Stream)에 저장된 데이터를 EnCase가 불러와 파일 형태로 보여주는 것이다.

<center><img src="/assets/2020-01-03-post-Zone_Identifier/encase.jpg"></center>


# 2. Zone Identifier

인터넷에서 파일을 다운로드 받을 때 다음과 같은 경고창이 뜬다. 이 때 `이런 형식의 파일을 열기 전 항상 확인` 체크를 한 상태로 다운로드 할 경 Windows의 NTFS 파일 시스템은 다운로드 파일의 출처(URL)를 기록한다.

<center><img src="/assets/2020-01-03-post-Zone_Identifier/warning.jpg"></center>


Zone Identifer가 기록된 파일들의 속성을 확인해보면 `보안 : 이 파일은 다른 컴퓨터로부터 왔으며 사용자의 컴퓨터를 보호하기 위해 차단 되었을 수도 있습니다.` 문구가 추가되어 있는 것을 확인할 수 있다.

<center><img src="/assets/2020-01-03-post-Zone_Identifier/security.jpg"></center>


다운로드 파일의 출처(URL)은 `DOS` 창에서도 다음과 같이 확인이 가능하다. (침해사고 분석 시 악성코드 출처를 밝히는데 용이)

```
dir /r
```

<center><img src="/assets/2020-01-03-post-Zone_Identifier/cmd.jpg"></center>


# 3. Zone Id

Zone Identifer는 인터넷 뿐만이 아닌, 다양한 네트워크 환경에서 다운로드 받은 파일에도 기록된다. Zone ID 값을 통해 내부망/외부망 중 어떤 네트워크에서 다운로드 받았는지 유추할 수 있다.

- URLZONE_INTRANET = 1
- URLZONE_TRUSTED = 2
- URLZONE_INTERNET = 3
- URLZONE_UNTRUSTED = 4 


# 4. 참고

http://www.sandersonforensics.com/Files/ZoneIdentifier.pdf




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

