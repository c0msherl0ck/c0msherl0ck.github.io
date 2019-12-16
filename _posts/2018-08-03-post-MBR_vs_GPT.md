---
title: "MBR vs GPT"
categories:
  - File System
tags:
  - MBR
  - GPT
comments: true
---

이번 글에서는 MBR 파티션 방식과 GPT 파티션 방식의 차이점을 비교 분석한다.

> 1. MBR의 한계점
> 2. MBR vs GPT
> 3. Quiz

# 1. MBR의 한계점

`MBR`은 파티션 크기를 `섹터 단위`로 표현하는데 `4bytes`를 사용한다.

```
0x00 0x00 0x00 0x00 ~ 0xFF 0xFF 0xFF 0xFF : 2^32 섹터 표현 가능
2^32 Sectors = 2^32 * (512 Bytes) = 2^41 Bytes = 2 TB
```

<div class="notice">
즉, MBR 방식은 파티션 하나 당 최대용량은 2TB로 제한되며,<br>
해당 문제를 개선하기 위하여 나온 파티션 방식이 바로 GPT 방식이다.
</div>

# 2. MBR vs GPT

> 기능상 차이

||MBR|GPT|
|:---:|:---:|:---:|
|파티션 테이블 장소|MBR|GPT Header(or Backup GPT Header|
|파티션 최대 용량|2TB|8ZB(실제로는 무한대)|
|최대 파티션 수|15개 (SCSI 디스크의 경우, EBR 사용 시)|128개|
|파티션 라벨|파티션 ID|GUID|
|파티션 작성 툴|fdisk|parted|

[하드 디스크의 이해](http://webdir.tistory.com/160)

> 파티션 할당 방식 차이

[MBR과 EBR 분석](https://c0msherl0ck.github.io/file%20system/post-MBR/)

<center><p><img src="/assets/2018-08-03-post-MBR_vs_GPT/MBR.png"></p></center>

[GPT 분석](https://c0msherl0ck.github.io/file%20system/post-GPT/)

<center><p><img src="/assets/2018-08-03-post-MBR_vs_GPT/GPT.png"></p></center>

<br>

# 3. Quiz

PDF 자료 [MBR, GPT, Filesystem.pdf](https://github.com/proneer/Slides/tree/master/Filesystem) 내 Quiz 에 대한 정답은 아래와 같다.

> MBR 과 GPT

- 윈도우 7에서 MBR Slack 의 크기는?
  - 윈도우 XP : 63섹터
  - 윈도우 Vista/7/8 : 2048 섹터
- MBR 부트 코드의 크기와 역할
  - Master Boot Record(512 byte) = Boot Code(446 byte) + Partition Table Entry(64) + MBR signature(2 byte)
- MBR 파티션 테이블에 저장할 수 있는 주 파티션 개수는?
  - 4개
- MBR signature?
  - 0x55AA
- VBR 의 크기는?
  - 512 byte, 1 sector 
- VBR 의 역할은?
  - 해당 볼륨의 파일시스템에 대한 정보를 가지고 있으며, 부트로더를 통해 해당 볼륨의 운영체제를 부팅시킨다.
- NTFS 파티션 타입 코드 값은?
  - 0x07(NTFS), 0x0B(FAT32), 0xEE(GPT)
- GPT 를 사용하는 이유?
  - 파티션 최대 개수 및 용량을 늘리기 위해
- GPT 에서 생성 가능한 주 파티션 개수는?
  - 128개

> File System

- 파일 시스템을 메타 영역과 데이터 영역으로 구분하는 이유?
  - 파일에 접근할 때, 메타 정보(메타 영역에 파일에 대한 메타정보들이 있다.)를 기준으로 접근하여 실린더 헤드가 불필요하게 움직이는 거리를 최소화 시키기 위하여.
- 1섹터의 크기는?
  - 512 byte
- LBA 주소 지정방식을 사용하는 이유? CHS 주소 지정방식으로는 용량을 표현하는데 한계가 있다.(504 MB)
- 파일시스템에서 클러스터나 블럭단위를 사용하는 이유는?
  - 데이터 관리와 CPU 성능 효율을 위해서이다. 클러스터나 블록의 크기가 작을수록 실린더 헤드가 많이 움직여 효율성이 떨어지며, 클수록 낭비되는 공간(슬랙)이 많아진다.
- 클러스터 4K, 파일크기 2K 일 때 램 슬랙과 드라이브 슬랙의 크기는?
  - 램슬랙 : 파일크기가 섹터단위로 나누어 떨어지지 않을때 발생, 그러므로 2K는 4섹터에 해당하므로 0 byte
  - 드라이브 슬랙(파일 슬랙) : 파일크키가 클러스터 단위로 나누어 떨어지지 않을 때 발생, 그러므로 2KB
- 램슬랙의 특징은?
  - 다 0 으로 채워져 있다.
- 드라이브 슬랙의 특징은?
  - 이전의 정보가 덮어씌워지지 않은 상태로 파일복구가 가능한 영역이다.
- 파티션과 볼륨의 차이는?
  - 파티션 : 물리적으로 연속된 섹터들의 모음
  - 볼륨 : 논리적으로 연속된 섹터들의 모음
- 윈도우가 기본 인식(마운트)할 수 있는 파일시스템은?
  - FAT(12/16/32), exFAT, NTFS

<br>

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
