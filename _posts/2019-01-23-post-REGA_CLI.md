---
title: "REGA.exe 후킹(Hooking)하기"
categories:
  - Reversing
tags:
  - x64Dbg
  - REGA
  - Hooking
comments: true
---

REGA 도구를 후킹하여 CLI 명령어를 따서, GUI 기반의 해당 프로그램을 직접 실행시키지 않고, 커맨드 창에서 명령어를 통해 레지스트리 정보를 수집하는 방법에 대해 기술한다.

> 1. REGA 실행 후, 기능 살펴보기
> 2. x64Dbg 를 통한 후킹
> 3. 명령어 작동 확인

# 1. REGA 실행 후, 기능 살펴보기

REGA 를 실행시킨 후, [파일]-[레지스트리 파일 수집]-[현재 시스템 레지스트리 수집] 을 클릭하면, 현재 시스템의 레지스트리를 수집한다.

<center><p><img src="/assets/2019-01-23-post-REGA_CLI/1-1.jpg"></p></center>

<center><p><img src="/assets/2019-01-23-post-REGA_CLI/1-2.jpg"></p></center>

<center><p><img src="/assets/2019-01-23-post-REGA_CLI/1-3.jpg"></p></center>

# 2. x64Dbg 를 통한 후킹

> x64 Dbg 를 이용하여 REGA.exe 를 불러온 다음 F9 을 통해 실행시킨다.
(REGA 를 실행시킬 때는 관리자 권한이 필요하므로, x64 Dbg 또한 관리자 권한으로 실행시켜야 한다.)

> Ctrl + G 를 통해 KERNEL32.CreateProcessA, KERNEL32.CreateProcessW 를 검색한 후 해당부분에 Break point 를 설정한다.

**※ CreateProcessA, CreateProcessW 에 Break Point 를 설정하는 이유는 [현재 시스템 레지스트리 수집] 클릭 시, "폴더 찾아보기" 창이 생성되기 때문이며, 이러한 기능을 하는 것이 위의 두 함수이다.**

<center><p><img src="/assets/2019-01-23-post-REGA_CLI/2-2.jpg"></p></center>


> Break Point 설정 이후, F9 를 통해 진행을 시키면 다음과 같은 "폴더 찾아보기" 창이 뜬다. 현재 시스템에서 레지스트리를 수집하여 저장할 폴더 위치를 물어보는 것이다.

<center><p><img src="/assets/2019-01-23-post-REGA_CLI/2-3.jpg"></p></center>

> 폴더 지정 후 확인버튼을 누르게 되면, 다음처럼 CreateProcessW 에서 멈추며, 이때 DUMP 창과 STACK 창에서 CLI 명령어를 확인 할 수 있다.

<center><p><img src="/assets/2019-01-23-post-REGA_CLI/2-4.jpg"></p></center>

`[DUMP 창]`

<center><p><img src="/assets/2019-01-23-post-REGA_CLI/2-5.jpg"></p></center>

`[STACK 창]`

<center><p><img src="/assets/2019-01-23-post-REGA_CLI/2-6.jpg"></p></center>

```
[CLI 명령어]

"C:\Users\holywater\Desktop\#Reversing\REGA_CLI\REGA Release\RegEx.exe" -h "C:\\Users\\holywater\\Desktop\\#Reversing\\REGA_CLI\\REGA Release\\registry"
```

<div class="notice">
[주의 1]<br>
<br>
STACK 창에서 보았을때 나타나는 표현을 CMD 창에 그대로 입력하면 오류가 난다. 결국엔 DUMP 에 있는 것을 x64Dbg 가 해석해서 보여주는 값이기 때문에 경우에 따라 부정확할 수 있으므로, STACK 창에서 해당 주소 00FECAD8 의 주소 값을 DUMP 창에서 Ctrl + G 를 통해 찾아가 정확한 표현을 확인하는 것이 중요하다.
<br>
[주의 2]<br>
<br>
폴더 경로에 한글이 섞여있을 경우 STACK 창에서 아무것도 안보이는 현상이 발생하므로, 컴공생이라면 폴더경로에 한글을 쓰지 않는다.
<br>
[교훈]<br>
<br>
Immunity Dbg 와 x64Dbg 두 가지 도구를 통해 코드 analysis 를 진행했을 때 해석되는 정도가 다르다. CreateProcessW, CreateProcessA  를 찾는 것은 x64Dbg 를 통해 보는 것이 편한 반면, 어셈블리 함수 단위 해석은 Immunity Dbg 이 더 낫다. 두 가지 도구 모두 이용해서 분석을 진행해야 한다.
</div>

# 3. 명령어 작동 확인

<center><p><img src="/assets/2019-01-23-post-REGA_CLI/3.jpg"></p></center>

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

