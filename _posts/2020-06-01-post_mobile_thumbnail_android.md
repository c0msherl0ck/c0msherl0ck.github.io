---
title: "[안드로이드 포렌식] 썸네일을 이용한 몰카, 아동음란물 소지 범죄의 입증방안 제2편"
categories:
  - Mobile Forensic
tags:
  - 썸네일
  - Autopsy
comments: true
---

이전 글에 이어 Autopsy 도구를 이용하여 안드로이드 핸드폰에서의 썸네일을 분석한다.

> 1. 들어가기 전에
> 2. 삭제된 이미지 파일 확인
> 3. 썸네일 확인
> 4. 참고

# 1. 들어가기 전에

이번글에서는 이전에 분석한 내용을 토대로 진행되었기 때문에 이전의 글들을 참고하여야 한다.

[안드로이드 포렌식 Autopsy 도구를 이용한 증거 분석](https://c0msherl0ck.github.io/mobile%20forensic/post-mobile-android-autopsy/)
에서 Autopsy의 Android analyzer 프로세싱을 진행한 이후 본 글에서의 분석을 진행할 수 있으며,

썸네일의 기본 개념에 대해서는 다음의 글을 참고한다. 
[아이폰 포렌식 썸네일을 이용한 몰카, 아동음란물 소지 범죄의 입증방안](https://c0msherl0ck.github.io/mobile%20forensic/post-mobile_thumbnail_iphone/)


# 2. 삭제된 이미지 파일 확인

Autopsy의 Views 메뉴에서는 삭제된 이미지를 포함하여 모든 이미지 파일을 확인할 수 있다. 만약 혐의자가 증거 인멸을 위해 사진을 삭제했다고 하더라도 해당 영역이 다른 데이터로 덮어씌워지지 않았다면 확인 및 복구가 가능하다.

<center><p><img src="/assets/2020-06-01-post_mobile_thumbnail_android/image.jpg"><br><em>삭제된 이미지 파일 확인</em></p></center>

Views 메뉴에서 보여주는 이미지 파일들의 저장위치를 알기 위해서는 원본 경로를 알아야 한다. 
이를 위해 모든 이미지 파일들에 대해 Location 컬럼에서 경로를 추출하여 엑셀에서 중복을 제거한 뒤 확인한 결과는 다음과 같다.

```
userdata/system/recent_images
userdata/media/0/PlayMemories Mobile
userdata/media/0/Pictures/Screenshots
userdata/media/0/NaverMap/navi/UserData/OBN/Route/.NIGHT
userdata/media/0/NaverMap/navi/UserData/OBN/Route/.DAY
userdata/media/0/KakaoTalkDownload
userdata/media/0/DCIM/.thumbnails
userdata/media/0/Android/data/com.kakao.talk/contents/91040664126874
userdata/data/com.sec.android.app.samsungapps
userdata/data/com.samsung.clipboardsaveservice/files
userdata/data/com.omnitel.android.dmb/files
userdata/data/com.hanabank.ebk.channel.android.hananbank/aria/temp
userdata/data/com.hanabank.ebk.channel.android.hananbank/aria
userdata/data/com.android.chrome/files/images
```


# 3. 썸네일 확인

이전글에서 알아본 바와 같이 혐의자가 증거인멸을 위해 사진을 삭제했다고 하더라도 썸네일은 삭제되지 않는다. 
앞서 알아본 모든 이미지 파일의 저장 경로 중 썸네일에 해당하는 경로는 다음과 같다.

```
userdata/media/0/DCIM/.thumbnails/
```

<center><p><img src="/assets/2020-06-01-post_mobile_thumbnail_android/thumbnail.jpg"><br><em>thumbnail</em></p></center>

해당 경로에는 `thumbdata3` 아카이브 파일과 `jpg` 형태의 개별 파일이 저장되어 있다.
`jpg` 형태의 썸네일은 `thumbdata3` 아카이브 파일 내의 썸네일보다 해상도가 더 큰 형태로 저장되는 것으로 썸네일의 내용에 차이가 있는 것은 아니다. 
따라서 아이폰과는 다르게 별도의 도구가 없더라도 썸네일 확인이 가능하다.

이외에도 참고한 논문(윤대호 저)에서는 다음의 경로에 있는 아카이브 형태의 썸캐시에 대해서 언급하였다.

|제조사|ThumbCache Path|
|---|---|
|삼성|userdata/data/com.sec.android.gallery3d/cache/|
|LG|userdata/data/com.android.gallery3d/cache/|

위 경로를 확인결과 25KB 크기의 com.android.opengl.shaders_cache 파일이 존재했으며 분석을 위해 추출하였다. 
추출한 파일을 <https://formats.kaitai.io/android_opengl_shaders_cache/python.html> 사이트에서 제공하는 파이썬 라이브러리를 통해 분석하고자 하였으나 썸캐시 파일이 온전하지 않은지 다음과 같은 파일크기 관련 에러가 발생하였다. 이 부분은 다른 안드로이드폰을 통해 추가적인 확인이 필요하다.

<div class="notice">
EOFError: requested 15335424 bytes, but got only 23906 bytes
</div>

<center><p><img src="/assets/2020-06-01-post_mobile_thumbnail_android/thumbcache.jpg"><br><em>thumbcache</em></p></center>


# 4. 참고

윤대호, "안드로이드 스마트폰 썸네일의 디지털 포렌식 조사 기법 연구", 2017.12
