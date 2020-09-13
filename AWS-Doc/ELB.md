# Configure health checks for your Classic Load Balancer
- Unhealthy Threshold: The number of consecutive failed health checks that must occur before declaring an EC2 instance unhealthy

- Healthy Threshold: The number of consecutive successful health checks that must occur before declaring an EC2 instance healthy.

# [Troubleshoot your Network Load Balancer
](https://docs.aws.amazon.com/ko_kr/elasticloadbalancing/latest/network/load-balancer-troubleshooting.html)

인스턴스가 피어링된 VPC에 속해 있음
로드 밸런서와 피어링된 VPC에 인스턴스가 있는 경우, 인스턴스를 인스턴스 ID가 아닌 IP 주소로 로드 밸런서에 등록해야 합니다.

# [What is a Network Load Balancer?](https://docs.aws.amazon.com/ko_kr/elasticloadbalancing/latest/network/introduction.html)

TCP 트래픽의 경우, 로드 밸런서는 프로토콜, 원본 IP 주소, 원본 포트, 대상 IP 주소, 대상 포트, TCP 시퀀스 번호에 따라 흐름 해시 알고리즘을 사용하여 대상을 선택합니다. 클라이언트로부터의 TCP 연결은 소스 포트와 시퀀스 번호가 서로 다르므로 다른 대상에 라우팅될 수 있습니다. 각 TCP 연결은 연결 수명 동안 하나의 대상에 라우팅됩니다.

UDP 트래픽의 경우, 로드 밸런서는 프로토콜, 원본 IP 주소, 원본 포트, 대상 IP 주소, 대상 포트에 따라 흐름 해시 알고리즘을 사용하여 대상을 선택합니다. UDP 흐름은 소스와 목적지가 동일하기 때문에 수명이 다할 때까지 일관되게 단일 대상으로 라우트됩니다. 서로 다른 UDP 흐름에는 서로 다른 소스 IP 주소와 포트가 있으므로 다른 대상으로 라우팅될 수 있습니다.

ELB도 내부적으로 Scale-Out 한다

각 AZ에 동일한 트래픽이 분배되므로 AZ마다 인스턴스 수 동일하게 해야 한다

Network Load Balancer는 클라이언트 측 소스 IP를 유지하므로 백엔드가 클라이언트의 IP 주소를 확인할 수 있습니다. 그런 다음 추가 처리를 위해 애플리케이션에서 이를 사용할 수 있습니다.

# [다중 TlS](https://aws.amazon.com/ko/blogs/korea/application-load-balancers-now-support-multiple-tls-certificates-with-smart-selection-using-sni/)
 애플리케이션 로드 밸런서(ALB)에서 서버 이름 표시(SNI)를 사용해 다중 TLS/SSL 인증서 지원을 시작합니다. 이제 단일 로드 밸런서 뒤에서 각각 자체 TLS 인증서를 갖는 다수의 TLS 보안 애플리케이션을 호스팅할 수 있습니다. SNI는 로드 밸런서에서 동일한 보안 리스너로 다수의 인증서를 바인딩하기만 하면 사용할 수 있습니다. 그러면 ALB가 각 클라이언트마다 최적의TLS 인증서를 자동으로 선택합니다. 
 