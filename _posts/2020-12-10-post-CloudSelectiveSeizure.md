---
title: "[논문리뷰] 클라우드 환경에서의 선별 수집 방안"
categories:
  - Cloud
tags:
  - 클라우드
  - 선별압수
  - API
comments: true
---

"클라우드 스토리지 서비스에 대한 메타데이터 기반 파일 선별 수집 방법 및 구현", Journal of Digital Forensics, 2020. 09 논문 리뷰


# 기존방식과 한계점

> 로컬로 전부 다운로드 한 이후 선별작업

Magnet Forensics 사의 Axiom, OpenText 사의Encase 등 기존 포렌식 도구들의 경우 선별 수집 보다는 저장소 내의 파일들을 내려받은 후 분석한다. 
해당 방법은 내려받는 파일들이 대용량 파일일 경우 현장에서 분석하는데 많은 시간이 소요된다.

> 클라우드에 접속하여 필요한 파일 선별 및 다운로드

수사관이 클라우드에 직접 접속하여 탐색할 경우 클라우드 별 UX/UI가 달라 효율성과 효과성이 떨어진다.


# API를 이용한 선별 작업

클라우드 사업자가 제공하는 API를 통해 파일 리스트와 파일에 관한 Metadata를 먼저 확보한 이후, 
해당 정보를 바탕으로 필요한 파일에 대해서만 다운로드 한다.

## 장점

필요한 파일에 대해서만 다운로드 하므로 선별 압수에 걸리는 시간을 단축시킬 수 있으며, 
기존 도구와 동일한 UI/UX 를 바탕으로 작업을 할 수 있다.

Metadata를 기반으로 특정 기간 업로드 된 파일들에 대해서 선별 압수가 가능하다.

## 한계점

로컬 내 파일이 존재하지 않을 경우, 파일 내 데이터를 상대로 Keyword Search는 여전히 불가능하다.


# Google Drive API

논문 내 소개된 도구는 드랍박스, 원드라이브에 대해서만 지원을 하고 있으며 Google Drive가 누락된 것이 아쉬워 관련 내용을 살펴보았다.

Google Drive API의 경우 아래 링크에서 확인할 수 있으며 이를 통해 드라이브 내 파일 정보를 확인해보았다.

[https://developers.google.com/drive/api/v3/quickstart/python]

<center><p><img src="/assets/폴더명/파일명.jpg"><br><em>파일명, id 확인</em></p></center>


# 참고

"클라우드 스토리지 서비스에 대한 메타데이터 기반 파일 선별 수집 방법 및 구현", Journal of Digital Forensics, 2020. 09

