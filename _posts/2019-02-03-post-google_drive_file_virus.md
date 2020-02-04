---
title: "[구글 드라이브] 바이러스가 발견되어 파일을 다운로드 할 수 없습니다."
categories:
  - Cloud
tags:
  - Google Drive
  - File Virus
  - Malware
  - Download
comments: true
---

구글 공유 드라이브에서 **악성코드 샘플**을 다운로드 받을 시, "바이러스가 발견되어 파일을 다운로드 할 수 없습니다."와 같은 문구가 뜰 때의 조치사항에 대해 기술한다.

> 1. 에러 메세지
> 2. 조치방법


# 1. 에러 메시지

구글 공유 드라이브에서 악성코드 샘플을 다운로드 받을 때, 다음의 문구가 뜨며 다운로드가 되지 않는다.

<center><p><img src="/assets/2019-02-03-post-google_drive_file_virus/1.png"></p></center>

더블클릭하여 [미리보기] 상태에서 다운로드를 시도해도 다음과 같은 메세지가 뜬다.

<center><p><img src="/assets/2019-02-03-post-google_drive_file_virus/2.png"></p></center>

<br>

# 2. 조치방법

<div class="notice">
구글 드라이브 정책 상 바이러스에 감염된 파일은 소유자만 다운로드할 수 있다. 그러므로, 공유된 악성코드 샘플의 소유자를 변경하는 방법을 통해 다운로드한다.
</div>

`2.1.` 공유 드라이브를 **[내 드라이브에 추가]**한다.

<center><p><img src="/assets/2019-02-03-post-google_drive_file_virus/3.png"></p></center>

`2.2.` 다운로드 받고자 하는 악성코드 파일(소유자가 '나' 가 아닌 상태일 것이다.)을 우클릭하여 **[사본 만들기]**를 클릭한다.

<center><p><img src="/assets/2019-02-03-post-google_drive_file_virus/4.jpg"></p></center>

`2.3.` 소유자가 '나' 인 사본 파일이 생성된 것을 확인할 수 있다.

<center><p><img src="/assets/2019-02-03-post-google_drive_file_virus/5.png"></p></center>

`2.4.` 사본 파일 다운로드 클릭 후, 다음과 같은 경고창이 뜨면 **[감염된 파일 다운로드]**를 클릭한다.

<center><p><img src="/assets/2019-02-03-post-google_drive_file_virus/6.png"></p></center>

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
