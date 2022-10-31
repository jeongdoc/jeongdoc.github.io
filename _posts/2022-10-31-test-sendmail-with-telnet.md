---
title: UBUNTU-LINUX Telnet으로 sendmail 테스트 해보기
date: '2022-10-31 17:00:42 +0900'
categories: [OS, UBUNTU-LINUX]
tags: [OS, ubuntu, linux, smpt]
---

### 들어가며

 현재 저는 공부용으로 라즈베리파이에 우분투 리눅스를 사용중입니다.     
MTA(Mail Transfer Agent)로 sendmail을 이용했습니다.    
구성 후 간단한 테스트를 위해 텔넷을 이용했던 내용을 기록합니다.

### 1. SMTP 포트 확인
 전제가 세팅 완료 이후이므로 세팅 이후 포트로 잘 동작하고 있는지 확인합니다.    
netstat으로 포트가 잘 열려있는지 확인할 수 있습니다.    
``` bash
netstat -tnlp | grep LISTEN
```

### 2. Telnet(텔넷)으로 테스트 해보기
 nslookup으로 사용가능한 메일서버를 찾을 수도 있습니다만, sendmail을 이용한 메일 전송 테스트가 목적이기 때문에 제 서버로 테스트를 진행합니다.    
``` bash
# 예시
telnet localhost 25
```

 위처럼 접근하면 아래와 같은 메세지를 볼 수 있습니다.
![""](https://jeongdoc.com/assets/img/2022/10/telnet-sendmail1.PNG)

흰색 네모 표기가 되어있는 곳은 서버의 IP나, 도메인등이 표기됩니다.

이후 발송주소를 입력합니다. mail from 이후 메일주소를 입력합니다.    
``` bash
mail from : sender@test.com
```

![""](https://jeongdoc.com/assets/img/2022/10/telnet-sendmail2.PNG)

mail from을 입력하고 나면 붉은색 네모 안에 표기된 메시지가 나오는데 SMTP 서버에 helo명령을 전송하여 SMTP를 식별하는 것입니다.    
보통은 SMTP의 도메인을 전송합니다. 여기서는 localhost로 접속하였다면 localhost라고 해주시면 됩니다.
``` bash
helo domain.co.kr
```
![""](https://jeongdoc.com/assets/img/2022/10/telnet-sendmail3.PNG)

~~ pleased to meet you라는 메세지가 나오면 잘 된 것입니다.    
이후 수신주소를 입력해야하는데요, rcpt to 이후 메일주소를 입력합니다.
``` bash
rcpt to : receiver@test.com
```

![""](https://jeongdoc.com/assets/img/2022/10/telnet-sendmail4.PNG)
![""](https://jeongdoc.com/assets/img/2022/10/telnet-sendmail5.PNG)

이때 Need to MAIL before RCPT라는 메세지가 나오는데, 앞서 HELO 명령어로 식별만 했을 뿐 발신주소가 인식된 것이 아닙니다.    
따라서 HELO 이후 한 번 더 mail from을 이용해 발신주소를 입력하면 됩니다. 이후 rcpt to로 수신주소를 입력하면 발신인/수신인 지정은 완료됩니다.    


이후 data 명령어로 보내고자 하는 내용을 작성합니다.    
내용을 다 작성하였다면 단일점(".")으로 끝내주시면 됩니다. Message accepted for delivery와 함께 완료되며 잘 진행됐는지 확인해보시면 됩니다.    

![""](https://jeongdoc.com/assets/img/2022/10/telnet-sendmail6.PNG)

모든걸 확인하셨다면 CTRL+], q로 접속을 종료해줍니다.    

### 3. SMTP commands 정리
위에서 사용하였던 명령어만 우선 정리합니다.    
더 자세한 것은 ["SMTP Commands 링크"](https://www.ibm.com/docs/en/zos/2.3.0?topic=set-smtp-commands) 에서 확인해주세요.

Subcommand|Description
:---:|:---|
DATA|텍스트 데이터로 메세지 본문 정의. DATA 본문 종료는 "."을 입력.
EHLO|HELO의 ESMTP(Enhanced SMTP) 버전. <br>ESMTP는 SMTP 프로토콜에 많은 필수 추가 사항을 추가함.
HELO|송신 서버는 자신이 누구인지 식별하고 수신 서버는(RFC에 따라) 지정된 이름을 수락함. <br>때문에 정확한 정보를 제공할 필요가 없으나 보통은 도메인 정보를 전달함.
MAIL FROM|발신 주소 지정
RCPT TO|수신 주소 지정

