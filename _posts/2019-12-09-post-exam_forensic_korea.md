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

한국포렌식학회 주관의 디지털포렌식전문가 2급 실기 시험과 관련하여, 한국포렌식학회에서 출판한 실기 책에 소개된 문제와 풀이방법은 실제 최신 시험 경향과 다르기 때문에 최신 경향에 맞는 풀이방법에 대해 기술한다. 이 밖에도 시험장에서의 분석 프로그램 설치, CD 굽기 등 평소 많이 접해보지 못한 내용 등 실기 책에 기술되지 않은 내용을 위주로 한다.

- EnCase 설치
- 이미징
- VBR 복구
- 증거물 찾기
- 법률 관련 문제
- 답안 및 증거물 제출(CD 굽기)


# EnCase 설치

시험장 컴퓨터 바탕화면에 분석 프로그램 폴더가 존재한다. 해당 폴더 내에는 EnCase, FTK, Autopsy, KFolt, EnCase Imager, FTK Imager, HxD 프로그램과 EnCase License 활성화를 위한 파일들이 존재한다.

필자의 경우 EnCase Certified Examiner를 취득하였고, 실무에서 EnCase를 계속해서 접하기 때문에 EnCase 8.08(분석), FTK Imager 3.0.4.2(이미징), HxD(파일 시그니처 복구 및 VBR 복구) 3가지를 설치하였다.

다른 분석 프로그램과 다르게 EnCase의 경우 설치 후, 추가적으로 License Activation 절차를 진행해야 한다. 해당 절차를 시험장에서 처음 접할 경우 당혹스러울 수 있다. EnCase 설치 및 자격 활성화 절차는 다음과 같다.

1. EnCase 설치 프로그램 x64.exe 실행 및 설치(별도의 설정 변경없이 계속해서 다음을 클릭하여 진행)
2. **EnCase 프로그램 실행 후, 다시 종료**(이 과정을 하지 않을 경우 내 문서 폴더 하위에 EnCase 관련 폴더가 생성되지 않는다.)
3. 자격 증명 관련 파일 복사
   3.1. 2개의 파일 `내 문서` 폴더 하위의 EnCase 폴더 내 경로에 복사 폴더 경로(상세 경로는 시험지에 기재되어 있음) C:\Users\sungsoo.kim\Documents\EnCase
<center><p><img src="/assets/폴더명/파일명.jpg" width="70%"><br><em>자격 증명 파일 복사 1</em></p></center>
   3.2. 1개의 파일(Cert)을 Program Files 폴더 하위의 EnCase 폴더 내 경로에 복사
폴더 경로(상세 경로는 시험지에 기재되어 있음) : C:\Program Files\EnCase8.08
<center><p><img src="/assets/폴더명/파일명.jpg" width="70%"><br><em>자격 증명 파일 복사 2</em></p></center>
4. EnCase 프로그램 실행 후, **License Activation** 클릭


# 이미징
- 논리적 쓰기 방지
- 증거물 이미지 생성
- 생성된 이미지 확인

## 논리적 쓰기 방지

1. 레지스트리 값 변경
2. EnCase FastBloc 이용 방법

## 증거물 이미지 생성

EnCase를 통해서 이미지 생성이 바로 가능하지만, FTK Imager를 이용한 이미지 생성 방법이 가장 직관적이고 쉽다. 또한, HxD를 통해 VBR 복구를 용이하게 하기 위해 DD 이미지 형태로 생성한다.

## 생성된 이미지 확인
FTK Imager의 경우 이미지가 생성된 폴더에 이미지 정보가 텍스트 파일로 저장되어 있다. 해당 내용을 확인하여 이미지 정보를 답안에 기록한다.

# VBR 복구
FTK Imager를 통해 생성된 이미지 확인 시, VBR 이 훼손되어 드라이브 하위 폴더가 제대로 보이지 않는 경우 HxD를 통해 VBR 복구를 진행한다.
- Back-up VBR
- HxD를 통한 VBR 복구

## Back-up VBR

