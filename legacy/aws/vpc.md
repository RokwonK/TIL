# VPC

## CIDR - IPv4
- 클래스 없는 도메인 간 라우팅
- IP주소를 할당하는 방법
- 0.0.0.0/0 = all IPs
- Base IP
- Subnet Mask
  - IP에서 변경가능한 IP
  - 

공용 IP와 사설 IP
- 공용 인터넷 주소 - 인터넷에서 유일
- 사설 IP는 10.0.0.0 ~ 10.255.255.255만 가능
- AWS에서 172.16.0.0 ~ 172.31.255.255(172.16.0.0/12)는 default VPC 영역
- 홈네트워크는 192.168.0.0 ~ 192.168.255.255(192.168.0.0/16)


## VPC
- 리전당 최대 5개
- CIDR은 /28 ~ /16
- 사설 IPv4만 가능


## 서브넷
- IPv4의 부분 주소
- 5개는 사용할수 없음
  - 10.0.0.0 은 Network address
  - 10.0.0.1 은 VPC router가 예약
  - 10.0.0.2 은 VPC router가 예약
  - 10.0.0.3 은 AWS가 미래가 사용될 것으로 예약
  - 10.0.0.255 은 브로드캐스트 주소지만 AWS는 이것을 지원하지는 않는다. 예약은 가능하지만 사용은 불가능
  - 따라서 만약 EC2에 29개 IP 어드레스가 필요하다면 (/27가 아닌 /26이어야함)


## Internet Gateway, 라우팅 테이블
- IGW는 인터넷 허용을 함
- 수평 확장, 가용성, 중복성이 높음
- VPC와 별개로 생성 1:1만 가능
- 자체만으로 인테넛으로 액세스 가능하지 않고 라우팅 테이블을 이용해야함


## Bastion 호스트
- 프라이빗에 접근하기 위한 VPC 내에 있는 Public 호스트(EC2)
- 프라이빗에서는 반드시 SSH 접근을 활성화해야함


## NAT 인스턴스
- 구형 솔루션
- 네트워크 주소 변환임
- 프라이빗이 인터넷에 연결하려고 할때
- 공용과 사설 서브넷을 연결하는 것
- EIP가 연결되어 있어야함

동작 방식
- Public Subnet의 SG에 EIP가 연결된 NAT Instance가 존재
- Private Subnet의 인스턴스가 NAT Instance를 통해 트래픽을 보냄
- 이때 보내는 IP가 NAT Instance IP임
- NAT Instance는 회신시 다시 Private에 보냄


## NAT Gateway
- 관리형, 높은 대역폭을 가지고 있음, 가용성이 높음
- 대역폭에 따라 비용청구
- 특정 AZ에 생성되고 EIP를 받음
- 같은 서브넷에서는 연결불가능 다른 서브넷에서만 연결가능
- Private Subnet -> NATGW -> IGW
- 보안 그룹 관리 필요 없음(라우팅 테이블과는 연결되어 있어야함)
- 단일 AZ

NAT 인스턴스와 비교
- 특정 AZ에서 가용성이 높음
- 고가용성을 위해 다른 AZ에 하나 더 만들어야함


## NACL 및 보안 그룹
서브넷 보안단계 Network ACL
- 인바운드 규칙이 존재
- NACL은 무상태이고 SG는 상태 유지
- 보안그룹으로 들어간것은 나갈 수 있음
- NACL은 들어올때 Inbound, 응답을 할때도 Outbound 규칙을 모두 적용함
- 서브넷 레벨의 방화벽
- 1 ~ 32766개의 규칙을 정할 수 있음(숫자가 낮으면 높은 우선순위) 100씩 띄워서 설정하는 것을 권장함

기본 NACL
- 인바운드, 아웃바운드 모든 요청을 허용하는 특수성을 가짐

임시 포트(Ephemeral Ports)
- 클라(요청하는 쪽)가 서버와 연결할때는 임시포트를 배정받음
- 임시포트를 통해 응답을 받아야 하기 때문
- NACL에서 규칙에 포트 범위를 잘 지정해줘야함(OS마다 가능 포트범위가 다르니 확인)

NACL VS SG
- 서브넷 수준 VS 인스턴스 수준
- 허용,거부 규칙지원 VS 허용 규칙 지원
- 무상태 VS 상태 유지
- 우선순위가 높은 평가로 비교(첫번째 비교로 결정) VS 모든 규칙 평가


## VPC Peering(피어링)
- AWS 네트워크를 통해 VPC끼리 소통하기 위함
- 체이닝 지원X(체이닝으로 모든 VPC끼리 통신 불가, 직접 연결해야함)
- 서브넷끼리 서로 소통할 수 있어야함
- 다른 계정, 다른 리전간에도 가능
- 보안그룹은 다른 계정의 같은 리전이면 참조가능


