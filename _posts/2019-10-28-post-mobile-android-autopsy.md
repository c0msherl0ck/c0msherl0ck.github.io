---
title: "[모바일 포렌식] Autopsy 도구를 이용한 안드로이드 이미지 분석"
categories:
  - Mobile Forensic
tags:
  - mobile
  - forensic
  - android
  - samsung galaxy
  - autopsy
---

Autopsy는 EnCase와 같은 통합 포렌식 분석 도구이며 무료이다. 흔히 무료라고 하면 기능상의 제약이 있을 것 같지만, 안드로이드 이미지 프로세싱에 적합한 android analyzer 등 강력한 기능을 제공한다.

> Autopsy 를 이용한 안드로이드 이미지 분석

이전글 [안드로이드 핸드폰 이미지 획득](https://c0msherl0ck.github.io/mobile%20forensic/post-mobile-android/)에서 획득한 이미지를 분석한다.

Autopsy 도구는 매우 직관적이고 간편하므로 별도의 설명없이 다음의 그림 순서에 따라 분석을 진행한다.

<center><p>
<img src="/assets/2019-10-28-post-mobile-android-autopsy/1.jpg" width="100%">
<em></em>
</p>Case Information</center>

<center><p>
<img src="/assets/2019-10-28-post-mobile-android-autopsy/2.jpg" width="100%">
<em></em>
</p>Optional Information</center>

<center><p>
<img src="/assets/2019-10-28-post-mobile-android-autopsy/3.jpg" width="100%">
<em>Select Type of Data Source</em>
</p></center>

<center><p>
<img src="/assets/2019-10-28-post-mobile-android-autopsy/4.jpg" width="100%">
<em>Select Data Source</em>
</p></center>

<center><p>
<img src="/assets/2019-10-28-post-mobile-android-autopsy/android_analyzer.jpg" width="100%">
<em>Configure Ingest Modules(processing option)에서 android analyzer 선택</em>
</p></center>

<center><p>
<img src="/assets/2019-10-28-post-mobile-android-autopsy/processing.jpg" width="100%">
<em>Processing 상태</em>
</p></center>

> 결과 분석

Processing 이 종료되면 다음과 같이 카테고리 별 분석 결과를 알 수 있다. 필자의 경우 2014년도 부터 kitri를 북마크에 추가하며 관심을 가지고 있었고, 2018 년도에 kitri 주관 보안 교육 프로그램에 참가하였다. 면접 당시 과거에 사용하던 핸드폰을 포렌식 분석하여 kitri 에 대한 관심을 어필하는 것은 어떨까 싶다. 이 글을 읽는 사람들 중 `BoB 디지털 포렌식 트랙` 지원을 희망하는 사람이 있다면 참고해보길..

<center><p>
<img src="/assets/2019-10-28-post-mobile-android-autopsy/bookmark.jpg" width="100%">
<em>bookmark</em>
</p></center>

> 비할당 영역

dd 방식의 이미징은 물리적 이미지 획득 방법으로 비할당 영역에서의 복구가 가능하다. 다음의 그림에서 선택한 파일은 "비지니스_Tool_요약.docx" 이지만, 해당 파일이 삭제된 이후 해당 영역(비할당 영역)이 다른 파일(사진)로 덮어씌워진 것이다.

<center><p>
<img src="/assets/2019-10-28-post-mobile-android-autopsy/unallocated.jpg" width="100%">
<em>unallocated space</em>
</p></center>

> 참고

[Using Autopsy to examine an Android image - A solid, open source tool](https://freeandroidforensics.blogspot.com/2014/11/using-autopsy-to-examine-android-image.html)
