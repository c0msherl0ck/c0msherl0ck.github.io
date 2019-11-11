---
title: "[아웃룩] PST, OST, MSG 파일"
categories:
  - E-mail
tags:
  - E-mail
  - Outlook
  - pst
  - ost
  - msg
comments: true
---

아웃룩 데이터 파일은 다음과 같이 크게 3가지 확장자가 있다. ".pst, .ost, .msg " 3가지 모두 아웃룩의 데이터(메일 등)를 저장한다는 점은 같지만, 사용방식과 저장되는 정보에는 차이가 있다. 각 파일별 특징에 대해 알아보자

# OST

Outlook 2016 에서 전자 메일 계정을 추가하면 계정 관련 정보의 로컬 복사본이 **자동으로** 컴퓨터에 저장되며, 이때 컴퓨터에 저장되는 파일의 확장자가 `.ost` 이다. `.ost` 파일에는 추가한 계정의 모든 전자 메일, 일정 데이터, 연락처 등이 저장되며, 이를 통해 인터넷이 연결되어 있지 않더라도 사용자는 이전에 자신이 수신한 메일을 확인할 수 있다.

<center><p>
<img src="/assets/2019-11-11-post-outlook_pst_ost_msg/account_setting.jpg" width="100%">
<em>전자 메일 계정 추가 화면</em>
</p></center>

OST 파일의 위치는 다음과 같으며, [계정 설정]-[데이터 파일] 메뉴에서 확인 가능하다. 필자의 경우 2개의 전자메일 계정을 추가하였고, 2개의 ost 파일이 생성된 것을 확인하였다.

```
C:\Users\[사용자명]\AppData\Local\Microsoft\Outlook\[전자 메일 계정]
```
<center><p>
<img src="/assets/2019-11-11-post-outlook_pst_ost_msg/ost_location_1.jpg" width="100%">
<em>OST 파일 위치</em>
</p></center>

<center><p>
<img src="/assets/2019-11-11-post-outlook_pst_ost_msg/ost_location_2.jpg" width="100%">
<em>계정별 OST 파일 확인</em>
</p></center>

# PST

Outlook 2016 에서 `.pst` 파일은 주로 **백업**을 위해 사용된다. `.ost` 파일이 사용자의 의도와 무관하게 자동적으로 생성된 것과 달리, `.pst` 파일은 사용자가 **가져오기/내보내기** 기능을 통해 백업에 필요한 항목을 선택한 후 생성한다.

<center><p>
<img src="/assets/2019-11-11-post-outlook_pst_ost_msg/import.jpg" width="100%">
<em>가져오기 - 다른 프로그램이나 파일</em>
</p></center>

<center><p>
<img src="/assets/2019-11-11-post-outlook_pst_ost_msg/import_2.jpg" width="100%">
<em>가져오기 - pst 파일</em>
</p></center>

<center><p>
<img src="/assets/2019-11-11-post-outlook_pst_ost_msg/export.jpg" width="100%">
<em>내보내기 - 파일</em>
</p></center>

<center><p>
<img src="/assets/2019-11-11-post-outlook_pst_ost_msg/export_2.jpg" width="100%">
<em>내보내기 - pst 파일</em>
</p></center>

# MSG

`.pst` 와 `.ost` 는 메일을 저장하기는 하지만, 개별 메일을 선택적으로 저장할 수는 없다. `.msg` 의 경우 개별 메일을 선택하여 **다른 이름으로 저장** 기능을 통해 선택적 추출 및 저장이 가능하다.(**첨부파일 포함**)

<center><p>
<img src="/assets/2019-11-11-post-outlook_pst_ost_msg/msg.jpg" width="100%">
<em>다른 이름으로 저장</em>
</p></center>

<center><p>
<img src="/assets/2019-11-11-post-outlook_pst_ost_msg/msg_2.jpg" width="100%">
<em>아웃룩 메세지 형식</em>
</p></center>

> 참고

[Outlook 데이터 파일(.pst 및 .ost) 소개](https://support.office.com/ko-kr/article/outlook-%EB%8D%B0%EC%9D%B4%ED%84%B0-%ED%8C%8C%EC%9D%BC-pst-%EB%B0%8F-ost-%EC%86%8C%EA%B0%9C-222eaf92-a995-45d9-bde2-f331f60e2790)


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
