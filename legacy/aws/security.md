# AWS Security Service

### 전송중 암호화(SSL)
- 데이터가 전송되기 전 암호화
- 받아서 복호화

### 서버측 암호화
- 데이터 수신 후 암호화
- 데이터를 암호화한 상태로 저장하는 것
- 암호키와 해독키는 보통 KMS에 저장함(서버가 KMS랑 통신할 수 있어야함)

### 클라이언트 측 암호화
- 클라이언트가 데이터 암호화
- 다시 받아온 후에 데이터 복호화
- 서버에서는 데이터가 뭔지 모름

<br /><hr><br />

## KMS
- AWS에서 암호화 키를 관리할 수 있음
- IAM고 완전히 통합되어 있음
- CloudTrail을 통해 사용API를 확인할 수 있음
- KMS 호출시에도 돈이 듬

키 유형 2개
1. Symmetric(대칭키, AES-256 keys)
  - 단일 암호화 키만 있음
  - 키 자체를 사용할 순 없고 API호출로 사용가능
2. Asymmetirc(비대칭키, RSA & EC2 key pairs)
  - 데이터 암호화에 사용하는 퍼블릭 키와 데이터 복호화에 사용하는 프라이빗 키가 있음
  - 작업 할당 및 검증에 사용
  - KMS에서 퍼블릭 키는 다운가능

KMS 키 유형 3개
1. AWS Managed Key (관리형 키)
  - aws/rds, aws/ebs와 같이 이름이 저장됨
  - 무료이며 특정 서비스에 저장 데이터 암호화 가능
2. Customer Managed Keys (CMK, 고객관리형 키)
  - 키 하나에 매달 1달의 비용이 듬
3. Customer Managed imported
  - 자체 키 구성 요소를 KMS에 저장
  - 키 하나에 매달 1달의 비용이 듬

Automatic Key rotation
- 자동으로 1년마다 키가 교체됨
- 고객 관리형은 반드시 교체되도록 활성화 해야함(빈도는 부조건 1년)
- 수동으로 키 교체도 가능

다른 리전에 키복사
- 불가 AWS 자동으로 다른 key로 만들어버림

Key Policies
- 키 정책에 아무것도 없으면 누구도 액세스 할 수 없음
- Default
  - 사용자 지정 키 정책을 제공하지 않으면 생성
  - 기본적으로 계정의 모든 사람이 키에 액세스하도록 허용하는 것
- Custom
  - 사용자, 역할을 지정
  - 교차 계정 액세스 시 매우 유용
  - 다른 계정이 사용할 수 있도록 권한 부여
  - Copying Snapshots across account에 사용

### Multi-Region Keys
- 선택한 한 리전에 키를 가짐
- 다른 리전에 복사
- 키 구성 요소가 복제됨 키 ID가 동일
- 핵심 - 한 리전에서 암호화 다른리전에서 복호화
- 전역으로 사용못한다
- 복제된 키도 자체 정책을 가짐
- 전역 클라이언트 측 암호화에 사용함
- 데이터의 특정 필드를 보호할 수 있음
- DynamoDB, Aurora 멀티리전에 사용할 수 있음


### S3 복제
S3를 복제하면 암호화되지 않은 객체와 SSE-S3로 암호화된 객체가 기본으로 복제됨  
- SSE-C로 암호화하면 복제되지 않음
- SSE-KMS는 기본적으로 복제되지 않음. 옵션을 활성화해야함
  - 다중 리전 키를 사용할 수 있으나 


### 암호호된 AMI 공유
- AMI는 KMS로 암호화 되어 있음
- 시작권한(Launch Permission)으로 AMI를 실행
- 다른 계정으로 KMS를 공유하여 AMI 복호화 및 사용

<br /><hr><br />

## SSM Parameter Store
구성 및 암호를 위한 보안 스토리지
- KMS 서비스를 이용해 암호하도 가능
- 서버리스이며 확장성과 내구성이 있음
- 매개변수를 업데이트할때 구성과 암호의 버전을 추적할 수도 있음
- IAM으로 보안
- CloudFormation을 사용도 가능함


Hierarchy
- 매개변수가 계층 구조 아래로 쭉 내려감(디렉터리 구조와 같이)

매개변수 티어 2가지
1. Standard
  - 용량 4KM
  - 무료
2. Advanced
  - 용량 8KM
  - 정책 적용 가능
    - TTL을 매개변수에 할당 가능
    - 만료 시 EventBridge로 알림을 받을 수 있음
  - 월 비용 지불


<br /><hr><br />

## AWS Secrets Manager
- 암호를 저장하는 최신 기술
- SSM 과는 다른 서비스
- X일마다 강제로 암호를 바꿈
- 교체할 암호를 강제 생성 및 자동화 가능
- 새암호를 정의할 람다 함수가 필요
- 다른 서비스와 통합이 좋음
- 암호화 KMS는 이것을 통해
- RDS, Aurora의 접속 암호에 대한 내용이면 이것!
- **교체, 관리 및 데이터베이스와의 긴밀한 통합을 지원하는게 장점**
- 비용이 나감

