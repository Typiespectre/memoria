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

### AWS SES에 도메인 인증하기

다시 돌아와서, 개인 도메인으로 이메일을 발송하기 위해 AWS SES에서 도메인을 인증받아야 한다.

AWS SES(Simple Email Service)는 미국 동부(버지니아 북부), 유럽 (아일랜드), 미국 서부 (오레곤) 리전만 지원하기에, 우측 상단에서 리전을 셋 중 하나로 맞춘다.

AWS SES에 들어갔다면, 좌측 대시보드의 `Configuration - Verified identities`에서, `Create Identity`를 클릭한다.

`Create identity` 페이지에서, `Identity details`의 `Identity type`를 `Domain`으로 설정한 후, 아래의 `Domain` 입력칸에 구입한 도메인을 적는다. 아래 몇 가지 설정은 따로 건드리지 않았다.

위에서 `Route 53`에서 구입한 도메인을 이야기하였는데, `Route 53`에서 도메인을 구입하였을 경우, 따로 DKIM 설정을 하지 않아도 AWS SES에서 자동으로 처리가 되는 편리함이 있다. 만약 외부에서 구입한 도메인이라면, 72시간 안에 도메인을 구입하였던 호스팅 서비스로 이동하여 DKIM 레코드를 설정해주어야 한다. 도메인에 DKIM 레코드가 잘 설정되면, 이메일로 DKIM이 잘 설정되었다는 메세지를 받을 수 있다.

* DKIM (Domain Keys Identified Mail) : 메일 발송자의 도메인과 메일 내용의 무결성을 검증할 수 있는 기술. DKIM 기술은 이메일 스푸핑을 막기 위해 사용하는 인증 방법이다. TXT 레코드로도 이메일 스푸핑을 방지할 수 있지만, AWS SES는 DKIM을 이용하여 이메일 위조를 방지한다.

모든 과정이 완료되었다면, 도메인 대쉬보드(`Verified Identities`)에서 해당 도메인의 상태가 초록색 Verified로 변경된다.

### 이메일 보내기

검증이 완료된 도메인으로 이동하여, 우측 상단의 `Send test email`을 클릭한다.

아참, 테스트 이메일을 보내기 위해선, 위와 비슷한 방식으로 `Create Identity`에서 `Email Address`를 인증하면 된다. 현재 등록된 도메인은 아직 샌드박스 환경이기에, 등록된 이메일에서만 수신과 송신 테스트를 진행할 수 있다.

`Send test email`을 클릭 후, `From-adddress`에서 원하는 이름을 적은 뒤, `Scenario`를 `Custom`으로 설정하고, `Custom recipient`에 등록된 이메일 주소를 작성한다. 마지막으로 `Subject`와 `Body`에 테스트용 문구를 작성하고 우측 하단 `Send test email`을 누르면 끝. 등록된 이메일 주소로 이동해서 이메일이 정상적으로 송신되었는지 확인해보자.

참고:  
[https://musma.github.io/2019/09/16/what-to-do-after-you-buy-your-new-domain-on-aws.html](https://musma.github.io/2019/09/16/what-to-do-after-you-buy-your-new-domain-on-aws.html)  
[https://brunch.co.kr/@topasvga/567](https://brunch.co.kr/@topasvga/567)  

### 샌드박스 모드에서 나가기

음... 이미 샌드박스 모드에서 나간 상태이기에 매뉴얼로 대체한다. 여담이라면, 예전에 샌드박스 모드에서 나가는 일이 굉장히 오래걸렸던 기억이 나는데, 왜냐하면 샌드박스 모드를 나가야 하는 이유를 파파고로 열심히 번역하여 영어로 적어서 내자 빈번히 퇴짜를 맞았던 반면, 마지막엔 자포자기하는 심정으로 그냥 한글로 적어서 냈더니 아주 빠르게 통과가 되었기 때문이다. 그러니 설명(description)을 쓰는 란에, 사용에 오해를 불러일으킬 영어를 작성하는 것보단, 그냥 한글로 적어서 내는게 더 도움이 되지 않을까... 추측해본다. 아마 영어로 적어서 냈을때도 무언가 오해가 있었기에 수락되지 않았던 것 같다.

매뉴얼 및 참고:  
[https://docs.aws.amazon.com/ko_kr/ses/latest/dg/request-production-access.html](https://docs.aws.amazon.com/ko_kr/ses/latest/dg/request-production-access.html)  
[https://coding8282.tistory.com/45](https://coding8282.tistory.com/45)  

### 자유롭게 이메일 사용하기

자유롭게 송수신을 하기 위해선 메일 서버를 따로 만들어야 한다. AWS에서 자체적으로 `Workmail`이라는 서비스를 제공하고 있는 것 같은데, 월간 구독료가 든다고 하여, `Lambda`와 `s3`를 이용하여 `SES`에 전송되는 메일을 네이버 메일함으로 전송하도록 설정해 볼 예정이다.

* SMTP (Simple Mail Transfer Protocol) : 이메일을 다른 컴퓨터로 전송할 때 사용하는 메일 서버의 기본 프로토콜이다. TCP 포트번호는 25번이고 DNS의 MX 레코드가 사용된다. 일반적으로 메일 서버 간 이메일을 주고받을 때 사용한다.

* POP3 (Post Office Protocol 3) : 유저(클라이언트)가 메일 서버에서 이메일을 받아오는 프로토콜 중 하나. 이메일 전체를 로컬 컴퓨터에 다운로드를 받는(옮기는) 방식이다.

* IMAP (Internet Message Access Protocol) : 유저가 메일 서버에서 이메일을 받아오는 프로토콜 중 하나. 중앙 서버에서 동기화가 이루어지기에 모든 장치에서 동일한 이메일 폴더를 확인할 수 있다.

참고:  
[https://m.blog.naver.com/yeopil-yoon/221286368883](https://m.blog.naver.com/yeopil-yoon/221286368883)

먼저, 메일을 수신하기 위해 MX 레코드를 만들어야 한다. `Route53`으로 가서 해당 도메인에 아래와 같이 MX 레코드를 생성해주자.

```txt
10 inbound-smtp.regionInboundUrl.amazonaws.com

// 만약 SES가 작동하는 도메인이 us-east-1에 있을 경우:
10 inbound-smtp.us-east-1.amazonaws.com
```

그리고 `s3`로 이동하여, 해당 도메인의 이메일을 담을 버킷을 새로 만들어주자. (혹시라도 접근에 문제가 있을까봐 퍼블릭으로 초기 설정을 해놓았는데, 나중에 굳이 퍼블릭이 아니더라도 괜찮으면 공개상태를 바꿔야겠다.) 여다른 설정 없이 버킷을 만들었다면, 해당 버킷으로 들어가서 `권한` 탭의 `버킷 정책`을 아래와 같이 편집한다.

```txt
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowSESPuts",
            "Effect": "Allow",
            "Principal": {
                "Service": "ses.amazonaws.com"
            },
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::[버킷 이름]/*",
            "Condition": {
                "StringEquals": {
                    "aws:Referer": "[사용자 ID]"
                }
            }
        }
    ]
}
// 대괄호를 포함해서 바꿔주어야 한다!
// 사용자 ID는 우측 상단 아이디를 클릭하면 확인할 수 있다.
```

이제는 `SES`로 다시 이동하여... (복잡하다 복잡해)

1) `Email receiving` 탭에서 `Create rule set`를 클릭한다.
2) rule set의 이름을 설정하고, `Create rule`을 클릭한다(현재 rule set 안으로 들어간 상태).
3) 마찬가지로 rule의 이름을 설정하고, 송수신하기 원하는 주소(`Add new recipient condition`)를 설정한다. (예를 들어, `recipient condition`을 hello@test.com으로 설졍하면, hello@test.com 외 다른 주소, 가령 goodbye@test.com과 같은 주소로 이메일이 송신되지 않는다. 옵션인 부분이여서 넘어가도 되지만 개인 판단으로는 필요한 과정이라고 생각한다. 만약 누군가 내 도메인을 이용해서 이메일을 무단으로 사용한다면?!)
4) `Add new action`에서 `Deliver to S3 bucket`을 클릭하고, 이전에 만들었던 버켓의 이름을 선택하고 끝낸다.
5) 해당 rule set 페이지로 들어가 `set as active`를 클릭하여 활성화한다.

