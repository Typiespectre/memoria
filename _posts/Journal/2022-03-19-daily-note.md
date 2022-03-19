# Saturday, March 19, 2022

[[Journal]]

VPC(Virtual Private Cloud): 가상 사설망
AWS에서 제공되는 대부분의 서비스가 VPC에 의존적이다. VPC를 사용하면 AWS 클라우드에서 논리적으로 격리된 공간을 프로비저닝하여 고객이 정의하는 가상 네트워크에서 AWS 리소스를 사용할 수 있다.
    - 프로비저닝(provisioning): 사용자의 요구에 맞게 시스템 자원을 할당, 배치, 배포해 두었다가 필요시 시스템을 즉시 사용할 수 있는 상태로 미리 준비해 두는 것을 말한다.

실제 아마존 웹 서비스는 각 나라마다 영역을 나누고, 나눠진 영역에서 여러 고객들이 "공용 환경"을 사용한다. 여기서 하나의 계정에서 각 AWS의 서비스들만의 격리된 네트워크를 만들어 주는 기능이 VPC이다. VPC를 사용하면 특정 사용자의 리소스들이 논리적으로 격리된 네트워크에서 생성되기 떄문에 다른 사람들이 접근하는 것이 불가능하다.

https://any-ting.tistory.com/64

S3, Cloudwatch, DynamoDB ... 는 VPC 내부에 탑재하여 사용하는 서비스들이 아닌, 공인 IP를 가지고 외부에서 접근하는 서비스들이다.

https://aws-hyoh.tistory.com/category/Amazon%20Web%20Service%20Network%20%EC%89%BD%EA%B2%8C%20%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0

모놀리식 아키텍처
https://aws-hyoh.tistory.com/145