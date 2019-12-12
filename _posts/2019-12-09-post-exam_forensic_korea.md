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

한국포렌식학회 주관의 디지털포렌식전문가 2급 실기 시험과 관련하여, 한국포렌식학회에서 출판한 실기 책에 소개된 문제와 풀이방법은 실제 최신 시험 경향과 다르기 때문에 최신 경향에 맞는 풀이방법에 대해 기술한다. 이 밖에도 시험장에서의 분석 프로그램 설치, CD 굽기 등 평소 많이 접해보지 못한 내용 등 **실기 책에 기술되지 않은 내용**을 위주로 한다.

1. EnCase 설치
2. 이미징
3. VBR 복구
4. 증거물 찾기
5. 법률 관련 문제
6. 답안 및 증거물 제출(CD 굽기)

<br>
# 1. EnCase 설치

시험장 컴퓨터 바탕화면에 분석 프로그램 폴더가 존재한다. 해당 폴더 내에는 EnCase, FTK, Autopsy, KFolt, EnCase Imager, FTK Imager, HxD 프로그램과 EnCase License 활성화를 위한 파일들이 존재한다.(해당 도구 이외의 다른 도구 사용을 원할 시, 사전에 신청한 프로그램에 한해 반입 및 설치가 가능하다.)

필자의 경우 EnCase Certified Examiner를 취득하였고, 실무에서 EnCase를 계속해서 접하기 때문에 EnCase(분석), FTK Imager(이미징), HxD(파일 시그니처 복구 및 VBR 복구) 3가지를 설치하였다.

다른 프로그램과 다르게 **EnCase**는 설치 후, **License Activation** 절차를 진행해야 한다. 해당 절차를 시험장에서 처음 진행할 경우 당혹스러울 수 있으므로 사전에 절차를 숙지하도록 한다. License Activation 절차는 다음과 같다.

- EnCase 설치 프로그램 x64.exe 실행 및 설치(별도의 설정 변경없이 계속해서 다음을 클릭하여 진행)
- **EnCase 프로그램 실행 후, 다시 종료**(이 과정을 하지 않을 경우 내 문서 폴더 하위에 EnCase 관련 폴더가 생성되지 않는다.)
- 자격 증명 관련 파일 복사
  - **2개**의 파일 `내 문서` 폴더 하위의 EnCase 폴더 내 경로에 복사
  <br>Folder Path : ```C:\Users\sungsoo.kim\Documents\EnCase```(시험지에 기재되어 있음)
  <br><center><img src="/assets/폴더명/파일명.jpg" width="70%"><br><em>자격 증명 파일 복사 1</em></center>
  - **1개**의 파일(Cert)을 Program Files 폴더 하위의 EnCase 폴더 내 경로에 복사
  <br>Folder Path : ```C:\Program Files\EnCase8.08```(시험지에 기재되어 있음)
  <br><center><img src="/assets/폴더명/파일명.jpg" width="70%"><br><em>자격 증명 파일 복사 2</em></center>
- EnCase 프로그램 실행 후, **License Activation** 클릭

<br>
# 2. 이미징
- 논리적 쓰기 방지
- 증거물 이미지 생성
- 생성된 이미지 확인

## 2.1. 논리적 쓰기 방지

1. 레지스트리 값 변경
2. EnCase FastBloc 이용 방법

## 2.2. 증거물 이미지 생성

EnCase를 통해서 이미지 생성이 바로 가능하지만, FTK Imager를 이용한 이미지 생성 방법이 가장 직관적이고 쉽다. 또한, HxD를 통해 VBR 복구를 용이하게 하기 위해 DD 이미지 형태로 생성한다.

## 2.3. 생성된 이미지 확인
FTK Imager의 경우 이미지가 생성된 폴더에 이미지 정보가 텍스트 파일로 저장되어 있다. 해당 내용을 확인하여 이미지 정보를 답안에 기록한다.

<br>
# 3. VBR 복구
FTK Imager를 통해 생성된 이미지 확인 시, VBR 이 훼손되어 드라이브(볼륨) 하위 폴더가 제대로 보이지 않는 경우 Back-up VBR를 이용하여 VBR 복구를 진행한다.

1. HxD 디스크 이미지 열기 기능을 통해 DD 이미지 열기(디스크 이미지 열기 기능 사용 시, 섹터 단위로 이미지 확인 및 이동이 가능)
2. <표 1>를 참고하여 MBR에서 Volume Start Sector 및 Volume Size 정보를 확인
3. <표 2>를 참고하여 Back-up VBR 위치로 이동 및 복사
4. Volume Start Sector에 덮어씌우기(**Ctrl + B**)
<br>만약, 붙여넣기(Ctrl+V)를 하게 되면, 해당 영역이 수정되는 것이 아니라 추가되므로 이미지가 정상적으로 열리지 않는다.
5. 다른 이름으로 저장
6. 수정된 이미지 확인


|실기 책|최신 경향|
|---|---|
|USB 내 단일볼륨|USB 내 다수의 볼륨 존재|
|USB 첫 번째 섹터가 VBR|USB 첫 번째 섹터가 MBR|

<center><em>표 1. USB 첫 번째 섹터 비교</em></center>

<br>

|운영체제|VBR|Back-up VBR|
|---|---|
|FAT32|Volume Start Sector|Volume Start Sector + 6 |
|NTFS|Volume Start Sector|Volume Start Sector + Volume Size(Volume의 마지막)|

<center><em>표 2. 운영체제 별 Back-up VBR 위치</em></center>

<br>
# 4. 증거 파일 찾기

