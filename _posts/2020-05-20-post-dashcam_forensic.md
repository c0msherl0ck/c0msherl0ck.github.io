---
title: 차량 블랙박스 포렌식(비영상정보)
categories:
  - Video
tags:
  - dashcam
  - 블랙박스
  - 아이나비
comments: true
---

차량용 블랙박스 영상에는 영상정보와 함께 녹화 시각, 충격량, 속도, GPS 등의 비영상정보가 함께 저장된다.
이러한 비영상정보는 범죄 발생 시각, 위치 등 혐의를 입증하는데 있어 중요한 정보로 사용될 수 있다.
이번 글에서는 010 Editor, MP4BOX를 통해 아이나비 블랙박스의 비영상정보를 확인하고 전용뷰어와 비교한다.

> 1. 분석 대상
> 2. 010 Editor
> 3. MP4BOX
> 4. 전용뷰어
> 5. 결론
> 6. 참고

# 1. 분석 대상

분석 대상인 블랙박스 상세 스펙은 다음과 같다.

- 제조사 : 아이나비
- 모델명 : V900
- 채널개수 : 전/후방 2채널
- 파일시스템 : FAT32
- 저장방식 : 메모리에 30초 ~ 1분의 영상을 보관한 후 AVI, MP4와 같은 영상 컨테이너를 이용하여 파일 단위로 저장
- 플레이어 : 별도의 플레이어 필요하지 않으나, 전용뷰어 존재

# 2. 010 Editor

MP4 파일은 동영상 파일 확장자로 010 Editor에서 MP4 Template 적용을 통해 구조를 파악할 수 있다.
시각 정보는 Movie Header Box와 각 트랙의 Track Header Box에서 확인가능하다.

<center><p><img src="/assets/2020-05-20-post-dashcam_forensic/1. MP4 구조.jpg">
<br><em>MP4 구조</em></p></center>

<center><p><img src="/assets/2020-05-20-post-dashcam_forensic/2. Movie_Header_Box.jpg">
<br><em>Movie_Header_Box</em></p></center>

<center><p><img src="/assets/2020-05-20-post-dashcam_forensic/3. Track_Header_Box.jpg">
<br><em>Track_Header_Box</em></p></center>

# 3. MP4BOX

GPAC Installers를 통해 MP4BOX를 설치할 수 있다. 다운로드 경로 <https://gpac.wp.imt.fr/downloads/>

<center><p><img src="/assets/2020-05-20-post-dashcam_forensic/9. MP4BOX 다운로드.jpg">
<br><em>MP4BOX 다운로드</em></p></center>

설치 후 다음의 명령어를 통해 MP4 영상의 정보를 확인한다.

```
mp4box -info [파일명]
```

<center><p><img src="/assets/2020-05-20-post-dashcam_forensic/4. MP4B0X 정보확인.jpg">
<br><em>MP4B0X 정보확인</em></p></center>

확인결과 총 3개의 트랙으로 이루어져 있으며, 첫번째 트랙은 영상(video), 두번째 트랙은 소리(sound), 세번째 트랙은 자막(text)에 해당하는 것을 알 수 있다.
블랙박스의 경우 자막트랙에 비영상정보를 저장하는 경우가 많으므로 상세확인을 위해 다음과 같이 추출한다.

```
mp4box -raw [자막트랙 번호] [파일명]
```

<center><p><img src="/assets/2020-05-20-post-dashcam_forensic/5. MP4B0X 자막추출.jpg"><br>
<br><em>MP4B0X 자막추출</em></p></center>

추출한 자막(text)파일을 notepad++ 로 확인한 결과 "gsensor" 문자열과 관련 데이터를 확인할 수 있었다.

<div class="notice">
gsensor란?
<br><br>
아이나비가 세계 최초로 적용한 G센서기술이란 자북(북극) 방향의 절대방향과 자차의 진행방향을 감지할 수 있는 3축 지자기 센서(Geomagnetic-Sensor)와 X, Y, Z 벡터 값을 이용한 차의 움직임 감지가 가능한 3축 가속도 센서(Gravity-Sensor)를 아이나비 맵매칭(Map Matching)엔진과 결합했다. 이에 따라 진행방향 인지(認知)와 좌우회전 인지, 오르막길 인지, 내리막길 인지, 주행인지와 정차인지를 가장 빠르고 정확하게 알 수 있다.
<br><br>
[출처] http://www.inavi.com/CustCenter/Notice/Dtls/741?notidiv
</div>

<center><p><img src="/assets/2020-05-20-post-dashcam_forensic/6. MP4BOX gsensor.jpg"><br>
<br><em>MP4BOX gsensor</em></p></center>


# 4. 전용뷰어

아이나비에서 제공하는 전용뷰어를 통해 영상을 확인할 경우 비영상정보를 함께 확인할 수 있다.
분석대상 블랙박스는 구형모델로 기록되는 비영상정보가 제한적이었으나, 신형모델의 경우 보다 많은 비영상정보를 확인할 것으로 보인다.

전용뷰어가 아닌 일반플레이어를 통해 영상을 확인할 경우 정상적인 비영상 정보 확인이 어렵다.

<center><p><img src="/assets/2020-05-20-post-dashcam_forensic/7. 전용뷰어.jpg">
<br><em>전용뷰어</em></p></center>

<center><p><img src="/assets/2020-05-20-post-dashcam_forensic/8. 곰플레이어.jpg">
<br><em>곰플레이어</em></p></center>


# 5. 결론

블랙박스 제조사, 모델 별로 영상 내 비영상정보를 저장하는 구조와 방식에 차이가 있을 것으로 보인다.
이를 확인하는 가장 효율적인 방식은 해당 제조사에서 제공하는 전용뷰어를 이용하는 것이다.

시각정보의 경우 초기 블랙박스 세팅 시 설정한 시간대로 기록이 되기 때문에 실제 시각 정보와 일치하지 않을 수도 있음에 유의해야하며,
별도의 분석도구 없이도 영상 좌측 하단에서 시각정보를 확인할 수 있다.


# 6. 참고

안휘항, "차량용 블랙박스 포렌식을 위한 분석 절차 및 저장 구조 분석", 정보보호학회논문지, 2015.12

"MP4 및 MKV 영화에서 자막을 추출하는 방법", <https://qastack.kr/superuser/393762/how-to-extract-subtitles-from-mp4-and-mkv-movies>

"MP4-분석-하기-MPEG4-파트-14", <https://videocube.tistory.com/entry/MP4-%EB%B6%84%EC%84%9D-%ED%95%98%EA%B8%B0-MPEG4-%ED%8C%8C%ED%8A%B8-14>

<https://github.com/gpac/gpac/wiki/MP4Box>

<https://gpac.wp.imt.fr/downloads/>


{% if page.comments %}

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
/*
var disqus_config = function () {
this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
*/
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://https-c0msherl0ck-github-io.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
                            
{% endif %}