Multi-Region Secrets
- 보조리전에 복제됨 - 고가용성


<br /><hr><br />

## AWS Certificate Manager
- TLS 배포 및 프로비저닝 서비스
- 퍼블릭(무료), 프라이빗 제공
- 여러 서비스와 통합가능함
- EC2에는 불가능!(퍼블릭 인증서일 경우 추출이 불가능)

퍼블릭 인증서
- 도메인 이름을 나열, FQDN, 와일드 카드도 가능
- DNS(자동 갱신 가능), 이메일 인증 가능
  - DNS는 CNAME 레코드를 통해 인증해야함 - Route53이 있으면 자동 통함됨
- 만료 60일 전에 자동 갱신해줌
- 외부에서 인증서를 가져올 수도 있음(자동 갱신은 불가)
  - 만료 45일 전부터 만료 이벤트를 EventBridge 서비스에 전송해줌(람다, SNS, SQS)
  - AWS Config를 이용해서 만료를 확인할 수 있음


With API Gateway
- 엣지 최적화 기능(CloudFront)
  - 지정 도메인 이름이라는 리소스를 구서해야함(TLS인증서가 CF와 통합)
  - CloudFront를 이용하므로 us-east-1에 ACM TLS 인증서가 있어야함
- Regional
  - API Gateway와 같은 리전에 있어야함
- Private - vpc endpoint(ENI)를 통해 VPC 내부에만 액세스할 수 있음
  
<br /><hr><br />

## AWS WAF
Web Application FireWall
- 계층 7(HTTP)에서 일어나는 취약점을 막아줌
- ALB, API Gateway, CF, AppSync GraphQL API, Cognito User Pool
- 웹 ACL을 정의해야함(IP주소 기반 제어)
- SQL 인젝션, XXS 등의 일반적인 공격 차단 가능, 용량 제약 걸기, 지역 일치 조건, 속도 기반 규칙을 설정해 IP당 요청 수 조절 가능
- Web ACL은 Region으로(Cloudfront는 글로벌)
- rule group(규칙 정리)

사용 사례
- 로드 밸런서와 함께 WAF사용할 경우

<br /><hr><br />

## AWS Shield
디도스 공격을 막는 방법
- DDoS(Distributed Denial of Service, 분산 서비스 거부 공격)
  - 인프라에 동시에 엄청난 양의 요청이 세계 곳곳의 컴퓨터에서 들어오는 것
  - 실제 사용자가 서비스를 사용하지 못하게 함
- Shield Standard
  - 모든 AWS 고객에게 무료로 활성화
  - SYN/UDP Floods나 반사공격 및 L3/L4 공격을 보호해줌
- Shield Advanced
  - 월 3000달러
  - 더 정교한 디도스 공격을 막아줌
  - 대응 팀이 항시 대기해줌
  - DDoS로 인한 요금 상승을 막아줌
  - 자동 WAF 규칙을 배포해줌(자동 L7 보호)
  - ALB, CLB, NLB, EIP, CloudFront, Global Accelerator에 적용가능

<br /><hr><br />

## AWS Firewall Manger
- AWS Organization의 모든 게정 규칙을 관리한다
- 보안 정책 설정 가능
  - WAF rules
  - AWS Shield Advanced
- VPC수준의 Network Firewall
- Route 53 Resolver, DNS Firewall
- 정책은 Region 수준에서 만들어짐

### WAF, Firewall, Shield
- 모두 포괄적인 계정보호 서비스
- WAF는 ACL 규칙을 정의 - 리소스별 보호를 구성하는데 적절
  - 여러 계정에서 사용하고 구성을 가속 및 자동화 할려면 Firewall로 관리해야함
- Shield는 DDos를 막아줌
  - WAF 규칙 자동 생성 등의 기능을 해줌
- Firewall은 모든 계정을 관리해줌

<br /><hr><br />

## AWS GuardDuty
- 계정을 보호하는 지능형 위협 탐지 서비스
- 30일 시험판 존재
- CloudTrail을 가지고 비정상 호출 무단 배포 탐지
- CloudTrail S3 Data Event로 확인
- VPC Flow Logs로 확인
- DNS Logs 확인
- Kubernetes Audit Logs 확인
- 암호화페 공격을 보호할 수 있음
  - 전용 탐지 기능이 있음
- CloudWatch Event 기능을 이용해 알림을 받을 수 있음

<br /><hr><br />

## AWS Inspector
- 인프라의 자동 보안 평가에 사용
- EC2
  - Sysyem Manager agent를 사용
  - 의도하지 않은 네트워크 접근성을 분석
  - OS를 분석해 알려진 취약성 확인
- ECR
- Security Hub로 기록
- 결과는 EventBridge로 전송
- EC2와 컨테이너 인프라에만 평가가능  

<br /><hr><br />

## AWS Macie
- 데이터 보안 데이터 프라이버시 서비스(S3), 머신러닝을 이용해 AWS의 민감한 정보를 검색 및 보호함
- 민감한 데이터를 경고
  - 개인식별 정도 PII 정보(S3에 저장)
  - Macie로 분석 후 EventBridge로 전송