## VPC Endpoints
- VPC 외부의 AWS 서비스(DynamoDB, S3등)을 인터넷을 통하지 않고 연결하기 위한 것
- VPC 내부에 배포됨
- 중복, 수평확장이 가능
- IGW, NAT 없이 AWS 리소스에 연결 가능
- 문제 발생하면 VPC에서 DNS설정 해석이나 라우팅 테이블 확인

유형 2개
1. Inerface EndPoints PrivateLink 이용
  - ENI를 이용(엔트리 포인트)
  - Subnet에 포함됨
  - 청구 요금은 시간단위, GB단위
2. Gateway
  - Gateway 프로비저닝
  - 라우팅 테이블의 대상이 되어야함
  - S3, DynamoDB에만 지원됨
    - 
  - 무료이다


## VPC Flow Logs
- IP 트래픽을 포착하는 것
- VPC, Subnet, ENI 계층으로 로그확인 가능
  - ELB 등도 가능
- 트러블 슈팅에 용이
- 로그은 S3, CloudWatch Logs로 보냄
- NACL & SG - Accept, Reject 트래픽 확인가능




## Site-to-Site VPN, Virtual Private Gateway, Customer Gateway
AWS와 다른 VPN을 비공개 연결하기 위한 방법
Virtual Private Gateway, Customer Gateway가 필요함

Virtual Private Gateway
- VPN 연결을 위한 AWS 측 VPN 집선기
- VPC와 연결됨
- ASN 지정가능


Customer Gateway
- 소프트웨어 혹은 물리적 장치
- IP주소 필요

AWS VPN CloudHub
- 많은 Customer Gateway와 소통을 해야한다면?


## Direct connect (DX)
- 원격 네트워크로 부터 VPC로의 전용 프라이빗 연결을 제공  
- VPC에는 Virtual Private Gateway 필요
- AWS Direct Connect Location에 요청해 연결
- 큰 데이터 세트 처리때 빨라짐, 비용도 쌈
- 연결 상태 유지 가능
- IPv4, IPv6 모두 지원
- 수동연결로 설치가 오래 걸릴 수 있음
- S3, Glacier에 바로 연결 가능

여러 리전에 있는 VPC와 연결하고 싶다면?
- DX Gateway 사용해야함


Type
- Dedicated Connection
  - 물리적 이더넷을 제공받음
- Hosted Connections
  - 파트너를 통해 또다시 연결 요청
  - 정용 연결보다 유연
  - 선택한 로케이션
- 둘 모두 새연결을 하려면 한 달보다 길어질 수 있음

암호화 기능
- 암호화 기능은 없음
- 프라이빗이어서 보안을 유지가능
- IPsec으로 보안된 연결은 가능

복원력(Resiliency)
- High
  - 여러 DX를 두는게 좋음
- Maximum
  - 각 로케이션에 독립적인 연결을 두개씩 수립

Site-to-Site VPN으로 DX의 단점을 해소할 수 있음
- 인터넷 연결
- 비용 감소

## Transit(환승) Gateway
복잡한 AWS 인프라를 해겨하기 위함
- 여러 VPC(VPC끼리 Peering 할 필요 없음) 전이적 연결가능(모든 VPC끼리 소통가능)
- 여러 DX끼리 연결가능
- 네트워크 문제 해결에 좋음
- 계정간 공유를 위해 AWS RAM 사용
- 라우팅 테이블을 통해 액세스 제한 함
- DX Gateway와 VPN 연결과 함께 작동
- AWS에서 유일하게 IP 멀티캐스트를 지원하는 서비스
- Site-to-Site VPN ECMP(라우팅 전략, 대역폭 늘리기)  

## VPC Traffic Mirroring
VPC 트래픽을 수집을 위해 트래픽이 있는 소스 ENI를 정의할 것임. 이때 트래픽을 분석하려면 기능을 방해하지 않으면서 일부 정보를 얻고 싶을때 사용
- 필터 사용 가능
- 컨텐츠 검사, 위협 모니터링, 네트워킹 문제 해결 등에 사용

## VPC용 IPv6
IPv6는 16진수 8자리로 나타냄
- VPC는 듀얼 스택 모드 가능(IPv4, IPv6)
- VPC는 IPv6 사용 활성화 가능
- 보통문제가 난다면 IPv6 문제가 아니라 IPv4의 주소공간 문제이다

### Egress-only(송신전용) Internet Gateway
- IPv6 전용 IGW