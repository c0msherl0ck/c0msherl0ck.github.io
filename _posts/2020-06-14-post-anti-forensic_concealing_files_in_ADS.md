---
title: "[안티 포렌식] 파일 은닉 방법에 관한 연구 제3편 (ADS 내 은닉)"
categories:
  - anti-forensic
tags:
  - Steganography
  - ADS
comments: true
---

파일을 은닉하는 방법에는 이미지 내 파일 은닉, 문서 파일 내 은닉, ADS 내 은닉하는 방법 등이 있다. 
이번 글에서는 NTFS 파일 시스템에서 ADS 내 데이터를 은닉하고 탐지하는 방법에 대해 소개한다.

> 1. ADS 란?
> 2. 파일 은닉 및 확인
> 3. 참고

# 1. ADS 란?

Alternative Data Stream 의 약자로 NTFS 파일시스템에서 맥킨토시에서 사용하는 파일시스템과 호환성 유지를 위해 제공하는 Data Stream 영역이다. 
MFT Entry 의 속성을 살펴보면 **$Data가 1개가 아닌 2개 이상인 경우**가 존재하며, ADS에 저장되는 데이터로는 대표적으로 Zone Identifer가 있다.

한편, NTFS 파일 시스템에서는 File Data를 저장할 때 $Data 속성을 이용하며, 파일 크기에 따라 실제 파일 데이터가 저장되는 위치에는 차이가 있다.

|파일 크기|데이터 저장 위치|
|---|---|
|2KB 미만의 작은 파일|resident 속성으로 MFT Entry 내 $Data 속성에 저장|
|2KB 초과의 파일|non-resident 속성으로 MFT Entry 내 $Data 속성에는 File Data가 위치하는 Cluster 정보만 기재하고, 해당 클러스터에 File Data를 저장|


# 2. 파일 은닉 및 확인

ADS 영역은 다음의 DOS 명령어를 통해 그 존재유무를 확인할 수 있다.

```
dir /r
```

<center><p><img src="/assets/2020-06-10-post-anti-forensic_concealing_files_in_ADS/ADS 존재유무 확인.jpg">
<br><em>ADS 존재유무 확인</em></p></center>

위에서 확인한 ADS 이름을 매개로 그 내용을 확인하는 방법은 다음과 같다.

```
more < [파일명]:[ADS 이름]
```

<center><p><img src="/assets/2020-06-10-post-anti-forensic_concealing_files_in_ADS/ADS 내용 확인.jpg">
<br><em>ADS 내용 확인</em></p></center>

ADS 내 파일 및 문자열을 은닉하는 방법은 다음과 같다.

```
type [은닉할 파일] > [원본 파일]:[ADS 이름 지정] // 파일 은닉
echo "은닉할 문자열" >> [원본 파일]:[ADS 이름 지정] // 문자열 은닉
```

<center><p><img src="/assets/2020-06-10-post-anti-forensic_concealing_files_in_ADS/ADS 파일 은닉.jpg">
<br><em>ADS 파일 은닉</em></p></center>


# 3. 참고

[NTFS - 속성($STANDARD_INFORMATION, $FILE_NAME, $DATA) 및 ADS(Alternate Data Stream)](http://blog.naver.com/PostView.nhn?blogId=sjhmc9695&logNo=221409012223&categoryNo=37&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=search)
