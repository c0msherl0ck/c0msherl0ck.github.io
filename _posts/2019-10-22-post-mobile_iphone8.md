---
title: "[아이폰 포렌식] iTunes 백업을 이용한 논리적 이미지 획득"
categories:
  - 모바일 포렌식
tags:
  - mobile
  - forensic
  - ios
  - iphone
  - iBackupBot
comments: true
---

아이폰의 경우 iTunes 백업을 이용한 **논리적 이미지** 획득 방법과, JailBreak 이후 Cydia의 OpenSSH를 이용한 **물리적 이미지** 획득 방법이 있다. 이번 글에서는 iTunes 백업을 이용한 **논리적 이미지** 획득 방법과 iBackupBot를 이용해 백업된 파일을 분석하는 방안에 대해 서술한다.

> 1. 백업 파일 생성
> 2. 백업 파일 분석
> 3. iBackupBot 을 이용한 분석
> 4. 참고

# 1. 백업 파일 생성

아이폰의 경우 **iTunes**를 이용하여 백업을 수행한다. 단, `로컬 백업 암호화 체크를 해제`하여야 정상적인 분석이 가능하다.

<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone8/itunes-backup.jpg" width="100%">
<em>iTunes 백업 화면</em>
</p></center>

iTunes를 이용한 백업 파일은 다음의 경로에 저장이 된다.

|운영체제   |백업경로   |
|---|---|
|Windows XP   |C:\Documents and Settings\[user name]\Application Data\Apple Computer\MobileSync\Backup\ |
|Windows 7  |C:\Users\[user name]\AppData\Roaming\AppleComputer\MobileSync\Backup\ |
|Windows 10   |C:\Users\holywater\AppData\Roaming\Apple Computer\MobileSync\Backup\ |
|MAC OS X   |~/Library/Application Support/MobileSync/Backup/ |

<br>
필자의 경우 Windows 10 에서 다음과 같이 저장되어 있는 것을 확인하였다. 폴더명은 제품에 따라 구분되며, 동일 제품을 백업할 경우 새로운 폴더가 생기는 것이 아닌 기존의 폴더가 업데이트된다. 만약, 다른 사람의 아이폰, 아이패드를 백업한다면 제품별 각각 폴더가 생긴다.

<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone8/backup-directory.jpg" width="100%">
<em>iTunes 백업 파일 경로</em>
</p></center>

<br>

# 2. 백업 파일 분석

 백업 폴더안에는 다음과 같은 파일과 폴더가 존재한다.

<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone8/backup-FilesAndFolders-1.jpg" width="400px"><br>
<img src="/assets/2019-10-22-post-mobile_iphone8/backup-FilesAndFolders-2.jpg" width="400px"><br>
<em>iTunes 백업 파일</em>
</p></center>

|파일명|역할|
|---|---|
|Manifest.db|백업 폴더 안에 저장되어 있는 파일명을 기록하는 sqlite DB 파일|
|Info.plist|Build Version, Device Name, GuID, ICCID, Last Backup Date 등을 기록하는 파일|
|Manifest.plist|Applications, Date, Encrypt 유무, System Domain Version 등을 기록하는 파일|
|Status.plist|BackupState, UUID, Version 등을 기록하는 파일|
|00 ~ ff 폴더|실제 핸드폰 내 파일들을 백업한 파일들이 존재|

<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone8/Manifest.jpg" width="100%">
<em>Manifest.db</em>
</p></center>

<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone8/plist.jpg" width="100%">
<em>status.plist</em>
</p></center>

Manifest.db 의 Files 테이블에는 fileID(기본키), domain, relativePath 속성이 있으며, domain 속성과 relativePath 속성의 값을 통해 fileID를 생성한다.

<div class="notice">
fileID = SHA1 (domain + "-" + relativePath)
</div>

<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone8/fileID_1.jpg" width="100%"><br>
<img src="/assets/2019-10-22-post-mobile_iphone8/fileID_2.jpg" width="500px"><br>
<em>fileID 생성 원리</em>
</p></center>

00 ~ ff 폴더 내 파일들의 파일명은 상기의 fileID 와 같으며, 각각의 파일이 어떤 파일인지는 mapping 되는 domain, relativePath 속성 값을 통해 알 수 있다.

예를 들어, d3 폴더 내의 d3c949392557a4a7d75233de0c7f1bb5e9322aa5 이라는 파일이 어떤 파일인지 알기 위해서는, fileID 에 해당하는 튜플(노란색 행)을 찾는다. 해당 튜플의 domain 속성의 값이 CameraRollDomain 것을 통해 카메라 앱과 관련된 파일이라는 것과, relativePath 를 통해 JPG 확장자를 가진 사진 파일이라는 것을 알 수 있다.

<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone8/example_1.jpg" width="400px"><br>
<em>d3 폴더</em>
</p></center>
<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone8/example_2.jpg" width="500px"><br>
<em>d3c949392557a4a7d75233de0c7f1bb5e9322aa5 파일</em>
</p></center>
<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone8/example_3.jpg" width="100%"><br>
<em>Manifest.db 내 Files 테이블에서 fileID 가 d3c949392557a4a7d75233de0c7f1bb5e9322aa5 인 튜플</em>
</p></center>
<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone8/example_4.jpg" width="100%"><br>
<em>JPG 확장자로 변경 후 파일 확인</em>
</p></center>

# 3. iBackupBot 을 이용한 분석

앞서 기술한 원리를 통해 백업 파일들을 맵핑해주는 도구가 있다. [iBackupBot](https://www.icopybot.com/itunes-backup-manager.htm) 이라는 도구이며, Freeware 이다. 해당 도구를 통해 분석에 필요한 파일들을 편리하게 추출할 수 있다.

<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone8/tools_1.jpg" width="100%"><br>
<em>iBackupBot 로딩(약 1분 정도의 시간이 소요)</em>
</p></center>

<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone8/tools_2.jpg" width="100%"><br>
<em>iBackupBot 에서 확인한 백업 파일들</em>
</p></center>

# 4. 참고

[[Tech Report] 아이폰 백업 파일의 흔적을 찾아라](https://www.ahnlab.com/kr/site/securityinfo/secunews/secuNewsView.do?menu_dist=2&seq=20118)

[[Tech Report] 앱을 읽으면 사용자의 라이프스타일이 보인다](http://v3.nonghyup.com/secu_info_view.asp?list=/secu_info_list.asp&seq=20245&pageno=100&v_num=1425)

[플래시 메모리 이미지 획득/IOS](http://forensic.korea.ac.kr/DFWIKI/index.php/%ED%94%8C%EB%9E%98%EC%8B%9C_%EB%A9%94%EB%AA%A8%EB%A6%AC_%EC%9D%B4%EB%AF%B8%EC%A7%80_%ED%9A%8D%EB%93%9D/IOS)

[Forensic Analysis on IOS Devices](https://www.sans.org/reading-room/whitepapers/forensics/paper/34092)