FAT32의 경우 Back-up VBR이 기존 VBR(Volume Start Sector) + 6 의 위치에 존재한다.
NTFS의 경우 Back-up VBR이 해당 Volume의 마지막에 존재한다.(Volume Start Sector + Volume Size를 통해 접근)

|운영체제|VBR|Back-up VBR|
|---|---|
|FAT32|Volume Start Sector|Volume Start Sector + 6 |
|NTFS|Volume Start Sector|Volume Start Sector + Volume Size|

Volume Start Sector 및 Volume Size는 MBR에서 확인할 수 있다. MBR은 증거물의 가장 첫번째 섹터에 존재하고, VBR은 각 볼륨의 첫 번째 섹터에 존재하나, USB 내 Volume이 한 개만 존재할 경우 증거물의 첫 번째 섹터에 VBR이 존재한다. (최근 경향은 USB 증거물이 Windows-to-go 형태로 운영체제가 설치되어 있고, 여러 드라이브가 존재하는 형태가 존재한다. 한국포렌식학회 출판 실기 책에서는 USB 내 단일 볼륨으로 VBR이 가장 먼저 등장한다.)

## HxD를 통한 VBR 복구

1. HxD 디스크 이미지 열기 기능을 통해 DD 이미지 열기(디스크 이미지 열기 기능 사용 시, 섹터 단위로 이미지 확인 및 이동이 가능)
2. MBR에서 확인한 Volume Start Sector 및 Volume Size 정보를 통해 Back-up VBR로 이동
3. Back-up VBR 복사(Ctrl + C)
4. Volume Start Sector에 덮어씌우기(**Ctrl + B**)
  만약, 붙여넣기(Ctrl+V)를 하게 되면, 해당 영역이 수정되는 것이 아니라 추가되므로 이미지가 정상적으로 열리지 않는다.
5. 다른 이름으로 저장
6. 수정된 이미지 EnCase 에서 확인


# 증거 파일 찾기

VBR 복구가 완료된 .dd 이미지를 EnCase에 불러온 후, 프로세싱을 진행한다. 프로세싱 옵션은 프로세싱 시간을 고려하여 Recovered Folders, File Signature Analysis, Hash Analysis(MD5, SHA1) 옵션 3가지를 우선적으로 선택한다.

<center><p>
<img src="/assets/폴더명/파일명.jpg" width="100%"><br>
<em>프로세싱 옵션</em>
</p></center>

증거 파일을 찾는 방법은 한국포렌식학회에서 출판한 실기 책의 풀이방법과 다르게 접근하는 것이 좋다. 실기 책에서 풀이한 문제 유형과 최근 문제 유형이 다르기 때문이다. 가장 큰 차이점은 USB 내 Windows-To-Go를 통해 운영체제가 설치되어 있어, 시스템 파일들이 많다는 점이며 이러한 파일들은 EnCase의 File Signature Analysis 결과 Bad Signature, Alias로 판단되기 때문에 기존 실기 책에 소개된 풀이 방법과 같이 File Signature Analysis 결과로 필터링할 경우 다수의 불필요한 파일들까지 검토해야 하기에 시간적 소모가 크다. 또한, OOXML 문서(docx, pptx, xlsx) 내 문서를 은닉하는 경우, File Signature Analysis 결과 Match 로 정상 판단되기 때문에 발견하기 어렵다.

|실기 책|최신 시험|
|---|---|
|USB 내 단일볼륨(USB 첫 번째 섹터가 VBR)|USB 내 다수의 볼륨 존재(USB 첫 번째 섹터가 MBR)|
|운영체제가 설치되어 있지 않고, 시스템 파일 존재하지 않음|운영체제가 설치되어 있으며, 다수의 시스템 파일 존재|
|증거 파일의 안티포렌식 행위가 확장자 변경에 한정|증거 파일의 안티포렌식 행위가 다양함|

필자가 제안하는 문제풀이 전략은 다음과 같다.

<div class="notice">
파일 생성 시간 순서로 정렬하여, 가장 최근부터 역순으로 검토 중 EnCase Doc 탭에서 내용이 확인이 되지 않는 파일을 중점적으로 안티포렌식 행위에 따라 적절한 조치 이후 파일 내용을 확인한다.
</div>

