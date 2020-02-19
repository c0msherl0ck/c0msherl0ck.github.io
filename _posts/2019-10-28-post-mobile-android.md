---
title: "[모바일 포렌식] 안드로이드 핸드폰 증거 획득(이미징)"
categories:
  - Mobile Forensic
tags:
  - mobile
  - forensic
  - android
  - samsung galaxy
  - adb
comments: true
---

안드로이드 운영체제를 사용하는 핸드폰의 경우, adb(Android Debug Bridge)를 통해 backup 명령어를 통해 **논리적 이미지**를 획득하는 방법과 dd 명령어를 입력함으로써 **물리적 이미지**를 획득하는 방법이 있다. 이번 글에서는 **물리적 이미지** 획득을 위한 Rooting, BusyBox 설치 등 이미징 사전 준비단계부터 이미지 획득까지 전 과정에 대해 서술한다.

> 1. adb(Android Debug Bridge) 설치
> 2. 핸드폰 루팅하기
> 3. 핸드폰에 BusyBox 설치하기(netcat 기능 지원)
> 4. 컴퓨터에 netcat 설치하기
> 5. 안드로이드폰 이미지 획득하기
> 6. 참고

# 1. adb(Android Debug Bridge) 설치

adb란? Android Debug Bridge 의 약자로, PC에서 명령어를 통해 안드로이드 핸드폰을 제어할 수 있도록 하는 개발자 도구이다. 이름에서 알 수 있듯이 adb 명령어를 사용하기 위해서는 핸드폰에서 `USB 디버깅 모드`가 선택되어 있어야 한다.

<https://developer.android.com/studio/releases/platform-tools>

먼저, 상기의 URL에서 `Windows 용 SDK 플랫폼 도구 다운로드` 클릭을 통해, adb 압축파일을 다운로드 받는다. 해당 압축 파일을 해제하고 생성된 폴더의 경로를 **환경변수** 상에 추가하면, windows의 cmd 창에서 adb 명령어를 사용할 수 있다.

<center><p>
<img src="/assets/2019-10-28-post-mobile-android/adb_system_path.jpg" width="100%">
<em>adb 폴더 환경변수 추가</em>
</p></center>

adb가 제대로 작동하는지 확인하기 위해, cmd 창에서 `adb devices`를 입력하고 결과를 확인한다. 정상적으로 설치가 되었을 경우 다음과 같은 연결 정보가 뜬다.

<center><p>
<img src="/assets/2019-10-28-post-mobile-android/adb_devices.jpg" width="100%">
<em>adb devices 결과</em>
</p></center>

# 2. 핸드폰 루팅하기

이미지 획득에 필요한 dd 명령어와 파일 접근 권한은 root 권한이 필요하다. 기본적으로 안드로이드 핸드폰의 경우 root 권한이 아닌 user 권한을 가지고 있기 때문에 별도의 루팅 과정이 필요하다. 

<https://autoroot.chainfire.eu/>

먼저, 상기의 사이트에서 자신의 핸드폰 기종을 확인하여 루팅에 필요한 파일을 다운로드 받는다. 필자의 경우 다음과 같이 Samsung SHV-E330K 모델에 해당하는 파일을 다운로드하였다.

<center><p>
<img src="/assets/2019-10-28-post-mobile-android/autoroot.jpg" width="100%">
<em>autoroot 사이트</em>
</p></center>

다운로드 파일의 합축을 해제하면, 다음과 같이 `odin` 소프트웨어와 루팅에 필요한 파일이 존재한다. 

<center><p>
<img src="/assets/2019-10-28-post-mobile-android/download_files.jpg" width="100%">
<em>다운로드 파일</em>
</p></center>

핸드폰을 **다운로드 모드**로 부팅한다.(부팅 시 `볼륨 버튼 하 버튼 + 홈 버튼 + 전원 버튼` 동시 누름)

<center><p>
<img src="/assets/2019-10-28-post-mobile-android/download_mode.jpg" width="50%"><br>
<em>다운로드 모드 화면</em>
</p></center>

`odin`을 실행하고, `AP` 버튼을 클릭하여 다운로드 받은 파일 중 md5 파일을 선택하고, `start`를 누른다.

<center><p>
<img src="/assets/2019-10-28-post-mobile-android/odin.jpg" width="100%">
<em>odin software</em>
</p></center>

완료 시 다음과 같이 obin software에서는 `Succeed` 메세지를 확인할 수 있고, 핸드폰에서는 `SuperUser 파일`이 생성된 것을 확인할 수 있다.

<center><p>
<img src="/assets/2019-10-28-post-mobile-android/odin_succeed.jpg" width="100%">
<em>odin software 의 succeed 메세지</em>
</p></center>

<center><p>
<img src="/assets/2019-10-28-post-mobile-android/SuperUser.jpg" width="50%"><br>
<em>SuperUser 파일</em>
</p></center>

루팅이 제대로 되었는지 확인하기 위해 cmd 창에서 다음의 명령어를 입력한다. 명령어 입력 시 핸드폰에서 루트권한 요청하는 알림이 뜬다. `허용`을 클릭한다.

<div class="notice">
adb shell<br>
su
</div>

<center><p>
<img src="/assets/2019-10-28-post-mobile-android/root_permission.jpg" width="50%"><br>
<em>핸드폰에서 루트권한 요청 허용</em>
</p></center>

