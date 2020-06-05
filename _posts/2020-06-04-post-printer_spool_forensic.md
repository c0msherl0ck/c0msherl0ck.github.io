---
title: "[프린터 포렌식] 스풀(Spool) 파일 분석"
categories:
  - Embedded Forensic
tags:
  - SHD
  - SPL
comments: true
---

프린터 포렌식의 경우에는 크게 두 가지로 분류할 수 있다. 하나는 프린트 명령을 내린 PC를 분석하는 것이고, 
다른 하나는 프린트를 진행한 복합기 및 프린터기를 직접 분석하는 것이다. 이번 글에서는 프린트 명령을 내린 PC 를 분석하는 방법에 대해 소개한다.

> 1. 프린트 흔적 (Print Artifacts)
> 2. 스풀 파일 분석(Spool)
> 3. 참고


# 1. 프린트 흔적 (Print Artifacts)

PC에서 프린트를 진행할 경우, PC 내 **임시파일** 형태로 프린트와 관련된 데이터를 저장하고 이를 프린터기에 전송한다. 
데이터는 `.SHD`, `.SPL` 형태로 저장되며 이들 파일을 `스풀 파일`(spool)이라고 부르며, `스풀러 작업`(spooler)이란 이러한 파일을 만드는 작업을 뜻한다.

스풀 파일이 생성되는 위치는 다음과 같다.

- 스풀 파일 생성 위치 : C:\Windows\System32\spool\PRINTERS 

한편, 스풀러 방식에는 Raw 방식과 EMF 방식이 있다. 프린터속성에서 변경 가능하며 각각의 방식에 따라 스풀 파일 내 저장되는 정보가 다르다.

<center><p>
<img src="/assets/2020-06-04-post-printer_spool_forensic/프린터속성.jpg" width="25%">
<img src="/assets/2020-06-04-post-printer_spool_forensic/스풀방식.jpg" width="65%">
</p></center>

|파일 확장자|Raw|EMF|
|---|---|---|
|SHD|프린트 작업 정보|데이터 저장 되지 않음(0 KB)|
|SPL|프린트 작업 정보 + 인쇄물 데이터|프린트 작업 정보 + 인쇄물을 EMF로 변환한 데이터|


# 2. 스풀 파일 분석(SHD, SPL)

스풀파일은 인쇄가 종료되면 자동으로 삭제되기 때문에 분석 PC의 이미지를 획득 후 파일 복구를 통해 분석해야한다.
그러나, SSD를 사용하는 PC에서 C 볼룸에 위치하는 파일을 복구하는 것은 가능성이 높지 않다.

필자의 경우 PC to Printer USB 케이블을 제거한 상태로 프린트 명령을 내렸고, 
프린터로 전송이 되지 못한 스풀파일들이 남아있는 상태에서 파일 복사 후 분석하였다. 
USB 케이블을 재연결하면 해당 스풀파일들은 삭제된다.

<center><p><img src="/assets/2020-06-04-post-printer_spool_forensic/케이블 연결 해제.jpg" width="50%"><br><em>USB 케이블 제거 후 프린트 시 장애화면</em></p></center>

## 2.1 Raw 방식

Raw 방식의 경우 000XX.SHD, 000XX.SPL 파일이름으로 생성되었다.

<center><p><img src="/assets/2020-06-04-post-printer_spool_forensic/RAW 방식.jpg"><br><em>Raw 방식</em></p></center>

각각의 파일을 헥사에디터로 확인한 결과 사용한 프린터기, PC, 윈도우 계정, 프린트한 파일명 등의 작업 정보를 확인할 수 있었다.
그러나, 인쇄물에 해당하는 데이터의 경우 그 내용을 확인 및 유추하기 어렵다.

<center><p>
<img src="/assets/2020-06-04-post-printer_spool_forensic/SHD_1.jpg" width="45%">
<img src="/assets/2020-06-04-post-printer_spool_forensic/SHD_2.jpg" width="45%">
<br><em>SHD</em>
</p></center>

<center><p>
<img src="/assets/2020-06-04-post-printer_spool_forensic/SPL_1.jpg" width="45%">
<img src="/assets/2020-06-04-post-printer_spool_forensic/SPL_2.jpg" width="45%">
<br><em>SPL</em>
</p></center>


## 2.2 EMF 방식

EMF 방식의 경우 FP0000XX.SHD, FP000XX.SPL 파일이름으로 생성되었다. 
단, SHD 파일의 경우 파일이 생성되기는 하지만 0 KB로 데이터를 저장하지는 않는다.

<center><p><img src="/assets/2020-06-04-post-printer_spool_forensic/EMF 방식.jpg"><br><em>EMF 방식</em></p></center>

SPL 파일에는 인쇄물이 EMF 포맷으로 저장되는데, 내용 확인이 용이하며 많은 도구들이 이를 지원한다. 
대표적으로 SPLView64 도구가 있으며 다음의 경로에서 다운로드 받을 수 있다.

- http://www.lvbprint.de/html/download.html

별도의 도구 없이도 SPL Header를 제거하고 파일의 확장자를 EMF 로 변환하면 윈도우 기본 그림판으로도 확인이 가능하다.

<center><p><img src="/assets/2020-06-04-post-printer_spool_forensic/SPL_HEADER.jpg"><br><em>SPL_HEADER</em></p></center>

<center><p><img src="/assets/2020-06-04-post-printer_spool_forensic/EMF 파일로 변경 후 확인.jpg" width="60%"><br><em>EMF 확장자로 변경 후 확인</em></p></center>

# 3. 참고

<http://forensic-proof.com/archives/2922>

<http://www.undocprint.org/formats/winspool/spl>
