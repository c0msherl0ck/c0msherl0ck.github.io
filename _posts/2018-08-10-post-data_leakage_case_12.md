---
title: "윈도우 이벤트 로그(System on/off)"
categories:
  - Data Leakage Case
tags:
  - Event Log
  - Windows Artifacts
comments: true
---

CFReDS-Data Leakage Case #12. List all traces about the system on/off and the user logon/logoff. (It should be considered only during a time range between 09:00 and 18:00 in the timezone from Question 4.)

> 1. Microsoft Message Analyzer
> 2. LogParserStudio

|Event Number|Role|
|---|---|---|
|4624|An account was successfully logged on|
|4625|An account failed to log on|
|4647|User initiated logoff|
|4648|A logon was attempted using explicit credentials|
|4608|Windows is starting up|
|4609|Windows is shutting down|

# 1. Microsoft Message Analyzer

`1.1.` 분석 PC 이벤트 로그를 다음의 경로에서 추출한다.

`%systemroot%\System32\winevt`
<center><img src="/assets/2018-08-10-post-data_leakage_case_12/1.1.jpg"></center>

`1.2.` Microsoft message analyzer 를 통해 해당 로그를 불러온 후 다음과 같이 필터링 한다.
<div class="notice">
*eventId == 4624 or *eventId == 4647 or *eventId == 4608 or *eventId == 4609
</div>
여기서 보여지는 timestamp 값은, HOST PC 의 timezone 에 해당하는 값으로, Target PC의 Time zone을 적용하기 위해서는 -13 hour 를 하면 된다.

<center><img src="/assets/2018-08-10-post-data_leakage_case_12/1.2.jpg"></center>

`1.3.` 우측상단의 Export 메뉴를 통해, 결과파일을 다음과 같이 .csv 파일로 export 할 수 있다.



# 2. LogParserStudio

`2.1.` 분석할 이벤트 로그를 다음과 같이 선택한다. (Choose log files/folders to query)

Add folder 메뉴는 버그로 작동하지 않아, Add files 를 통해 모든 파일을 불러온다.

<center><img src="/assets/2018-08-10-post-data_leakage_case_12/2.1.jpg"></center>

`2.2.` "Create a new query" 를 통해 새로운 질의창을 생성하고, Log Type: EVTLOG 를 선택한다.

<center><img src="/assets/2018-08-10-post-data_leakage_case_12/2.2.jpg"></center>

`2.3.` 다음의 쿼리를 실행한다.
<div class="notice">
SELECT * FROM '[LOGFILEPATH]' <br>
WHERE (EventID = 4624) OR (EventID = 4647) OR (EventID = 4608) OR (EventID = 4609) <br>
ORDER BY TimeGenerated ASC
</div>

<center><img src="/assets/2018-08-10-post-data_leakage_case_12/2.3.jpg"></center>

`2.4.` CSV 파일로 export 한다.

한글이 깨질 경우, 메모장으로 연 후 "utf-8" 인코딩으로 다른이름저장하면 한글이 깨지지 않는다.

<center><img src="/assets/2018-08-10-post-data_leakage_case_12/2.4.jpg"></center>

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
