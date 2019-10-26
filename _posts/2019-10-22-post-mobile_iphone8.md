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

필자의 경우 Windows 10 으로 다음과 같이 저장되어있다.

<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone8/backup-directory.jpg" width="100%">
<em>iTunes 백업 파일 경로</em>
</p></center>

해당 폴더안에는 다음과 같은 파일과 폴더가 존재한다.

<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone8/backup-FilesAndFolders-1.jpg" width="500px"><b>
<img src="/assets/2019-10-22-post-mobile_iphone8/backup-FilesAndFolders-2.jpg" width="500px"><b>
<em>iTunes 백업 파일</em>
</p></center>





> 참고

[link](#)
