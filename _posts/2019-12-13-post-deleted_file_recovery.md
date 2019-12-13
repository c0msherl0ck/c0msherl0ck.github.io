---
title: "[데이터 복구] 사용자 삭제 행위 별 복구 방법"
categories:
  - Data Recovery
tags:
  - 휴지통
  - Delete
  - Shift + Delete
  - Eraser
  - $MFT
  - EnCase
  - Winhex
  - R-Studio
comments: true
---

윈도우 환경에서 사용자가 파일을 삭제하는 방법은 크게 3가지(`Delete`/`Shift + Delete`/`삭제 도구 이용`)로 분류할 수 있다. 이번 글에서는 윈도우 `NTFS` 파일 시스템에서 사용자 삭제 행위 별 복구 방법에 대해 알아본다.

> 1. Delete & 휴지통 비우기
> 2. Shift + Delete
> 3. Eraser(삭제 도구)


# 1. Delete & 휴지통 비우기

우클릭 파일 삭제 또는 Delete 키를 통한 삭제는 **단순히 휴지통이라는 폴더에 파일을 이동**하는 것과 동일하며, 휴지통 폴더 경로는 다음과 같다.

```C:\$Recycle.Bin\[User SID]```

윈도우 탐색기 창에서 확인 시 사용자 편의를 위해 `로그인 사용자의 휴지통`은 User SID 대신 휴지통이라는 이름으로 수정해서 보여주는 것이며, FTK Imager를 통해 확인할 경우 원래 폴더명인 `User SID(Security Identifier)`을 확인할 수 있다.

일반적인 업무용 PC의 경우 사용자 계정이 1명이므로 휴지통 폴더가 1개만 생성되지만, 서버와 같이 여러 사용자가 사용할 경우 **사용자별로 휴지통 폴더가 생성**된다.

<center><p><img src="/assets/2019-12-13-post-deleted_file_recovery/윈도우 탐색기 휴지통.jpg" width="80%"><br><em>윈도우 탐색기로 확인한 서버 휴지통</em></p></center>
<center><p><img src="/assets/2019-12-13-post-deleted_file_recovery/FTK Imager 휴지통.jpg" width="50%"><br><em>FTK Imager로 확인한 서버 휴지통</em></p></center>

사용자에 부여된 SID는 다음과 같이 확인 가능하다.

<center><p><img src="/assets/2019-12-13-post-deleted_file_recovery/SID.jpg" width="80%"><br><em>SID 확인</em></p></center>

휴지통으로 파일 이동(파일 삭제) 시, **$R~**(`실제 데이터 / Real`), **$I~**(`삭제 전 파일 경로 등 메타 데이터 / Information`) 2개의 파일로 구분되어 저장되며, **휴지통에서 파일 복원 시 해당 정보를 이용하여 삭제 전 경로에 파일을 복원한다.** EnCase의 경우 $R~ 파일에 대응되는 $I~ 파일의 정보를 이용하여, 삭제 전 을 표시하는 기능을 제공한다.(FTK Imager는 지원하지 않는 기능)

<center><p><img src="/assets/2019-12-13-post-deleted_file_recovery/휴지통 파일 비교_EnCase_FTK_imager.jpg" width="100%"><br><em>EnCase vs FTK Imager</em></p></center>

`휴지통 비우기`를 하는 것은, 휴지통 폴더의 파일을 삭제하는 것이다. 이 때 삭제란 `$MFT에서 해당 파일과 관련된 Records를 삭제하고, 데이터가 차지하고 있는 영역을 Unallocated로 변경하여, 해당 영역에 다른 데이터가 저장 가능하도록 하는 것`을 의미힌다.

그러므로, 만약 $R~, $I~ 파일이 해당하는 영역(비할당 영역)에 다른 데이터가 덮어씌워지지 않았다면, **$R~(데이터), $I(데이터 위치 및 파일 경로) 파일 정보를 이용**하여 **File Path(파일명, 폴더구조), Data(내용)**를 복구할 수 있다.

