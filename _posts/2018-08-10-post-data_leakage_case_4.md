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


# 2. 시간값 해석하기


<center><p><img src="/assets/2018-08-10-post-data_leakage_case_1/5.jpg"></p></center>

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
