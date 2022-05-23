---
title: "2022-05-23-makingEmail"
lang: "ko"
layout: page
date: 2022-05-23 15:41:52 +9000
tags: "Computer"
type: documents
---
<!-- [[Computer]] -->
## AWS SES를 이용하여 개인 이메일 만들기

우리는 일상에서 네이버와 같은 웹 플랫폼에서 제공하는 메일 서비스를 자주 사용한다. 하지만 낭만을 위해... 개인 도메인을 이용하여 개인 이메일을 만들어볼 생각이다.

우선 구입한 개인 도메인이 있어야 한다. 만약 `Route53`에서 구입했을 경우, `호스팅 영역`을 살펴보면 해당 도메인에 여러 레코드가 설정되어 있는 것을 확인할 수 있는데, 무엇인지 하나씩 살펴보자.

### DNS 레코드의 종류

* SOA (Start Of Authority) : 도메인의 모든 정보와 권한을 의미한다. SOA 레코드가 없을 경우 다른 레코드를 등록할 수 없다.

* NS (Name Server) : 도메인의 네임서버를 지정하는 레코드. 일반적으로 사용자가 도메인 이름을 등록한 회사가 된다.
  
* A (Address Mapping Records) : IP 주소를 사용하여 도메인 이름을 개별 서버에 연결한다.

* AAAA (IP Version 6 Address Records) : 확장된 IP 주소를 연결하기 위한 레코드이다.

* CNAME (Canoncial NAME) : 구매한 도메인에 2차 도메인을 부여한다. 즉 도메인에 별칭을 부여한다고 볼 수도 있을 것 같다. A 레코드를 사용하여 서브 도메인이 해당 서버의 IP 주소를 가리키게 할 수도 있지만, CNAME은 A 레코드와 달리 IP 주소를 사용하지 않는다.

* MX (Mail eXchanger) : 특정 도메인에 대한 메일을 수신한다. 도메인의 MX 레코드에 이메일을 받을 메일 서버를 지정하면, 도메인으로 오는 메일은 해당 메일 서버로 전달된다.

* TEXT (TXT, Text) : 형식이 지정되지 않은 텍스트를 저장할 수 있다. 도메인의 TEXT 레코드를 저장함으로써 도메인의 구매자임을 인증하거나, 메일 서버의 경우에는 가짜 메일을 방지하기 위한 기능을 위해 사용한다.
  * SPF (Sender Policy Framework) : TEXT 레코드 안에서 사용되며, 메일 스푸핑을 방지하는데 사용된다.

참고:
[https://shutcoding.tistory.com/20](https://shutcoding.tistory.com/20)
[https://server-talk.tistory.com/176](https://server-talk.tistory.com/176)
[https://cheershennah.tistory.com/124](https://cheershennah.tistory.com/124)
[https://odenwar.net/dns-레코드-타입과-설명/](https://odenwar.net/dns-%EB%A0%88%EC%BD%94%EB%93%9C-%ED%83%80%EC%9E%85%EA%B3%BC-%EC%84%A4%EB%AA%85/)

