---
title: "[아이폰 포렌식] 아이폰 메신저 앱(문자, 카카오톡, 페이스북) 분석"
categories:
  - Mobile Forensic
tags:
  - mobile
  - forensic
  - ios
  - iphone
  - iBackupBot
  - SMS
  - KakaoTalk
  - Facebook Messenger
comments: true
---

모바일 포렌식에서 메신저 내 채팅 정보는 중요한 증거로, sqlite db 형태로 저장되어 있는 경우가 많다. 이번 글에서는 메신저 앱(문자, 카카오톡, 페이스북) 별 분석 방법을 소개한다.

> 1. iBackupBot 을 이용한 파일 추출
> 2. 문자 DB 분석(sms.db)
> 3. 카카오톡 DB 분석
> 4. 페이스북 메신저 분석
> 5. 참고

# 1. iBackupBot 을 이용한 파일 추출

iTunes를 이용해 생성한 아이폰 백업 파일에서 iBackupBot 도구를 이용하여 메신저 종류별로 다음의 경로에서 sqlite db 파일을 추출한다. 이전 글 [[모바일 포렌식] 아이폰 이미지 획득 및 분석](https://c0msherl0ck.github.io/mobile%20forensic/post-mobile_iphone8/)을 참고

|메신저 앱|iBackupBot 상 경로|
|---|---|
|문자|System Files/HomeDomain/SMS/sms.db|
|카카오톡|User App Files/com.iwilab.KakaoTalk/Libray/PrivateDocuments/Message.sqlite|
|페이스북 메신저|iTunes 백업을 통해 논리적 이미지를 획득하였을 경우 확인 불가(JailBreaking 이후 물리적 이미지 획득하여야함)|

<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone_messenger/SMS.jpg" width="100%">
<em>문자-sms.db</em>
</p></center>

<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone_messenger/KakaoTalk.jpg" width="100%">
<em>카카오톡-Message.sqlite</em>
</p></center>

<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone_messenger/facebook.jpg" width="100%">
<em>페이스북 메신저-확인되지 않음</em>
</p></center>

# 2. 문자 DB 분석(sms.db)

`sms.db` 내에는 여러 테이블이 존재하며, 이 중 눈여겨 보아야 하는 테이블은 `chat` 테이블과, `message` 테이블이다. `chat` 테이블에는 문자를 수발신한 전화번호 정보가 저장되며, `message` 테이블에는 실제 문자 내용이 저장된다. 이 두 테이블의 외래키 정보를 이용하여 수발신한 전화번호에 대응되는 문자 메세지 내용을 복구할 수 있다.(iBackupBot 기본 제공 기능)

<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone_messenger/sms.chat.jpg" width="100%">
<em>sms db 내 chat 테이블</em>
</p></center>

<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone_messenger/sms.message.jpg" width="100%">
<em>sms db 내 message 테이블</em>
</p></center>

<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone_messenger/example.jpg" width="100%">
<em>iBackupBot 기본 제공 기능</em>
</p></center>

message 테이블에서 시간 속성인 date_read는 `UNIX Time` 으로 기록되는데, 1970년 기준이 아닌 **2001년**을 기준으로 한다. 즉, UNIX Time 변환 값에 **31년**을 더해주어야 정확한 시간 값을 얻을 수 있다. ([아이폰 시간 속성](https://developer.apple.com/documentation/foundation/date) 참고)

<div class="notice">
예를 들어, date_read 의 값이 540212434일 경우 1987년 2월 13일 11:00:34(UTC+0) 이 아닌 2018년 2월 13일 11:00:34(UTC+0)가 되며, 이를 통해 필자가 2018년 2월 13일 20:00:34(UTC+9)에 신규가입 문자를 읽은 것을 알 수 있다.
</div>

<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone_messenger/cyberchef.jpg" width="100%">
<em>UNIX time 계산</em>
</p></center>

<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone_messenger/timeInterval.jpg" width="100%">
<em>2001년을 기준으로 하는 timeInterval</em>
</p></center>

# 3. 카카오톡 DB 분석

카카오톡의 대화내용은 Message 테이블의 message 속성에 저장된다. 대화 내용을 기본적으로 암호화며, 암호화된 내용을 Base64 인코딩하여 저장한다. 대화 내용을 복구하기 위해서는 별도의 복호화 과정이 필요하며 해당 내용은 많은 블로그에서 역공학의 방법을 통해 소개하고 있으므로 생략한다. 

<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone_messenger/kakao.message.jpg" width="100%">
<em>카카오톡 Message 테이블</em>
</p></center>

# 4. 페이스북 메신저 분석

페이스북 메신저를 비롯하여, 텔레그램 등의 메신저 앱들은 iTunes Backup 시 대화 내용을 백업하지 않도록 설정되어 있다.(*안티포렌식*) 해당 앱들의 대화 내용 데이터를 얻기 위해서는 JailBreak(관리자 권한 획득) 이후, **물리적 이미지**를 획득해야 한다.

물리적 이미지 획득 시 다음의 경로 확인 필요

    /private/var/mobile/Containers/Shared/AppGroup/<UUID>/_store_<ID>/messenger_messages_v1/orca2.db


# 5. 참고

[[모바일 포렌식] 아이폰 분석](https://c0msherl0ck.github.io/mobile%20forensic/post-mobile_iphone8/)

[IOS Forensics](https://resources.infosecinstitute.com/ios-forensics/#gref)
