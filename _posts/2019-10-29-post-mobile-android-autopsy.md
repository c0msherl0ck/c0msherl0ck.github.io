---
title: "[안드로이드 포렌식] Autopsy 도구를 이용한 안드로이드 핸드폰 분석"
categories:
  - Mobile Forensic
tags:
  - mobile
  - forensic
  - android
  - samsung galaxy
  - autopsy
comments: true
---

Autopsy는 EnCase와 같은 통합 포렌식 분석 도구이며 무료이다. 흔히 무료라고 하면 기능상의 제약이 있을 것 같지만, 안드로이드 포렌식에 적합한 android analyzer 등 강력한 기능을 제공한다.

> 1. Autopsy를 이용한 프로세싱 (android analyzer)
> 2. 파일시스템 분석 및 데이터 추출 (Data Source)
> 3. 파일 확인 및  (Views)
> 4. 기본 앱 분석 (Results)
> 5. Timeline 분석을 통한 사용자 행위 추적
> 6. 참고

# 1. Autopsy 를 이용한 프로세싱

이전글 [안드로이드 핸드폰 증거 획득](https://c0msherl0ck.github.io/mobile%20forensic/post-mobile-android/)에서 획득한 이미지를 분석한다.

Autopsy 도구는 매우 직관적이고 간편하므로 별도의 설명없이 다음의 그림 순서에 따라 분석을 진행한다.

<center><p><img src="/assets/2019-10-29-post-mobile-android-autopsy/1.jpg"><br><em>Case Information</em></p></center>

<center><p><img src="/assets/2019-10-29-post-mobile-android-autopsy/2.jpg"><br><em>Optional Information</em></p></center>

<center><p><img src="/assets/2019-10-29-post-mobile-android-autopsy/3.jpg"><br><em>Select Type of Data Source</em></p></center>

<center><p><img src="/assets/2019-10-29-post-mobile-android-autopsy/4.jpg"><br><em>Select Data Source</em></p></center>

<center><p><img src="/assets/2019-10-29-post-mobile-android-autopsy/android_analyzer.jpg"><br><em>android analyzer 선택</em></p></center>

<center><p><img src="/assets/2019-10-29-post-mobile-android-autopsy/processing.jpg"><em>Processing 상태</em></p></center>

# 2. 파일시스템 분석 및 데이터 추출 (Data Source)

안드로이드 운영체제의 파일시스템은 ext4를 사용한다. Data source 메뉴에서 각 파티션을 확인할 수 있으며, 
각 파티션(볼륨) 중 사용자 관련 데이터가 저장된 부분은 `userdata` 파티션이다.
`userdata` 파티션 내에는 여러 폴더가 존재하며 그 중 유의깊게 봐야하는 폴더는 `app`, `data`, `media` 이다. 

|폴더명|역할|
|---|---|
|app|사용자가 다운로드한 어플리케이션의 설치파일(.apk)이 저장된다.|
|data|어플리케이션이 사용하는 데이터(sqlite db, xml 등)가 저장된다.|
|media|사진 및 영상 파일이 저장된다.|

<center><p><img src="/assets/2019-10-29-post-mobile-android-autopsy/파티션 확인.jpg"><br><em>파티션 확인</em></p></center>

<center><p><img src="/assets/2019-10-29-post-mobile-android-autopsy/폴더 확인.jpg"><br><em>폴더 확인</em></p></center>

각 폴더를 클릭하면 폴더 내 존재하는 파일을 확인할 수 있으며 우클릭을 통해 자료추출 또한 가능하다. 
다음과 같이 카카오톡 메세지 DB를 추출할 수 있다. 카카오톡 메세지와 같이 사용자가 직접 설치한 어플리케이션의 데이터들은 Autopsy에서 분석결과가 제공되지 않으므로 추출 후 추가적인 분석이 필요하다.

<center><p><img src="/assets/2019-10-29-post-mobile-android-autopsy/카카오톡 데이터 추출.jpg"><br><em>카카오톡 데이터 추출</em></p></center>


# 3. 파일 확인 및 파일 복구 (Views)

Views 메뉴에서는 파일 유형 별 조회가 가능하다.

|Tab|기능|
|---|---|
|By Extension|확장자별 분류|
|Documents|알려진 문서 속성|
|Executable|실행파일|
|By MIME Type|이메일 관련 파일|

Autopsy 에서는 파일카빙을 통해 비할당 영역에서의 파일복구를 지원한다. 
만약, 삭제된 파일영역이 다른 데이터로 덮어씌워지지 않았다면 복구가 가능하다.
파일이 덮어씌웠는지 여부는 해당 파일의 Size를 보고 알 수 있다. Size가 0 일 경우 이미 해당 영역은 다른 데이터에 의해 덮어씌워진 것으로 복구가 불가능하다.

<center><p><img src="/assets/2019-10-29-post-mobile-android-autopsy/파일 복구.jpg"><br><em>파일 복구</em></p></center>

# 4. 기본 앱 분석 (Results)

핸드폰을 초기화하였을 때 기본으로 설치되어 있는 안드로이드 기본 어플리케이션에 대해서는 Results 메뉴에서 분석결과를 제공한다. 
통화기록, 연락처, 문자, 웹 기록을 확인할 수 있으며, Keyword Search 또한 가능하다. 
프로세싱 옵션을 선택 시 Keyword를 설정하여 진행한다면 사건과 관련된 데이터만을 확인할 수 있다.

<center><p><img src="/assets/2019-10-29-post-mobile-android-autopsy/기본 앱 분석.jpg"><br><em>기본 앱 분석</em></p></center>

# 5. Timeline 분석을 통한 사용자 행위 추적

Autopsy 상단의 메뉴 중 Timeline을 클릭하면 시간대별 사용자 행위를 분석할 수 있다. 필자가 분석한 핸드폰은 지인으로부터 중고로 핸드폰을 구매하여 사용하던 것으로, 파일시스템 흔적(빨간색)의 경우 2014년보다 더 이전에도 존재하는 것을 확인할 수 있다. 반면, Web activity 등 사용자 관련 흔적들은(초록색, 파란색) 필자가 핸드폰을 사용한 2014년~2016년 기간만 존재하는 것을 확인하였다.

<center><p><img src="/assets/2019-10-29-post-mobile-android-autopsy/타임라인 분석.jpg"><br><em>타임라인 분석</em></p></center>

Details 메뉴를 클릭하면 각 흔적의 상세한 내역들을 확인할 수 있으며 이를 통해 시간대별로 핸드폰의 사용자가 어떠한 행위를 하였는지 유추할 수 있다.

<center><p><img src="/assets/2019-10-29-post-mobile-android-autopsy/timeline.jpg"><br><em>타임라인 Details</em></p></center>

# 6. 참고

[Using Autopsy to examine an Android image - A solid, open source tool](https://freeandroidforensics.blogspot.com/2014/11/using-autopsy-to-examine-android-image.html)