`su` 입력 시 명령어 입력창이 `#` 형태로 바뀌면 root 권한으로 상승한 것으로 루팅이 성공한 것이다. 만약, 권한이 없다는 메세지가 뜬다면 루팅에 실패한 것이다.

<center><p>
<img src="/assets/2019-10-28-post-mobile-android/su.jpg" width="100%">
<em>su 명령어를 통한 root 권한 취득 확인</em>
</p></center>

# 3. 핸드폰에 BusyBox 설치하기(netcat 기능 지원)

BusyBox 란 리눅스 명령어 모음으로, 안드로이드 폰 내에서 리눅스 명령어를 사용할 수 있도록 해준다. 이미지 획득시 **netcat** 명령어(기능)을 사용하기 위해 BusyBox를 설치한다.

<https://androidapksfree.com/busybox/busybox-latest-version-apk-download/>

먼저, 상기의 사이트에서 BusyBox.apk 를 다운로드 한다. 이후 BusyBox.apk 파일이 존재하는 폴더 경로에서 cmd 창을 실행 후(또는, cmd 창에서 해당 파일이 있는 경로로 이동한다.), 다음의 명령어를 통해 핸드폰에 BusyBox를 설치한다.

<div class="notice">
adb -d install BusyBox.apk
</div>

<center><p>
<img src="/assets/2019-10-28-post-mobile-android/busybox_install.jpg" width="100%">
<em>BusyBox 설치</em>
</p></center>

설치가 완료되면 다음과 같이 핸드폰에서 BusyBox 앱을 확인할 수 있다.

<center><p>
<img src="/assets/2019-10-28-post-mobile-android/busybox_1.jpg" width="50%">
<img src="/assets/2019-10-28-post-mobile-android/busybox_2.jpg" width="50%">
<br>
<em>BusyBox 설치 확인</em>
</p></center>

# 4. 컴퓨터에 netcat 설치하기

<https://eternallybored.org/misc/netcat/>

상기의 사이트에서 **netcat**을 다운로드 받고, `nc.exe`를 `C:\Windows\System32` 경로에 복사 붙여넣기 한다. 해당 경로는 cmd 창에서 사용할 수 있는 윈도우 기본 명령어들이 존재하는 곳이다.

# 5. 안드로이드폰 이미지 획득하기

cmd 창을 2개 띄운다. (CMD_1)하나는 컴퓨터에 명령어를 입력하기 위함이고, (CMD_2)다른 하나는 adb 를 통해 핸드폰에 명령어를 입력하기 위함이다.

먼저, 컴퓨터와 핸드폰을 TCP 로 연결한다. TCP 연결을 맺는 것은 dd 이미지를 네트워크 포트 8888을 통해 핸드폰으로부터 컴퓨터로 전달하기 위함이다.

<div class="notice">
CMD_1 터미널 : adb forward tcp:8888 tcp:8888
</div>
<center><p>
<img src="/assets/2019-10-28-post-mobile-android/tcp.jpg" width="100%">
<em>tcp 연결</em>
</p></center>

su 명령어를 통해 root 계정으로 변경한다.(`ls /data` 는 root 권한으로만 실행이 가능한 명령어로 root 권한을 취득했는지 확인하기 위한 테스트 용도이다.)
<div class="notice">
CMD_2 터미널 : adb -d shell<br>
CMD_2 터미널 : su
</div>
<center><p>
<img src="/assets/2019-10-28-post-mobile-android/switch_user_root.jpg" width="100%">
<em>switch user to root</em>
</p></center>

dd 명령어를 통해 물리 이미지를 획득하고, 파이프라인에 연결하여 netcat을 통해 핸드폰에서 컴퓨터로 전송한다.
<div class="notice">
CMD_2 터미널 : dd if=/dev/block/mmcblk0 | busybox nc -l -p 8888
</div>
<center><p>
<img src="/assets/2019-10-28-post-mobile-android/dd.jpg" width="100%">
<em>dd 명령어를 통한 이미지 획득 및 netcat을 통한 전송</em>
</p></center>
<div class="notice">
CMD_1 터미널 : nc 127.0.0.1 8888 > android_data.dd
</div>
<center><p>
<img src="/assets/2019-10-28-post-mobile-android/nc_computer.jpg" width="100%">
<em>netcat을 이용한 데이터 수신 및 컴퓨터 내 파일 생성</em>
</p></center>

이미지 획득이 완료되면 다음과 같은 메세지가 뜬다. 필자의 경우 약 15GB의 이미지를 획득하는데 1시간 정도 소요되었다.

<center><p>
<img src="/assets/2019-10-28-post-mobile-android/imaging_completed.jpg" width="100%">
<img src="/assets/2019-10-28-post-mobile-android/final.jpg" width="100%">
<em>이미지 획득 완료</em>
</p></center>


# 6. 참고

[Android Forensics: imaging android filesystem using ADB and DD](https://www.andreafortuna.org/2018/12/03/android-forensics-imaging-android-file-system-using-adb-and-dd/)
<br>
[Imaging Android with ADB, Root, Netcat and DD](https://dfir.science/2017/04/Imaging-Android-with-root-netcat-and-dd.html)
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
