---
title: "프로그램 설치 및 실행 흔적 분석"
categories:
  - Data Leakage Case
tags:
  - Registry
  - LNK
  - Prefetch
  - Shimcache
comments: true
---

CFReDS-Data Leakage Case #10 ~ 11 #10. What applications were installed by the suspect after installing OS? #11. List application execution logs.

> 1. 레지스트리 분석(UserAssist, RunMRU, Uninstall)
> 2. 링크파일 분석(LinkParser)
> 3. 프리패치 분석(WinPrefetchView)
> 4. 호환성 아티팩트 분석(ShimcacheParser)
> 5. 참고

# 1. 레지스트리 분석

|종류|레지스트리 경로|
|---|---|
|프로그램 설치 흔적|HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Uninstall|
|실행파일 실행 기록|HKU\{USER}\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\{CEBFF5CD-ACE2-4F4F-9178-9926F41749EA}\Count|
|바로가기 실행 기록|HKU\{USER}\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\{F4E57C4B-2036-45F0-A9AB-443BCFE33D9F}\Count|
|가장 최근에 실행되었던 프로그램|HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU|

`1.1.` 프로그램 설치 흔적 확인

<center><img src="/assets/2018-08-11-post-data_leakage_case_10-11/1.1.jpg"></center>

`1.2.` 실행파일 실행 기록 확인

<center><img src="/assets/2018-08-11-post-data_leakage_case_10-11/1.2.jpg"></center>

`1.3.` 바로가기 실행 기록 확인

<center><img src="/assets/2018-08-11-post-data_leakage_case_10-11/1.3.jpg"></center>

`1.4.` 가장 최근에 실행되었던 프로그램 확인

<center><img src="/assets/2018-08-11-post-data_leakage_case_10-11/1.4.jpg"></center>


# 2. 링크파일(.lnk) 분석

링크파일이란?

LNK 파일이라고 하면 특정 응용프로그램을 설치했을 때 바탕화면에 자동으로 생성되거나, 사용자가 편의를 위해 바로가기를 생성했을 때 생기는 파일을 떠올릴 수 있다. 하지만, 바탕화면(Desktop) 외에도 "최근 문서 폴더(Recent)", "시작프로그램(Start)", "빠른실행(QuickLaunch)" 등 다양한 곳에서 수동 혹은 자동으로 생성되어 저장된다.

|종류|경로(Windows 7)|
|---|---|
|최근문서(Recent) 폴더|C:\Users<user name>\AppData\Roaming\Microsoft\Windows\Recent|
|바탕화면(Desktop) 폴더|C:\Users\<user name>\Desktop|
|시작프로그램(Start) 폴더|C:\Users\<user name>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs|
|빠른실행(QuickLaunch) 폴더|C:\Users\<user name>\AppData\Roaming\Microsoft\Internet Explorer\Quick Launch|

`2.1.` Winhex 에서 Explore recursively 기능을 통해 partition 2 에 있는 모든 파일을 탐색한다.

<center><img src="/assets/2018-08-11-post-data_leakage_case_10-11/2.1.jpg"></center>

`2.2.` 확장자로 정렬하여 .lnk 파일을 모두 추출한다.

<center><img src="/assets/2018-08-11-post-data_leakage_case_10-11/2.2.jpg"></center>

`2.3.` LinkParser 툴을 이용하여 추출한 링크 파일들이 있는 폴더를 불러온다.

<center><img src="/assets/2018-08-11-post-data_leakage_case_10-11/2.3.jpg"></center>

`2.4.` LinkCreationDate 로 정렬하여 프로그램 실행흔적을 분석한다.<br>
(`링크가 생성된 시점이 파일이 실행된 시점이기 때문이다.`)

<center><img src="/assets/2018-08-11-post-data_leakage_case_10-11/2.4.jpg"></center>


# 3. 프리패치(.pf) 분석

프리패치 파일이란?

윈도우는 프리패치 파일에 실행 파일이 사용하는 시스템 자원을 미리 저장하였다가 윈도우 부팅 시 프리패치 파일(.pf)을 모두 메모리에 로드한 후 실제 사용할 때에는 메모리 매핑만을 수행하여 사용할 수 있도록 하여 실행 속도를 다소 향상시켰다.


프리패치 파일을 분석하면 다음을 알 수 있다.

- 실행 파일 이름
- 실행 파일의 실행 횟수
- 실행 파일의 마지막 실행 시간
- 프리패치 파일의 생성 시간(최초 실행 시간)
- 실행된 볼륨의 정보
- 실행파일 실행 시 참조하는 파일의 목록

`3.1.` Winhex 에서 분석 PC 의 Prefetch 폴더를 [우클릭]-recover/copy 한다.

<center><img src="/assets/2018-08-11-post-data_leakage_case_10-11/3.1.jpg"></center>

`3.2.` WinPrefetchView 프로그램에서 해당 폴더를 다음과 같이 불러온다.

<center><img src="/assets/2018-08-11-post-data_leakage_case_10-11/3.2.1.jpg"></center>

<center><img src="/assets/2018-08-11-post-data_leakage_case_10-11/3.2.2.jpg"></center>

