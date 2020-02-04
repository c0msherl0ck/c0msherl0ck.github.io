---
title: "Zone.Identifier"
categories:
  - File System
tags:
  - Alternative Data Stream
  - Zone.Identifier
comments: true
---

EnCase에서 파일 목록 확인 시, 파일 이름의 끝에 Zone.Identifier가 붙은 파일들을 확인할 수 있다. 해당 파일들은 사실 파일이 아니며 NTFS 파일 시스템의 Alternative Data Stream에 저장된 데이터를 의미한다. 이번 글에서는 Zone.Identifier가 언제 생성되는지, 어떠한 데이터를 저장하는지 알아본다.

<center><p><img src="/assets/2020-01-03-post-Zone_Identifier/파일명.jpg"><br><em>EnCase - Zone.Identifier 확인</em></p></center>

>
>
>




# 제목

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

