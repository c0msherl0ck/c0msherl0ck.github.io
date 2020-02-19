---
title: "[E01] 이미징 해시값 검증"
categories:
  - Data Leakage Case
tags:
  - E01
  - DD
  - Hash
comments: true
---

CFReDS-Data Leakage Case #1. What are the hash values (MD5 & SHA-1) of all images? Does the acquisition and verification hash value match?

> 1. 이미지 및 해시 종류
> 2. 확인방법

# 1. 이미지 및 해시 종류

|Image Type|특징|
|---|---|
|DD|Raw Image Type, bit-by-bit, 실제 파일과 동일한 크기|
|E01|EnCase Image Type, EnCase 프로그램을 통해 압축파일 형식으로 이미징하며, 이미지 내에 Verification Hash등의 정보가 저장 가능함|

|EnCase|FTK Imager|용도|
|---|---|---|
|Acquisition Hash|Computed Hash|분석도구에 추가한 증거물에 대해 해시를 계산한 것|
|Verification Hash|Stored Verification hash|증거물 내에 저장되어 있던 해시값|

# 2. 확인방법

FTK Imager를 통해 증거물의 정보를 확인한다.

`2.1.` [File]-[Add Evidence Item]

<center><p><img src="/assets/2018-08-10-post-data_leakage_case_1/1.jpg"></p></center>

`2.2.` Image file 선택

<center><p><img src="/assets/2018-08-10-post-data_leakage_case_1/2.jpg"></p></center>

`2.3.` E01 만 로드하면 된다.(E01, E02, E03, E04 는 분할 압축 파일 개념과 유사하다.)

<center><p><img src="/assets/2018-08-10-post-data_leakage_case_1/3.jpg"></p></center>

`2.4.` [File]-[Verify drive/image] 를 통해 이미지 해시를 검증한다. (Computed Hash 계산)

<center><p><img src="/assets/2018-08-10-post-data_leakage_case_1/4.jpg"></p></center>

`2.5.` [View]-[Properties] 를 통해, 이미지의 해시값을 비롯한 여러 정보를 확인할 수 있다.

- Acquire date : 2015-04-23
- Acquired on OS : Windows 7
- Examiner : dForensic_Team

<center><p><img src="/assets/2018-08-10-post-data_leakage_case_1/5.jpg"></p></center>

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
