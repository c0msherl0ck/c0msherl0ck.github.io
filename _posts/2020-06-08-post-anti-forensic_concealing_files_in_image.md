---
title: "[안티 포렌식] 파일 은닉 방법에 관한 연구 제1편 (이미지 파일 내 은닉)"
categories:
  - anti-forensic
tags:
  - Steganography
  - JPG
  - PNG
  - Footer Signature
  - OpenStego
  - OpenPuff
comments: true
---

파일을 은닉하는 방법에는 이미지 내 파일 은닉, 문서 파일 내 은닉, ADS 내 은닉하는 방법 등이 있다.
이번 글에서는 이미지 내 은닉한 파일을 발견 및 복구하는 방법에 대해 알아본다.

> 1. 이미지 내 파일 은닉 방법
> 2. HxD를 통한 Footer Signature 확인
> 3. 010 Editor Template 적용을 통한 확인
> 4. 참고

# 1. 이미지 내 파일 은닉 방법

OpenStego, OpenPuff 등의 스테가노그래피 도구가 존재하며, DOS 명령어을 통해서도 파일을 은닉할 수 있다. 
DOS 명령어를 이용하는 방법은 다음과 같이 바이너리 형태로 은닉할 파일을 원본 파일 뒤에 이어붙이는 형식으로 진행된다.

```
copy /b [원본 파일명] + [은닉할 파일명] [은닉한 파일이 포함된 새로운 파일명]
```

<center><p><img src="/assets/2020-06-08-post-anti-forensic_concealing_files_in_image/DOS 명령어.jpg"><br><em>DOS 명령어</em></p></center>

한편, 은닉된 파일을 탐지하는 stegdetect 도구도 존재하며 다음 경로에서 다운로드 받을 수 있다.

- <https://github.com/abeluck/stegdetect>


# 2. HxD를 통한 Footer Signature 확인

은닉된 파일은 HxD 에서 File Signature 분석을 통해 알 수 있다.
통상적인 Signature 분석의 경우 파일의 앞 부분인 Header Signature에 대해서만 분석을 하는 경우가 많지만, 
다수의 이미지 파일의 경우 **Footer Signature** 를 가지고 있으므로 다음을 확인하면 파일의 은닉여부를 알 수 있다.

- Footer Signature와 Header Signature의 일치 여부
- Footer Signature가 파일의 마지막이 맞는지 여부

가령, 파일의 앞 부분을 보았을 때 PNG 파일의 Header Signature에 해당하나, 파일의 마지막 부분 Footer Signature를 확인하였을 때 
JPG Footer Signature에 해당한다면 PNG 파일 내 JPG 파일이 은닉되었는지 의심해볼 수 있다.

해당 문제 유형의 문제와 풀이는 다음을 참고한다.

- [xcz.kr] 1번 End Of Image 풀이, <https://h4nu.tistory.com/1>
- [UTCTF 2020 Forensics] Observe Closely, <https://hjh0703.tistory.com/4>

# 3. 010 Editor Template 적용을 통한 확인

앞서 소개한 HxD 방법이외에도 파일 구조 분석을 용이하게 해주는 010 Editor의 **Template** 적용을 통해 파일 은닉 관련 흔적을 손쉽게 발견할 수 있다. 
파일이 은닉된 JPG, PNG 파일 등을 010 Editor 에서 Template 을 적용하여 확인하면 다음과 같다.

<center><p><img src="/assets/2020-06-08-post-anti-forensic_concealing_files_in_image/010 Editor JPG.jpg">
<br><em>010 Editor JPG</em></p></center>

<center><p><img src="/assets/2020-06-08-post-anti-forensic_concealing_files_in_image/010 Editor PNG.jpg">
<br><em>010 Editor PNG</em></p></center>

두 파일 모두 공통적으로 Footer Signature 이후 **추가적인 데이터 부분(Unknown Padding 등)** 이 존재하는 것을 확인할 수 있다. 
해당 부분을 추출하여 별도의 파일로 생성후, 추출된 파일의 타입에 맞는 확장자를 설정해주면 숨겨진 은닉파일의 확인이 가능하다.


# 4. 참고

구글 검색어 : "이미지 footer 파일 은닉"

안티 포렌식 - Passion - 티스토리, <https://duddnr0615k.tistory.com/283>

Digital Forensic Challenge 2018 AF100 문제 풀이, <https://whitesnake1004.tistory.com/650>

Steganography. 스테가노그래피 툴 받기, <https://whackur.tistory.com/74>

데이터 은닉 기술(스테가노그래피) - 안전제일 - 티스토리, <https://jdh5202.tistory.com/99>

[UTCTF 2020 Forensics] Observe Closely, <https://hjh0703.tistory.com/4>

xcz.kr 1번문제 풀이 - K-Shield - 티스토리, <https://h4nu.tistory.com/1>

제2회 디지털 범인을 찾아라_이규호_풀이, <https://extr.tistory.com/178?category=484146>

