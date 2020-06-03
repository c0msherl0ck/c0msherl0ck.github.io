---
title: "[아이폰 포렌식] 썸네일을 이용한 몰카, 아동음란물 소지 범죄의 입증방안 제1편"
categories:
  - Mobile Forensic
tags:
  - 썸네일
  - 몰카
  - 증거인멸
  - Thumbnail
  - iThmb Converter
  - iThmb Viewer
comments: true
---

몰카, 아동음란물 소지 등의 범죄에 있어서 범죄자가 증거인멸을 위해 범죄와 관련된 사진, 영상 등을 삭제하더라도,
핸드폰 내 자동적으로 생성된 썸네일이라는 파일을 증거로 범죄를 소명할 수 있다.

> 1. 썸네일이란?
> 2. 썸네일의 포렌식적 의미
> 3. 아이폰 썸네일 확인
> 4. 안드로이드 썸네일 확인
> 5. 참고

# 1. 썸네일이란?

썸네일은 이미지의 축소판이다. 썸네일은 사진 혹은 동영상 파일에서 일부 장면을 원본보다 작은 크기의 또 다른 그래픽 파일로 만들어 
원본 파일을 직접 확인하지 않더라도 사용자가 빠른 속도로 인식할 수 있도록 도와준다. 
다음과 같이 이미지 파일을 직접 클릭하지 않더라도 윈도우 탐색기 창에서 해당 이미지를 작은 사이즈로 확인 가능하다.

<center><p><img src="/assets/2020-05-27-post-mobile_thumbnail/1.jpg"><br><em>윈도우 썸네일 예시</em></p></center>

# 2. 썸네일의 포렌식적 의미

썸네일은 시스템이 자동으로 생성하며, 원본 파일이 삭제 되더라도 삭제되지 않는다.
{: .notice}

혐의자가 증거인멸을 위해 범죄와 관련된 이미지, 동영상을 삭제한다고 하더라도 그러한 삭제행위를 하기 위해서는 사진, 갤러리 어플리케이션 등을 실행해야 하고,
사진, 갤러리 어플리케이션이 실행되면 썸네일이 자동적으로 생성된다. 카메라 어플리케이션을 실행하여 사진을 찍은것만으로는 썸네일이 생성되지 않을 수 있다.

만약, 혐의자가 촬영 직후 어플리케이션을 통해 원본 삭제를 진행하지 않고 파일 시스템에 접근하여 명령어를 통해 원본을 삭제할 경우, 
사진, 갤러리 어플리케이션이 실행되지 않았기 때문에 썸네일이 남지 않을 수 있으나 이는 현실적으로 어려운 방법이다.

# 3. 아이폰 썸네일 확인

아이폰 썸네일의 경로는 다음과 같으며, 필자는 이전글 [iTunes를 이용한 백업](https://c0msherl0ck.github.io/mobile%20forensic/post-mobile_iphone8/) 파일에서 iBackupBot을 이용해 추출하였다.

```
/var/mobile/media/PhotoData/Thumbnails/[.ithmb]
```

<center><p><img src="/assets/2020-05-27-post-mobile_thumbnail/iBackupBot.jpg"><br><em>iBackupBot을 이용한 썸네일 추출</em></p></center>

<p>
추출한 ithumb 파일은 아카이브 형태이며, 썸네일 크기 별로 ithumb 파일이 다르게 구성되어 있다.
필자의 경우 총 3가지 크기로 구분되어 있으며 각각의 아카이브 내 썸네일 사진들을 확인하기 위해서는 iThmb Converter, iThmb Viewer 등의 별도의 도구가 필요하다. 두 도구다 유료이며 무료버전에서는 일부만을 지원하고, 전체 썸네일을 확인하기 위해서는 별도의 추가 비용을 지불하여야 한다.
</p>

iThmb Converter, <http://www.ithmbconverter.com/>

CompuClever ITHMB Viewer, <http://https://www.microsoft.com/ko-kr/store/p/compuclever-ithmb-viewer/9wzdncrdm8hg>

<center><p><img src="/assets/2020-05-27-post-mobile_thumbnail/iThmb_Converter.jpg"><br><em>iThmb_Converter</em></p></center>

<center><p><img src="/assets/2020-05-27-post-mobile_thumbnail/iThmb_Viewer.jpg"><br><em>iThmb_Viewer</em></p></center>

# 4. 안드로이드 썸네일 확인

안드로이드 편은 이어서 다음 글에서 다루도록 하겠다.

[안드로이드 포렌식 썸네일을 이용한 몰카, 아동음란물 소지 범죄의 입증방안 제2편](https://c0msherl0ck.github.io/mobile%20forensic/post_mobile_thumbnail_android/)

# 5. 참고

윤대호, "안드로이드 스마트폰 썸네일의 디지털 포렌식 조사 기법 연구", 2017.12, 고려대학교 석사학위 논문


