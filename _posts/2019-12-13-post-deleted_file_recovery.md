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

윈도우 탐색기 창에서 확인 시 사용자 편의를 위해 User SID 대신 휴지통이라는 이름으로 수정해서 보여주는 것이며, FTK Imager를 통해 확인할 경우 원래 폴더명인 `User SID(Security Identifier)`을 확인할 수 있다.

<center><p><img src="/assets/폴더명/파일명.jpg" width="100%"><br><em>윈도우 탐색기로 확인한 휴지통</em></p></center>
<center><p><img src="/assets/폴더명/파일명.jpg" width="100%"><br><em>FTK Imager로 확인한 휴지통</em></p></center>

사용자에 부여된 SID는 다음과 같이 확인 가능하다.

<center><p><img src="/assets/폴더명/파일명.jpg" width="100%"><br><em>SID 확인</em></p></center>

일반적인 업무용 PC의 경우 사용자 계정이 1명이므로 휴지통 폴더가 1개만 생성되지만, 서버와 같이 여러 사용자가 사용할 경우 **사용자별로 휴지통 폴더가 생성**된다.

<center><p><img src="/assets/폴더명/파일명.jpg" width="100%"><br><em>서버(다계정) 휴지통</em></p></center>

휴지통으로 파일 이동(파일 삭제) 시, $R~(실제 데이터 / Real), $I~(삭제 전 파일 경로 등 메타 데이터 / Information) 2개의 파일로 구분되어 저장되며, 휴지통에서 파일 복원 시 해당 정보를 이용하여 삭제 전 경로에 파일을 복원한다. EnCase의 경우 $R~ 파일에 대응되는 $I~ 파일의 정보를 이용하여, 삭제 전 이름을 표기해준다.

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
