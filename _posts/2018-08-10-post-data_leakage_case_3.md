---
title: "운영체제 OS 정보"
categories:
  - Data Leakage Case
tags:
  - OS
  - Registry
  - Windows Artifacts
comments: true
---

#3. Explain installed OS information in detail. (OS name, install date, registered owner…)

윈도우 OS 정보는 다음 레지스트리 경로에 있다.
```
HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion
```

> Tools
- 이미지 내 레지스트리 획득 : Arsenal Image mounter, WinHex 
- 레지스트리 분석 : registry explorer

# 1. Arsenal Image Mounter 로 이미지 파일 마운팅하기
1.1. 왼쪽 하단의 mount image 를 클릭하고, 이미지 파일을 선택한다.
1.2. 다음과 같이 이미지 파일이 마운트 되어 있는 것을 확인할 수 있다.
1.3. 이미지 마운팅을 하지 않고, Winhex 에서 이미지를 바로 연후, [Specialist]-[Interpret Image File As Disk]를 하여도 된다.

# 2. Winhex 에서 레지스트리 파일 추출하기
2.1. [Tools]-[Open disk]
2.2. Arsenal Virtual 클릭
2.3. 레지스트리 파일 추출

[(FP) 레지스트리 포렌식과 보안 (Registry Forensics)](https://github.com/proneer/Slides/tree/master/Windows)를 참고하면, 레지스트리 파일은 hive 파일 형태로 저장되어 있으며, 윈도우 7의 레지스트리 파일 경로는 다음과 같다.

- 운영체제 하이브 파일 위치 : `%SystemRoot%\System32\Config`
- %UserProfile% 목록 확인 : `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList`
- HKLM 레지스트리 경로별 하이브 파일 위치
  - HEKY_LOCAL_MACHINE\COMPONENTS : `%SystemRoot%\System32\Config\COMPONENTS`
  - HEKY_LOCAL_MACHINE\SYSTEM : `%SystemRoot%\System32\Config\SYSTEM`
  - HEKY_LOCAL_MACHINE\SAM : `%SystemRoot%\System32\Config\SAM`
  - HEKY_LOCAL_MACHINE\SECURITY : `%SystemRoot%\System32\Config\SECURITY`
  - HEKY_LOCAL_MACHINE\SOFTWARE : `%SystemRoot%\System32\Config\SOFTWARE`
- USER 관련 레지스트리 경로별 하이브 파일 위치
  - HKEY_USERS\<SID of local service account> `%SystemRoot%\ServiceProfiles\LocalService\NTUSER.DAT`
  - HKEY_USERS\<SID of network service account> `%SystemRoot%\ServiceProfiles\NetworkService\NTUSER.DAT`
  - HKEY_USERS\<SID of username> `%UserProfile%\NTUSER.DAT`
  - HKEY_USERS\<SID of username>_Classes `UserProfile%\AppData\Local\Microsoft\Windows\Usrclass.dat`
  - HKEY_USERS\.DEFAULT `%SystemRoot%\System32\Config\DEFAULT`

WinHex에서 partition#2의 `\Windows\System32\config` 에서 components, system, sam, security, software 5개의 파일을 추출한다.
[우클릭] - [Recover/Copy]

# 3. Registry Explorer 를 통한 분석
3.1. [File]-[Load offline hive]
3.2. Winhex 에서 추출한 5개의 파일을 로드한다.
3.3. 5개의 파일이 다 로드되면 다음과 같다.
3.4. HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion 경로의 registry 를 보면 다음과 같다.
- ProductName : Windows 7 Ultimate
- InstallDate : 1427034866(unix time) = Sunday, March 22nd 2015, 14:34:26 (GMT+0)
- RegisteredOwner : informant



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
