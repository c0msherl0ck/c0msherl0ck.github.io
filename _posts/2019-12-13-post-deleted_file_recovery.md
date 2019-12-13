---
title: "[파일 복구] 사용자 삭제 행위 별 복구 가능성"
categories:
  - Data Recovery
tags:
  - 파일 삭제
  - 파일 복구
  - Delete
  - Shift + Delete
  - Eraser
  - $MFT
comments: true
---

윈도우 환경에서 사용자가 파일을 삭제하는 방법은 크게 3가지(`Delete`/`Shift + Delete`/`도구 이용`)로 분류할 수 있다. 이번 글에서는 각각의 사용자 삭제 행위 별 복구 가능성에 대해 알아본다.

> 1. Delete & 휴지통 비우기
> 2. Shift + Delete
> 3. Eraser(삭제 도구)


# 1. Delete & 휴지통 비우기

일반적인 파일 삭제는 **단순히 휴지통이라는 폴더에 파일을 이동**하는 것과 동일하며, 휴지통 폴더 경로는 다음과 같다.
```C:\$Recycle.Bin\[User SID]```

이 때, 휴지통이라는 폴더 이름은 윈도우가 사용자 편의를 위해 임의로 수정해서 보여주는 것이며, FTK Imager를 통해 확인할 경우, 휴지통 폴더의 원래 이름 `사용자에 부여된 윈도우 SID(Security Identifier)`을 확인할 수 있다.

<center><p><img src="/assets/폴더명/파일명.jpg" width="100%"><br><em>휴지통 폴더 경로</em></p></center>
<center><p><img src="/assets/폴더명/파일명.jpg" width="100%"><br><em>휴지통 폴더 경로(SID)</em></p></center>

사용자에 부여된 SID는 다음과 같이 확인 가능하다.

<center><p><img src="/assets/폴더명/파일명.jpg" width="100%"><br><em>SID 혹인</em></p></center>

일반적인 업무용 PC의 경우 사용자 계정이 1명이기 때문에, 휴지통 폴더가 1개만 생성되지만, 서버와 같이 여러 사용자가 

# 2. Shift + Delete



# 3. Eraser(삭제 도구)



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
