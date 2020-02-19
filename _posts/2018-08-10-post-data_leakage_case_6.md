---
title: "[레지스트리] 윈도우 계정 정보"
categories:
  - Data Leakage Case
tags:
  - Registry
  - Windows Artifacts
comments: true
---

CFReDS-Data Leakage Case #6. List all accounts in OS except the system accounts: Administrator, Guest, systemprofile, LocalService, NetworkService. (Account name, login count, last logon date…)

다음의 두 레지스트리에서 사용자 계정 정보를 확인할 수 있다.
```
HKEY_LOCAL_MACHINE\SAM\SAM\Domains\Account\Users
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList
```

> 1. Users
> 2. ProfileList
> 3. SID

# 1. Users

`HKEY_LOCAL_MACHINE\SAM\SAM\Domains\Account\Users` 키 값에서는 계정명과 각 계정에 해당하는 부가적인 정보들을 확인할 수 있다.

- `계정명` : informant, temporary, ITech Team, admin11, Guest, Administrator
- `부가적인 정보` : login count, last logon date, created on time, total login count, invalid login count 등

<center><img src="/assets/2018-08-10-post-data_leakage_case_6/1.jpg"></center>


# 2. ProfileList

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList` 키 값에서는 system, local, network 서비스들을 수행하기 위해서 필요한 계정과 해당 계정의 SID 값을 확인할 수 있다.

|SID|User Folder Path|
|---|---|
|S-1-5-18|%systemroot%\System32\config\systemprofile|
|S-1-5-19|%systemroot%\ServiceProfiles\LocalService|
|S-1-5-20|%systemroot%\ServiceProfiles\NetworkService|
|S-1-5-{21-xxx}-1002|C:\Users\informant|
|S-1-5-{21-xxx}-1001|C:\Users\admin11|
|S-1-5-{21-xxx}-1003|C:\Users\temporary|

<center><img src="/assets/2018-08-10-post-data_leakage_case_6/2.jpg"></center>


# 3. SID

SID(Security Identifier)란 윈도우에서 계정을 하나의 코드 값으로 표시한 것이다.

S-1-5-{Sub-authority value}-{relative ID(RID)}

1) S : SID 를 의미한다.
2) 1 : revision number(SID 명세버전)
3) 5 : Identifier authority value(48bit, 윈도우 보안 권한)
4) Sub-authority value
- 도메인 또는 로컬 컴퓨터 구분자이다.
- 시스템의 고유한 숫자로, 시스템을 설치할 때 시스템의 특성을 수집하여 생성된다.
5) relative ID(RID)
- 기본적으로 생성되는 builtin 계정이 아니라면, 1000보다 큰 숫자의 RID 가 생성된다.
- 관리자(Administrator)는 500번, Guest 계정은 501번, 일반 사용자는 1000번 이상의 숫자를 갖는다.

|SID|Description|
|---|---|
|S-1-0-0|SID 를 모를 때 사용하는 SID|
|S-1-0-1|Everyone|
|S-1-5-7|Anonymous|
|S-1-5-18|System Profiles(시스템의 서비스용 계정이다)|
|S-1-5-19|Local Service|
|S-1-5-20|Network Service|
|S-1-5-domain-500|Administrator|
|S-1-5-domain-501|Guest|

[출처] 알기사 2018 정보보안기사&산업기사 윈도우 서버 보안 편

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