|안티포렌식 행위|조치 방법|
|---|---|
|zip 파일의 확장자를 다른 것으로 변경|File Signature Analysis 결과가 Alias 이다. 해당 파일 추출하여 확장자를 zip 으로 변경 후 내용 확인|
|docx, jpg 파일의 시그니처 훼손|File Signature Analysis 결과가 Bad Signature 이다. 해당 파일을 추출하여 HxD를 통해 파일 시그니처를 복구 후 내용 확인|
|OOXML 파일(docx, pptx, xlsx) 내 문서 은닉|OOXML 파일은 압축 파일 형태이기 때문에, 해당 파일을 추출하여 확장자를 zip 으로 변경 후 열람|
|zip 파일의 확장자를 변경|OOXML 파일은 압축 파일 형태이기 때문에, 해당 파일을 추출하여 확장자를 zip 으로 변경 후 열람|

1. 파일 이름 또는 시간 순서로 정렬 후, 파일 순차 검토(시스템 파일을 포함하여 불필요한 파일들을 검토하는 시간을 줄이기 위해서)
   1.1. 파일 이름으로 정렬 시, 파일 이름이 **한글**인 파일들(혐의자가 생성한 파일)을 중점적으로 검토
   1.2. 파일 생성 시간 순서로 정렬 시, 가장 **최근**부터 역순으로 검토
2. 파일 검토 시 체크 사항
   2.1. EnCase **DOC 탭에서 내용(문서, 그림) 확인**, 확인되지 않을 경우 안티포렌식 행위로 손상된 증거 파일일 확률이 높음
   2.2. File Signature Analysis 확인 (`Bad Signature` / `Alias` / `Match`)
      2.2.1. `Bad Signature` : 파일 시그니처가 손상 되었으므로, 해당 파일을 추출 후 HxD를 통해 파일 시그니처를 복구한다.(Bad Signature의 경우 EnCase Doc 탭에서 내용 확인이 불가능하다.) 해당 확장자에 해당하는 파일 시그니처는 별도로 외우고 있지 않아도, 다른 정상 파일의 파일 시그니처를 참고하여 확인할 수 있다.
      2.2.2. `Alias` : 파일 시그니처와 파일 확장자가 다르므로, 확장자를 시그니처에 맞게 변경한 후 내용을 확인한다.(Alias의 경우 EnCase Doc 탭에서 내용 확인이 가능하다.) EnCase의 File Type 컬럼은 File Signature에 따른 파일 형태를 구분하며, Category 컬럼은 파일 확장자에 따라 파일 형태를 구분한다.   
      2.2.3. `Match` : OOXML 문서 파일(docx, pptx, xlsx) 내 문서를 은닉할 경우, File Signature Analysis 결과가 Match로 정상으로 분류된다. 이 경우 해당 파일을 추출하여 확장자를 zip 으로 변경하거나, EnCase의 **View File Structure** 기능을 통해 은닉된 파일을 확인할 수 있다.

3. 증거 파일 정보 기록
  EnCase Report 탭에서 Hash 정보와 파일 경로가 반드시 포함되도록 증거 파일의 메타 정보와 내용을 답안에 기록한다.


# 법률 관련 문제

- 전문법칙 (형사소송법 제 313조, 315조)
- 압수 절차, 피압수자 참여 보장, 제 3자 참여
- 별건 정보 발견 시 수사 중단 후, 압수수색 영장 별도로 청구

# 답안 및 증거물 제출(CD 굽기)

답안과 증거물은 CD로 제출하지만, 요즘 실생활에서 CD를 많이 사용하지 않고, 노트북 또한 CD 드라이브를 지원하지 않는 경우가 많으므로 절차가 생소할 수 있으므로 사전에 절차를 확인하는 것이 좋다.

1. CD 삽입 후, CD/DVD 로 사용 선택
  사진 첨부
2. 답안 및 증거 파일 복사
  사진 첨부
3. CD 굽기
  사진 첨부


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

