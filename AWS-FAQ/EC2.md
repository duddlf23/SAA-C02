# [EC2](https://aws.amazon.com/ko/ec2/faqs/)

Q: Amazon EC2로 할 수 있는 작업은 무엇입니까?

A: ... Amazon EC2는 실제 사용한 만큼만 요금을 지불하면 되므로, 컴퓨팅 비용이 절약됩니다. ...

Q: 이러한 한도와 관련된 인스턴스 사용량을 확인할 수 있습니까?

A: Amazon CloudWatch 메트릭을 통해 Service Quotas 콘솔에서 한도에 대한 EC2 사용량을 확인할 수 있습니다. 또한 Service Quotas을 통해 CloudWatch에서 경보를 구성하여 한도에 도달할 때 고객에게 경고할 수 있습니다. 또한 Trusted Advisor 및 Limit Monitor에서 인스턴스 사용량을 계속 추적하고 검사할 수 있습니다.

## EC2 SMTP 엔드포인트 정책 변경 사항

Q: 변경 사항은 무엇입니까?

A: 2020년 1월 27일부터, Amazon Elastic Compute Cloud(EC2)는 고객과 이메일 수신자를 스팸 및 이메일 남용 사례로부터 보호하기 위해 포트 25를 통한 이메일 트래픽을 기본적으로 제한하는 변경 사항을 적용하기 시작합니다. 포트 25는 일반적으로 SMTP 포트를 사용하여 이메일을 전송합니다. 과거에 요청을 통해 포트 25 조절 기능을 제거한 계정은 이 변경 사항의 영향을 받지 않습니다.

