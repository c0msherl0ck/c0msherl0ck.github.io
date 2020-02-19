---
title: "[레지스트리] TimeZone"
categories:
  - Data Leakage Case
tags:
  - E01
  - DD
  - Hash
comments: true
---

CFReDS-Data Leakage Case #4. What is the timezone setting?

윈도우에서 TimeZone information는 다음의 레지스트리에 존재한다.
```
HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation
```

> 1. CurrentControlSet 파악하기
> 2. 시간값 해석하기

# 1. CurrentControlSet 파악하기

CurrentControlSet은 현재 로그인한 유저의 세팅값(Hardware profile 등)을 저장하고 있으며, 오프라인 상태에서는 ControlSetXXX을 통해 분석한다. ControlSetNumber는 아래와 같이 `HKLM\SYSTEM\Select의 `Current Value`에 저장되어 있다. 해당 값이 1이라면 ControlSet001을, 2라면 ControlSet002를 분석한다.

<center><p><img src="/assets/2018-08-10-post-data_leakage_case_4/1.jpg"></p></center>

# 2. 시간값 해석하기

`HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation` 레지스트리 값을 해석하면 다음과 같다.

<div class="notice">
Bias : 300 minutes
DaylightBias : -60 minutes
ActiveTimeBias = Bias + DaylightBias = 240 minutes

즉, **UTC-4** 가 timezone 값이다.
[주의] UTC+4 가 아닌 UTC-4 임에 유의한다.
</div>

<center><p><img src="/assets/2018-08-10-post-data_leakage_case_4/2.jpg"></p></center>

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
