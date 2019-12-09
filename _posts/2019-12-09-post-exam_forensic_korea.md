---
title: "디지털포렌식전문가 2급 실기"
categories:
  - Certificate
tags:
  - 디지털포렌식전문가 2급
  - EnCase
  - FTK Imager
  - HxD
  - VBR
comments: true
---

한국포렌식학회 주관의 디지털포렌식전문가 2급 실기 시험 후기와 전략에 대해 기술한다. 필자의 경우 한국포렌식학회에서 출판한 실기 책을 공부하였으나, 시험문제의 유형이 다르기 때문에 문제풀이 전략을 다르게 세워야겠다고 느꼈고, 이외에도 시험장에서의 분석 프로그램 설치, CD 굽기 등 평소 많이 접해보지 않은 사항들에 대해 기술한다.

- EnCase 설치
- 문제풀이 전략
-- 이미징
-- VBR 복구
-- 증거물 찾기
-- 법률 관련 문제
- 증거물 제출(CD 굽기)

# EnCase 설치

시험장 컴퓨터 바탕화면에 분석 프로그램 폴더가 존재한다. 해당 폴더 내에는 EnCase, FTK, Autopsy, KFolt, EnCase Imager, FTK Imager, HxD 프로그램과 EnCase License 활성화를 위한 파일들이 존재한다.

필자의 경우 EnCase Certified Examiner를 취득하였고, 실무에서 EnCase를 계속해서 접하기 때문에 EnCase 8.08(분석), FTK Imager 3.0.4.2(이미징), HxD(파일 시그니처 복구 및 VBR 복구) 3가지를 설치하였다.

다른 프로그램과 다르게 EnCase 8.08의 경우 설치 후 License Activation 절차를 시험장에서 처음할 경우 당혹스러울 수 있다. 설치 및 자격 활성화 절차는 다음과 같다.

1. EnCase 설치 프로그램 x64.exe 실행 및 설치(별도의 설정 변경없이 계속해서 다음을 클릭하여 진행)
2. EnCase 프로그램 실행 후, 다시 종료(이 과정을 하지 않을 경우 User 폴더 하위에 EnCase 관련 폴더가 생성되지 않는다.)
3. 자격 증명 관련 파일 복사
 3.1. 2개의 파일 User 폴더 하위의 EnCase 폴더 내 경로에 복사(상세 경로는 시험지에 기재되어 있음)
 3.2. 1개의 파일(Cert)을 Program Files 폴더 하위의 EnCase 폴더 내 경로에 복사(상세 경로는 시험지에 기재되어 있음)
4. EnCase 프로그램 실행 후, **License Activation** 클릭

# 이미징

## 논리적 쓰기 방지

1. 레지스트리 값 변경
2. EnCase FastBloc 이용 방법

## 증거물 이미지 생성

EnCase를 통해서 이미지 생성이 바로 가능하지만, FTK Imager를 이용한 이미지 생성 방법이 가장 직관적이고 쉽다. VBR 복구 시, HxD를 통해 진행하기 위해 DD 이미지 형태로 생성한다.

## 생성된 이미지 확인
아래의 두 해시값 비교
Acqusition Hash
Verification Hash

# VBR 복구

FTK Imager를 통해 생성된 이미지 확인 시, VBR 이 훼손되어 드라이브 하위 폴더가 제대로 보이지 않는 경우 HxD를 통해 VBR 복구를 진행한다.

FAT32의 경우 Back-up VBR이 기존 VBR(Volume Start Sector) + 6의 위치에 존재한다.
NTFS의 경우 Back-up VBR이 해당 Volume의 마지막에 존재한다.(Volume Start Sector + Volume Size를 통해 접근)

Volume Start Sector 및 Volume Size는 MBR에서 확인할 수 있다.
MBR은 증거물의 가장 첫번째 섹터에 존재하나, USB 내 Volume이 한 개만 존재할 경우 바로 VBR이 나온다.
(최근 경향은 USB 증거물이 Windows-to-go 형태로 운영체제가 설치되어 있고, 여러 드라이브가 존재하는 형태가 존재한다. 한국포렌식학회 출판 실기 책에서는 USB 내 단일 볼륨으로 VBR이 가장 먼저 등장한다.)

# 증거 파일 찾기

VBR 복구가 완료된 .dd 이미지를 EnCase에 불러온 후, 프로세싱을 진행한다. 프로세싱 옵션은 Recovered Folders, File Signature Analysis, Hash Analysis(MD5, SHA1) 옵션 3가지를 우선적으로 선택한다.



[인라인 링크](https://)


<center><p>
<img src="/assets/폴더명/파일명.jpg" width="100%">
<em>주석</em>
</p></center>

> 참고

[](https://)


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