모든 과정이 완료되었다면, 도메인에 이메일을 보낼 경우 `s3`에 이메일 객체가 쌓이는 것을 확인할 수 있을 것이다.

이제 마지막으로, `s3`에 저장된 이메일 객체를 읽어서 내보내는 `Lambda` 함수를 설정하면 된다.

우선, `SES`와 동일한 리전에 `Lambda` 함수를 생성해야 한다. `Lambda`로 이동하여 별다른 조건 없이 `Lambda` 함수를 파이썬으로 설정하고 생성하였다면, 이어서 `IAM`으로 이동하여 해당 함수에 권한을 부여해야 한다.

`IAM`에서 `역할` 탭에 들어간 후, `[생성한 람다 함수 이름]-role-[8자리 랜덤 글자]`(예: `my-lambda-role-abed3fgh`)를 클릭한다. 그리고 `권한 추가`를 클릭하여, `AmazonS3FullAccess`, `AmazonSESFullAccess`를 추가한다. 권한을 추가해주어야 해당 함수에서 `s3`와 `SES`에 접근을 할 수 있게 된다.

다시 `Lambda`로 돌아와서, 해당 함수 안으로 들어가 `트리거`를 클릭하고, `s3`를 선택한 다음, 이메일 객체가 저장되는 버킷을 클릭한다. 설정이 완료되었다면 이후 s3에 객체가 도달할 때마다, 자동으로 함수가 작동되어 함수 내부에 작성된 코드가 실행될 것이다.

결과적으로 지금까지의 워크플로우를 나타내보면...

```txt
사용자 -(이메일 발송)-> SES -> S3 -> Lambda -> 네이버 메일
                                     ^^^^^^
                                     진행중!
```

이제 정말 마지막으로, 함수 안에 코드를 작성하여 `s3`에서 읽어온 객체를 네이버 메일로 보내는 일만 남았다. 하지만 이메일 객체를 읽고 다시 전송하는 일은 조금 복잡하기에, 분량 상 이후에 따로 작성하여 연결할 예정이다. 이번 글에서는 AWS의 다양한 서비스를 이용하여, 개인 도메인을 이메일로 어떻게 사용할 수 있는지, 전체적인 서비스의 연결을 그리는 것을 목표로 하였다.

음... 후반 부분 설명이 사용 중심으로 돌아가서 개념적인 내용이 빈약한 것 같지만...(도대체 `s3`와 `Lambda`가 뭔데?!) 기회가 된다면 나중에 좀 더 보강을 해야겠다. `s3`와 `Lambda`의 시너지가 꽤 좋은 것 같아서, 이후에도 자주 사용하게 되는 서비스가 될 것 같다.

참고:  
[https://m.blog.naver.com/demonic3540/221635831475](https://m.blog.naver.com/demonic3540/221635831475)  
[https://brunch.co.kr/@topasvga/570](https://brunch.co.kr/@topasvga/570)  

[Back](/)
