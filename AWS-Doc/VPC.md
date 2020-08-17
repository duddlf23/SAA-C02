# [VPC](https://docs.aws.amazon.com/ko_kr/vpc/latest/userguide/what-is-amazon-vpc.html)

## 개념
Amazon EC2의 네트워킹 계층

- VPC - 가상 네트워크
- 서브넷 - VPC의 IP 주소 범위
- 라우팅 테이블 - 네트워크 트래픽을 전달할 위치 결정에 사용되는 라우팅의 집합
- 인터넷 게이트웨이 - VPC의 리소스와 인터넷 간의 통신을 활성화하기 위해 VPC에 연결하는 게이트웨이
- VPC 엔드포인트 - PrivateLink 구동 지원 AWS 서비스 및 VPC 엔드포인트 서비스에 VPC를 비공개로 연결할 수 있습니다. VPC의 인스턴스는 서비스의 리소스와 통신하는 데 퍼블릭 IP 주소를 필요로 하지 않습니다. VPC와 기타 서비스 간의 트래픽은 Amazon 네트워크를 벗어나지 않습니다.

## [작동 방식](https://docs.aws.amazon.com/ko_kr/vpc/latest/userguide/how-it-works.html)



## [퍼블릭 서브넷과 프라이빗 서브넷](https://docs.aws.amazon.com/ko_kr/vpc/latest/userguide/VPC_Scenario2.html)
퍼블릭 서브넷의 인스턴스는 인터넷에 바로 아웃바운드 트래픽을 전송할 수 있는 반면, 프라이빗 서브넷의 인스턴스는 그렇게 할 수 없습니다. 반면, 프라이빗 서브넷의 인스턴스는 퍼블릭 서브넷에 있는 NAT(Network Address Translation) 게이트웨이를 사용하여 인터넷에 액세스할 수 있습니다. 


  