<br>

# 2. Shift + Delete

`Shift + Delete`를 통한 삭제는 앞서 기술한 `휴지통 비우기`와 **같은 방법**으로 $MFT에서 해당 파일과 관련된 Records를 삭제하고, 데이터가 차지하고 있는 영역을 Unallocated로 변경하여, 해당 영역에 다른 데이터가 저장 가능하도록 한다.

**그러나,** $R~, $I~ 파일이 없기 때문에, 삭제된 비할당 영역에 대한 정보(데이터 위치 및 파일 이름)를 알기 위해서는 **$MFT 파일 내에서 삭제된 Records를 분석**해야 한다. EnCase 에서는 $MFT 분석을 통해 파일 복구를 지원하며, **File Path** 확인이 가능하다.

<center><p><img src="/assets/2019-12-13-post-deleted_file_recovery/EnCase Recovery.jpg" width="60%"><br><em>EnCase - Recovering Folders</em></p></center>

`Winhex` 도구의 `File Recovery by Type` 기능을 이용하면 **$MFT 파일을 분석하지 않고** 파일 복구가 가능하다(**단,** 폴더는 복구 대상이 아님). 해당 기능은 디스크 전체에서 파일 시그니처 기반의 파일 카빙을 통해 특정 파일 유형(Pictures, Documents, E-mail 등)을 선택하여 **다수의 파일들**을 복구하고, 그 중 찾는 파일이 있는지 **일일이 내용을 확인**하는 방식으로 진행한다(**단,** $MFT를 분석하지 않았기 때문에 파일 이름은 알 수 없음).

<center><p><img src="/assets/2019-12-13-post-deleted_file_recovery/Winhex Recovery.jpg" width="70%"><br><em>Winhex - File Recovery by Type</em></p></center>

`R-studio`는 앞서 말한 EnCase와 Winhex 파일 복구 방법 2가지를 모두 갖추고 있다.

[데이터 복구 프로그램 R-STUDIO 사용법](https://uuu8uuu.tistory.com/361)

|도구|EnCase|Winhex|R-Studio|R-Studio|
|:---:|:---:|:---:|:---:|:---:|
|기능|Recovering Folders|File Recovery by Type|Recognized2|Raw Files|
|복구 방법|$MFT 분석|File Signature 분석|$MFT 분석|File Signature 분석|
|복구 대상|폴더 및 파일|특정 유형 파일|폴더 및 파일|특정 유형 파일|
|File Path(파일명, 폴더구조)|△|X|△|X|
|Data(파일 내용)|△|△|△|△|

*△ : 해당 영역이 덮어씌워지지 않았을 경우 복구 가능*

<div class="notice">
$MFT에서 파일 시스템 로그(파일 생성, 삭제, 수정 등)를 분석하여 File Path를 알 수 있지만, 기간이 오래 되면(약 일주일 이상) 복구할 수 없다. 사용자의 PC 사용 습관에 따라 차이가 있지만, PC가 켜져 있을 경우 시스템 파일들이 계속해서 생성 및 삭제되기 때문에 $MFT 내 삭제된 Records 흔적들이 덮어씌워지기 때문이다.
</div>

<br>

# 3. Eraser(삭제 도구)

Eraser 삭제 도구 이용 시, 데이터 영역을 난수로 덮어씌우기 때문에 **데이터 복구는 불가능**하다. 그러나, $MFT 파일 분석을 통해 Eraser를 통해 삭제한 파일의 **File Path(파일명, 폴더구조)**는 확인할 수 있다. $MFT 분석에는 `NTFS Log Tracker`를 사용한다.

[blueangel's ForensicNote - NTFS Log Tracker](https://sites.google.com/site/forensicnote/ntfs-log-tracker)

<br>

# 참고

[데이터 복구 프로그램 R-STUDIO 사용법](https://uuu8uuu.tistory.com/361)<br>
[blueangel's ForensicNote - NTFS Log Tracker](https://sites.google.com/site/forensicnote/ntfs-log-tracker)

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