`3.3.` Last Run Time 을 기준으로 정렬한다.

- Last Run Time 은 분석을 진행하는 PC의 Time zone 이 적용된 값을 보여준다.(Korea Standard Time UTC+9)
  - 분석 PC 의 Time zone 은 Eastern Standard Time (UTC-5) 에 daylight time bias -60 minutes (+1 hour) 적용한 UTC-4 이다. 그러므로, 현재 보여지는 Last Run Time에서 -13 hour 를 해준값이, 분석 PC 의 time zone 에 해당하는 값이 된다.
- Created Time, modified time 등은 분석이미지에서 host PC 로 recover/copy 할때의 시간값이므로, 분석에 의미없는 값이다.

<center><img src="/assets/2018-08-11-post-data_leakage_case_10-11/3.3.jpg"></center>

`3.4.` 다음과 같이 HTML Report 로 따로 뽑아낼 수도 있다.

<center><img src="/assets/2018-08-11-post-data_leakage_case_10-11/3.4.jpg"></center>


# 4. 호환성 아티팩트(Shimcache, AppCompatCache) 분석

ShimCache(=AppCompatCache) 호환성 아티팩트란?

- 마이크로소프트 사에서 응용 프로그램 간 호환성을 제어하고, 트러블 슈팅과 문제 해결을 위해 만든 파일이다.
- OS에 따라 다르지만, 파일 경로/ 크기/ 마지막 수정시간/ 마지막 실행 시간등의 정보를 가지고 있다.

<div class="notice">
"응용프로그램의 실행 정보는 보통 프리패치(prefetch) 정보를 확인하면 알 수 있지만 프리패치의 개수는 한정되어 있기 때문에 오래된 흔적을 찾기는 어렵다. 또한, 최근 악성코드는 프리패치와 같이 알려진 흔적을 모두 삭제하는 경향이 있다. 이런 상황에서 호환성 캐시 정보는 분석에 매우 중요하게 사용될 수 있다. 특히, 시간 정보가 포함되기 때문에 더할 나위없이 유용하다. 뛰어난 공격자라면 각 운영체제 버전에 맞게 호환성 문제가 없는 프로그램을 사용하겠지만, APT 공격과 같이 오랫동안 여러 시스템을 감염시켜야 하는 경우 시스템 버전이 다양하기 때문에 호환성 캐시에 흔적이 저장될 가능성이 매우 크다."

[출처] http://forensic-proof.com/archives/3397 "응용프로그램 호환성 아티팩트 (Application Compatibility Artifacts)"
</div>

- HKCU\Software\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\*
- HKLM\Software\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\*
- HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache\AppCompatCache
- %SystemRoot%\Windows\AppCompat\Programs\*


`4.1.` Eric Zimmerman's tools 의 AppCompatCacheParser (a.k.a shimcacheparser)사용한다.

<center><img src="/assets/2018-08-11-post-data_leakage_case_10-11/4.1.jpg"></center>

`4.2.` 프로그램 설명

<center><img src="/assets/2018-08-11-post-data_leakage_case_10-11/4.2.jpg"></center>

`4.3.` 다음의 명령어를 통해 분석 대상 PC의 SYSTEM 레지스트리에서 shimcache 관련 정보를 추출한다.
```
AppCompatCacheParser.exe --csv [결과파일 경로] -t --f [SYSTEM 레지스트리 하이브 파일 경로]
```
<center><img src="/assets/2018-08-11-post-data_leakage_case_10-11/4.3.jpg"></center>

`4.4.` 생성된 결과파일을 확인하면 다음과 같다.

<center><img src="/assets/2018-08-11-post-data_leakage_case_10-11/4.4.jpg"></center>


# 5. 참고

https://www.cfreds.nist.gov/data_leakage_case/data-leakage-case.html

https://m.blog.naver.com/PostView.nhn?blogId=s2kiess&logNo=220796244110&proxyReferer=https%3A%2F%2Fwww.google.co.kr%2F "UserAssist를 통한 파일실행 흔적 분석"

http://solatech.tistory.com/167 "윈7에서의 레지스트리 정보 분석"

http://forensic-proof.com/archives/607 "LNK (Windows Shortcut) 파일 포렌식 분석"

http://portable-forensics.blogspot.com/2014/11/prefetch-and-superfetch.html "프리패치와 슈퍼패치(Prefetch and Superfetch)"

https://m.blog.naver.com/PostView.nhn?blogId=nobless_05&logNo=50192638786&proxyReferer=https%3A%2F%2Fwww.google.co.kr%2F "[윈도우 포렌식] 파일 실행 흔적 - AppCompatCache Registry"

http://forensic-proof.com/archives/3397 "응용프로그램 호환성 아티팩트 (Application Compatibility Artifacts)"

http://forensic-proof.com/archives/5819 "응용프로그램 호환성 아티팩트 II (Application Compatibility Artifacts II)"

https://blog.1234n6.com/2018/10/available-artifacts-evidence-of.html?m=1&fbclid=IwAR3d-84boV0blmoIcjtWTWfBJZRIOWl-dp5TVtUedZkqXONFuWmDHOqppx8 "Available Artifacts - Evidence of Execution"


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
