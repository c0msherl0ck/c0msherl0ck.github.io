---
title: "파티션 정보"
categories:
  - Data Leakage Case
tags:
  - MBR
  - Partition
comments: true
---

#2. Identify the partition information of PC image.

파티션 정보는 MBR에 저장되어 있다. 만약, MBR에 저장이 안되어있다면, 이것은 GPT 방식으로 GUID Partition Tables를 확인해야 한다.
MBR에서 파티션 정보를 확인하는 방법은 [MBR과 EBR 분석](https://c0msherl0ck.github.io/file%20system/post-MBR/)를 참고한다.

<center><p><img src="/assets/2018-08-10-post-data_leakage_case_2/1.jpg"></p></center>

|partition#1|
|---|
|Boot flag : 80 / File system type : 07 / LBA : 00 00 08 00 / SIZE : 00 03 20 00|
|부트 파티션 / NTFS / 시작위치 : 0x8000 sector(=0x1000000 bytes) / 크기 : 0x32000 sector(=0x6400000 bytes = 104857600 bytes = 100MB)|

|partition#2|
|---|
|Boot flag : 00 / File system type : 07 / LBA : 00 03 28 00 / SIZE : 02 7C D0 00|
|부트 파티션 아님 / NTFS / 시작위치 : 0x32800 sector(=0x6500000 bytes) / 크기 : 0x27CD000 sector (=0x4F9A00000 bytes = 21367881728 bytes = 20378MB)|

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
