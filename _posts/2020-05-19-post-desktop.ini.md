---
title: "desktop.ini 설정 변경을 통한 Folder Customizing"
categories:
  - Windows
tags:
  - System Files
comments: true
---

Desktop.ini란 윈도우 시스템 파일로 사용자 정의 아이콘, 폴더명 등을 저장하는 '폴더 설정 파일'이다.
이번 글에서는 desktop.ini 파일의 값을 변경함으로써 폴더명을 사용자 커스텀(Cutomizing)하는 방법에 대해 기술한다.

> 1. 파일 확인
> 2. Folder Customizing
> 3. 참고


# 1. 파일 확인

Desktop.ini 파일은 윈도우에서 기본 폴더 내 Default로 존재하며, 사용자가 임의로 특정 폴더 내에도 생성할 수 있다.
해당 파일에는 폴더에 대한 설정값이 저장되며, 상세 스펙은 [Desktop.ini Documentation](https://hwiegman.home.xs4all.nl/desktopini.html)에서 확인할 수 있다.

<center><p><img src="/assets/desktop.ini/1. 기본폴더.jpg"></p></center>

<center><p><img src="/assets/desktop.ini/2. 파일내용.jpg"></p></center>


# 2. Folder Customizing

테스트를 위해 다음과 같이 임의의 폴더를 구성한다.

<center><p><img src="/assets/desktop.ini/3. 변경전.jpg"></p></center>

test 폴더 내 desktop.ini 파일을 생성하고, 메모장으로 `LocalizedResourceName` 값을 변경한다.

<center><p><img src="/assets/desktop.ini/4. 커스터마이징.jpg"></p></center>

설정값 변경 적용을 위해, test 폴더의 속성에서 `기본값 복원`을 클릭하고 적용한다.

<center><p><img src="/assets/desktop.ini/5. 적용.jpg"></p></center>

다음과 같이 파일 시스템 상에서는 test 폴더라는 이름은 그대로이나, 
윈도우 탐색기 창에서 표시되는 폴더명 `LocalizedResourceName` 에서 설정한 값으로 변경된 것을 알 수 있다.

<center><p><img src="/assets/desktop.ini/6. 변경후.jpg"></p></center>

위 원리를 이용하면 기본폴더에 해당하는 폴더들이 사용자 환경에 따라서 각각 다른 언어로 표시되도록 할 수 있다.


# 참고

<p>
다운로드폴더가 일반폴더처처럼 영문으로 바뀌었을 때 해결방법, (https://answers.microsoft.com/ko-kr/windows/forum/windows_7-files/%EB%8B%A4%EC%9A%B4%EB%A1%9C%EB%93%9C/cdf75cc7-58b9-49ab-8262-771bf9a894da)
How to Customize Folders with Desktop.ini, https://docs.microsoft.com/en-us/windows/win32/shell/how-to-customize-folders-with-desktop-ini
Desktop.ini Documentation, https://hwiegman.home.xs4all.nl/desktopini.html
폴더 이름을 desktop.ini로 다른 이름으로 표시하기, https://studioxga.net/1430
</p>

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
