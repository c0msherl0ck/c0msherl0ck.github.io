---
title: "피싱메일 분석(.eml) SPF(Sender Policy Framework)"
categories:
  - E-mail
tags:
  - SPF
  - Fishing mail
  - E-mail header
  - Recevied header
comments: true
---

기업의 침해사고에 있어서 가장 많은 부분을 차지하는 것 중에 하나가 임직원을 대상으로 하는 피싱메일이다. 이번 글에서는 실제 피싱메일 분석을 통해, SPF 값을 기반으로 피싱메일을 필터링하는 방법에 대해서 기술한다.

> 1. eml 파일 확인
> 2. E-mail Header 분석
> 3. E-mail Content 분석
> 4. Conclusion
> 5. Additional info about SPF
> 6. Additional info about Email Header



# 1. eml 파일 확인

분석한 실제 피싱메일 파일은 다음의 경로에서 다운로드 받을 수 있다.
[피싱메일 실습 다운로드](https://github.com/c0msherl0ck/c0msherl0ck.github.io/tree/master/assets/2018-08-27-post-Fishing_Email_Analysis/spam.eml)

## 1.1. EML Viewer 를 통한 확인

`https://www.encryptomatic.com/viewer`

<center><p><img src="/assets/2018-08-27-post-Fishing_Email_Analysis/1.1.png"></p></center>

## 1.2. MS OUTLOOK 을 통한 확인

<center><p><img src="/assets/2018-08-27-post-Fishing_Email_Analysis/1.2.png"></p></center>


# 2. Email HEADER 분석

<center><p><img src="/assets/2018-08-27-post-Fishing_Email_Analysis/2.png"></p></center>

## 2.1. Received-SPF : none

SENDER 주소 osman.alshikh@saudianfal.com 가 등록되지 않은 메일서버

## 2.2. Authentication-Results : spf=none

SENDER 주소 osman.alshikh@saudianfal.com 가 등록되지 않은 메일서버

## 2.3. X-session-IP : 108.66.43.104 조회

`http://ipaddress.is`

<center><p><img src="/assets/2018-08-27-post-Fishing_Email_Analysis/2.3.png"></p></center>

## 2.4. Received Part

```
Received:
	from (1)
	by (2)
	with (3) id (4)
	for (5);
	(6)
```

- (1) : 메일을 발송하는 서버의 주소 또는 호스트이름을 명시하는 부분으로 메일을 처음 발송하는 부분이 될 수도 있고 중간 경유지가 될 수도 있다.
- (2) : 해당 메일을 받는 서버의 호스트이름이나 주소
- (3) : SMTP와 같은 메일 프로토콜
- (4) : 해당 메일이 발송되는 서버나 수신되는 서버에서 해당 메일이 구분되어질 수 있는 고유한 식별자(Message-ID)
- (5) : 수신 받는 사용자의 이메일 주소
- (6) : 해당 메일이 수신된 날짜와 시간

```
[End Point]

Received:
	from smtp104.ord1c.emailsrvr.com (smtp104.ord1c.emailsrvr.com [108.166.43.104])
	by crcvmail12.nm.naver.com
	with ESMTP id ZYf9otdSSiqg5dsoO2Q4Bg
	for <charsyam@naver.com>;
	Wed, 22 Aug 2018 09:04:06 -0000

[Intermediate]

Received:
	from smtp14.relay.ord1c.emailsrvr.com (localhost [127.0.0.1])
	by smtp14.relay.ord1c.emailsrvr.com (SMTP Server)
	with ESMTP id 16DA0C0269
	for <charsyam@naver.com>;
	Wed, 22 Aug 2018 05:04:04 -0400 (EDT)

[Intermediate]

Received:
	by smtp14.relay.ord1c.emailsrvr.com (Authenticated sender: osman.alshikh-AT-saudianfal.com)
	with ESMTPSA id D7CAAC0259
	for <charsyam@naver.com>;
	Wed, 22 Aug 2018 05:04:03 -0400 (EDT)

[Start Point]

Received: from saudianfal.com ([UNAVAILABLE]. [45.126.211.209])
	(using TLSv1.2 with cipher DHE-RSA-AES256-GCM-SHA384)
	by 0.0.0.0:25 (trex/5.7.12);
	Wed, 22 Aug 2018 05:04:04 -0400
```

**"최초 이메일을 전송한 곳의 메일 서버가 존재하지 않는다. UNAVAILABLE"**


<center><p><img src="/assets/2018-08-27-post-Fishing_Email_Analysis/2.4.png"></p></center>


# 3. Email CONTENT 분석

## 3.1. 메일 내용 중 "3316", "88" 이라는 숫자 의심

- 88 번 port 는 커베로스 - 인증 에이전트에 사용되는 Well Known Port 이다.
- 3316 port 를 통해 어떠한 행위를 할 수 있는지 검색 결과, Trojan 이나 Virus 가 사용할 수도 있다는 내용이 있다.

<center><p><img src="/assets/2018-08-27-post-Fishing_Email_Analysis/3.1.png"></p></center>

## 3.2. content 내 의심되는 URL 찾기

`http://ygeucpu.cf/(.)/nav.php?email=charsyam@naver.com`

<center><p><img src="/assets/2018-08-27-post-Fishing_Email_Analysis/3.2.png"></p></center>

## 3.3. 해당 URL 접속

다음과 같이 사기성(Phishing) 사이트인 것을 알 수 있다.

<center><p><img src="/assets/2018-08-27-post-Fishing_Email_Analysis/3.3.png"></p></center>

# 4. Conclusion

<div class="notice">
피싱메일의 여부는 메일서버등록제의 SPF(Sender Policy Framework) 의 값을 통해 쉽게 확인가능하며, 핕터링 할 수 있다.
</div>

`Received-SPF` : "Pass" 가 아닌 것을 필터링 해준다면 강력한 피싱메일 방지 보안설정이 될 것이다.

<center><p><img src="/assets/2018-08-27-post-Fishing_Email_Analysis/4.png"></p></center>

# 5. Additional info about SPF

## 5.1. SPF(Sender Policy Framework) 란?

- 메일서버 정보를 사전에 DNS에 공개 등록함으로써 수신자로 하여금 이메일에 표시된 발송자 정보가 실제 메일서버의 정보와 일치하는지를 확인할 수 있도록 하는 인증기술
- 대다수 스팸발송자가 자신의 신원을 감추기 위하여 발송자 주소나 전송경로를 허위로 표기하거나 변경하는 경우가 많다는데 착안

## 5.2. SPF를 이용한 이메일 인증절차

- `발신자` : 자신의 메일서버 정보와 정책을 나타내는 SPF 레코드를 해당 DNS에 등록
- `수신자` : 이메일 수신시 발송자의 DNS에 등록된 SPF 레코드를 확인하여 해당 이메일에 표시된 발송IP와 대조하고 그 결과값에 따라 수신여부를 결정<br>(메일서버나 스팸차단솔루션에 SPF 확인기능이 설치되어 있어야 함)

# 6. Additional info about Email Header

## 6.1. 메일전송 과정

- User Agent
  - MUA(Mail User Agent) :  MTA와 사용자 간의 인터페이스를 제공하는데, 쉽게 이야기하면 PC에서 사용하는 "메일 프로그램"이라고 생각하면 된다. 대표적인 예로 유닉스의 메일프로그램인 /bin/mail, elm, pine 이나 유도라, 마이크로소프트 아웃룩 익스프레스 등이 있다.
- Server Agent
  - MTA(Mail Transfer Agent) : 메일 메시지를 저장하고 포워딩하거나 전송하는 역할을 한다. 대표적인 예로 일반적인 "메일서버" 프로그램인 sendmail이나 qmail 또는 마이크로소프트 서버 등이 있다.
  - MDA(Mail Delivery Agent) : sendmail 등의 MTA가 서버에서 수신한 메일을 /var/spool/mail/user 등과 같은 사용자의 로컬메일 박스 파일로 옮기는 역할을 한다. 대표적인 예로 procmail 이 있다.
- 메일 전송과정별 추가되는 헤더
  - 발송자, MUA(①) → ② →  ③ → MTA → ④ → MTA → ⑤ → MDA → ⑥ → MUA, 수신자
    - ① : 사용자가 MUA를 이용해 메일을 작성하는 과정에서는 다음과 같은 메일 헤더가 사용된다.
From, To, Cc, Bcc, Subject, Reply-to, Priority, Precedence, Resent-To, Resent-Cc
    - ② : MUA를 통해 메일을 발송할 때는 다음과 같은 헤더가 자동으로 추가된다.
Date, From, Sender, X-Mailer, Mime-Version, Content-Type, Content-Transfer-Encoding
    - ③ : MUA를 통해 발송된 메일을 MTA가 수신할 때는 다음과 같은 추가적인 헤더가 포함된다.
From, Date, Message-Id, Received, Return-Path
    - ④ : MTA에서 다른 MTA로 전송되는 과정에서는 다음과 같은 헤더가 추가된다.
"Received Header"
    - ⑤ : MTA가 MDA로 전송하는 과정에서 다음과 같은 헤더가 추가된다.
Apparently-To, From
    - ⑥ : 특별한 헤더가 추가되지 않는다.(???)


## 6.2. Received Header 

- 구조(2가지)
  - Received: ["from" 발송 호스트] "by" 수신 호스트 ["with" 메일 프로토콜] "id" 문자열 ["for 수신자메일주소" ] ";" 날짜 및 시각
  - Received: from 발송호스트의 이름([발송 호스트의 IP 주소]) by 수신 호스트 (수신서버의 메일 소프트웨어)

- 스팸매일 분석 시 활용방안<br>
  `Received: from`은 제일 아래 부분에서 위쪽으로 순서대로 메일이 전송된 것이므로, 실제 해당 메일을 처음 발송한 곳은 제일 아래쪽에 있는 Received: from 이다.<br><br>그러므로, **해당 메일을 처음 발송한 곳을 알려면 메일 헤더에서 제일 밑에 보이는 Received: from 부분을 살펴보면 된다. 그러나 전문적인 스패머라면 이런 추적을 어렵게 하기 위해 메일 발송 전에 위조된 Received: 헤더를 추가하는 경우가 많다.**

<br>

# 참고

[eml 파일 열기](http://gbworld.tistory.com/929)<br>

[SPF(Sender Policy Framework)를 사용하여 위조된 전자 메일 처리](https://docs.oracle.com/cd/E19957-01/820-0512/gdpno/index.html)<br>

[메일서버등록제(SPF: Sender Policy Framework)](https://www.kisarbl.or.kr/whiteip/whiteip_tutorial2.jsp)<br>

[port 3316](https://www.auditmypc.com/tcp-port-3316.asp)<br>

[이메일 헤더 분석](http://maj3sty.tistory.com/980)<br>

[메일 헤더 분석하기](http://www.ylabs.co.kr/index.php?document_srl=6020&mid=board_centos&search_target=regdate&search_keyword=201009&sort_index=readed_count&order_type=desc)<br>

[Email 구조, 왜 헤더가 중요한가?](http://yahon.tistory.com/55)<br>
