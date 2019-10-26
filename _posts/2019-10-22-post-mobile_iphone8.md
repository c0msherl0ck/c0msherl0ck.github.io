---
title: "[모바일포렌식] 아이폰 분석"
categories:
  - mobile
tags:
  - mobile
  - ios
  - iphone
---

This theme supports **link posts**, made famous by John Gruber. To use, just add `link: http://url-you-want-linked` to the post's YAML front matter and you're done.

> 논리적 이미지 획득

아이폰의 경우 iTunes를 이용한 백업을 통해 논리적 이미지를 획득한다. 단, 로컬 백업 암호화 체크를 해제하여야 정상적인 분석이 가능하다.

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

필자의 경우 Windows 10 에서 다음과 같이 저장되어있다. 폴더명은 제품에 따라 구분되며, 동일 제품을 백업할 경우 새로운 폴더가 생기는 것이 아닌 기존의 폴더가 업데이트된다. 만약, 다른 사람의 아이폰, 아이패드를 백업한다면 제품별 각각 폴더가 생긴다.

<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone8/backup-directory.jpg" width="100%">
<em>iTunes 백업 파일 경로</em>
</p></center>

> 백업 파일 분석

 폴더안에는 다음과 같은 파일과 폴더가 존재한다.

<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone8/backup-FilesAndFolders-1.jpg" width="300px">
</p></center>

<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone8/backup-FilesAndFolders-2.jpg" width="300px">
<em>iTunes 백업 파일</em>
</p></center>

- Manifest.db: 백업 폴더 안에 저장되어 있는 파일명을 기록하는 sqlite DB 파일
- Info.plist: Build Version, Device Name, GuID, ICCID, Last Backup Date 등을 기록하는 파일
- Manifest.plist: Applications, Date, Encrypt 유무, System Domain Version 등을 기록하는 파일
- Status.plist: BackupState, UUID, Version 등을 기록하는 파일
- 00 ~ ff Folder : 실제 파일이 저장되어 있는 폴더




> 참고

[link](#)
