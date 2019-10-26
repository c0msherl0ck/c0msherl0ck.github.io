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
|00 ~ ff 폴더|실제 사용자 파일이 저장되어 있는 폴더|

<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone8/Manifest.jpg" width="100%">
<em>Manifest.db</em>
</p></center>

<center><p>
<img src="/assets/2019-10-22-post-mobile_iphone8/plist.jpg" width="100%">
<em>status.plist</em>
</p></center>

Manifest.db 의 Files 테이블에는 fileID(기본키), domain, relativePath 속성이 있으며, domain 속성과 relativePath 속성의 값을 통해 fileID를 생성한다.

<pre><code>
fileID = SHA1 (domain + "-" + relativePath)
</code></pre>

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

> 참고

[link](#)
