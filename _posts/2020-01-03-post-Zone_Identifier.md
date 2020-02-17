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
> 2. 
> 3. 


# 1. 파일 확인(EnCase)

EnCase에서는 다음과 같이 Zone.Identifier 파일들을 확인할 수 있다. 해당 파일들은 **사실 파일이 아니며** NTFS 파일 시스템의 `ADS`(Alternative Data Stream)에 저장된 데이터를 EnCase가 불러와 파일 형태로 보여주는 것이다.

<center><img src="/assets/2020-01-03-post-Zone_Identifier/encase.jpg"><em></em></center>

# 2. Zone Identifier 기능

인터넷에서 파일을 다운로드 받을 때, Windows의 NTFS 파일 시스템은 그 출처(URL)를 기록한다. 


- aa
  - bb
    - cc
- dd


```
aaaaaaaaaaaaaaaaa
```


`bbbbbbbbbbbbbb`


|파일명|역할|
|---|---|
|||
|||
|||

<center><p><img src="/assets/폴더명/파일명.jpg"></p></center>

<center><p><img src="/assets/폴더명/파일명.jpg" width="100%"><br><em>주석</em></p></center>

> 참고

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