VBR 복구가 완료된 DD 이미지를 EnCase에 불러온 후, 프로세싱을 진행한다. 프로세싱 옵션은 프로세싱 시간을 고려하여 Recovered Folders, File Signature Analysis, Hash Analysis(MD5, SHA1) 옵션 3가지를 선택한다.

<center><p><img src="/assets/2019-12-09-post-exam_forensic_korea/프로세싱 옵션.jpg" width="100%"><br><em>프로세싱 옵션</em></p></center>

증거 파일을 찾는 방법은 실기 책에서 소개한 풀이방법과 다르게 접근하는 것이 좋다. 실기 책과 최신 시험 문제의 유형이 다르기 때문이다. 가장 큰 차이점은 <표 3>과 같다. 

|실기 책|최신 경향|
|---|---|
|운영체제가 설치되어 있지 않고, 시스템 파일 존재하지 않음|Windows-To-Go를 통해 USB 내 운영체제가 설치되어 있으며, 다수의 **시스템 파일** 존재|
|증거 파일의 안티포렌식 행위가 **확장자 변경**에 **한정**|증거 파일의 안티포렌식 행위가 **다양함**|

<center><em>표 3. 실기 책과 최신 시험 문제 유형 차이점</em></center>

<br>
최신 시험 유형은 Windows-To-Go를 통해 USB 내 운영체제를 설치하여 시스템 파일들이 많다. 그러므로, 실기 책에서 소개된 풀이방법과 같이 File Signature Analysis 결과로 필터링하여 Bad Signature, Alias 파일을 검토할 경우, 다수의 불필요한 파일들까지 검토하게 되어 시간적 소모가 크다. 또한, OOXML 문서(docx, pptx, xlsx) 내 증거를 은닉하는 경우, OOXML 문서 파일의 File Signature Analysis 결과가 Match 로 정상 판단되기 때문에 발견하기 어렵다.

필자가 제안하는 문제풀이 전략은 다음과 같다.

<div class="notice">
파일 생성 시간 순서로 정렬하여, 가장 최근부터 역순으로 검토 중 EnCase Doc 탭에서 내용이 확인이 되지 않는 파일을 중점적으로 안티포렌식 행위에 따라 적절한 조치 이후 파일 내용을 확인한다.
</div>

- 파일 이름 또는 시간 순서로 정렬 후, 파일 순차 검토
  - 시스템 파일을 포함하여 불필요한 파일들을 검토하는 시간을 줄이기 위함
  - **파일 이름**으로 정렬 : 파일 이름이 **한글**인 파일들(혐의자가 생성한 파일)을 중점적으로 검토
  - **파일 생성 시간**으로 정렬 : 가장 **최근**부터 역순으로 검토
- 파일 검토 시 체크 사항
  - EnCase **DOC 탭에서 내용(문서, 그림) 확인**, 확인되지 않을 경우 안티포렌식 행위로 손상된 증거 파일일 확률이 높음
  - File Signature Analysis 결과 확인 (`Bad Signature` / `Alias` / `Match`)
  - <표 4> 참고하여 안티포렌식 행위 별 조치 후, 파일 내용 확인
- **증거 파일 정보 기록**
  - EnCase Report 탭에서 **Hash 정보**와 **파일 경로**가 반드시 포함되도록 증거 파일의 메타 정보와 내용을 답안에 기록
<br>

|안티포렌식 행위|Fils Signature Analysis 결과|조치 방법|
|---|---|
|zip 파일의 확장자를 변경|Alias|파일 추출하여 확장자를 zip 으로 변경 후 내용 확인(또는, EnCase의 **View File Structure** 기능사용)|
|docx, jpg 파일의 시그니처 훼손|Bad Signature|파일 추출하여 HxD를 통해 파일 시그니처를 복구 후 내용 확인(파일 시그니처는 다른 정상 파일에서 확인)|
|OOXML 문서 파일(docx, pptx, xlsx) 내 증거 은닉|Match|OOXML 문서 파일은 압축 파일 형태이기 때문에, 해당 파일을 추출하여 확장자를 zip 으로 변경 후 내용 확인(또는, EnCase의 **View File Structure** 기능사용)|
|압축 파일에 비밀번호가 걸려 있음|-|비밀번호를 다른 증거 파일에서 찾아 압축 해제 후 내용 확인|

<center><em>표 4. 안티포렌식 행위에 따른 조치 방법</em></center>

<br>
# 5. 법률 관련 문제

- 전문법칙 (형사소송법 제 313조, 315조)
- 압수 절차, 피압수자 참여 보장, 제 3자 참여
- 별건 정보 발견 시 수사 중단 후, 압수수색 영장 별도로 청구

<br>
# 6. 답안 및 증거 파일 제출(CD 굽기)

답안과 증거 파일은 CD로 제출한다. 그러나, 요즘 실생활에서 CD를 많이 사용하지 않고, 노트북 또한 CD 드라이브를 지원하지 않는 경우가 많기 때문에 시험장에서 처음 진행할 경우 실수를 할 가능성이 있다. 제출용 CD는 여분이 따로 지급되지 않기 때문에, 사전에 절차를 숙지하도록 한다.

1. CD 삽입 후, **CD/DVD 플레이어에서 사용** 선택
2. 답안 및 증거 파일 복사
3. CD 선택 후, [관리]-[드라이브 도구]-[**굽기 완료**]

<br>
<center><p><img src="/assets/2019-12-09-post-exam_forensic_korea/디스크 굽기.png" width="60%"><br><em>CD/DVD 플레이어에서 사용</em></p></center>
<center><p><img src="/assets/2019-12-09-post-exam_forensic_korea/굽기 완료.jpg" width="100%"><br><em>굽기 완료</em></p></center>
<br>


> 참고
[디지털포렌식 자격검정시험](https://exam.forensickorea.org/)
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

