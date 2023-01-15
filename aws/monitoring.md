# Monitoring Service

## CloudWatch

### Metrics(지표)
- 모니터링할 지표(CPU 사용률 등)
- namespace로 나눔
- 지표는 시간을 기반
- 외부로 Stream 가능
  - 거의 실시간 전송
  - Firehose, 타사 서비스 가능
  - 필터링가능
- 고해상도 사용자 지정 지표는 최소 해상도로 1second를 가질 수 있다.


### Logs
- 로그 그룹(하나의 어플)
  - 로그 스트림
  - 로그 만료일 정의 가능
- S3, Kinesis, lambda 등에 보낼 수 있음
- 필터 표현식 가능
  - 특정 IP, 'ERROR' 문구를 찾을 수 있음
- 지표를 만들 수 잇음
  - 경보와 연결가능
- 여러 계정, 여러 리전의 로그스를 Subscription Filter를 통해 합칠 수 있음

### Monitoring
- 지표가 1분마다 수거가능함(상세 모니터링 활성화)

S3로 보내기
- 최대 12시간 걸릴 수있음
- 실시간이 아님

### Agent
- ec2 등에서 이벤트를 받을려면 Agent가 필요

### EventBridge
- 이벤트의 Filter를 거쳐 다른 서비스 실행
- Default Event Bus -> AWS 서비스로부터 받음
- Partner Event Bus -> 타사 서비스로부터 받음
- Custom Event Vus -> 사용자 지정 이벤트로부터 받음

Schema Registry
- 스키마를 추론하는 능력이 있음 
- 이벤트 버스의 데이터가 어떻게 정형화 되는지 미리 알 수 있음
- 즉 API DOCS

Resource-based Policy
- 다른 계정의 이벤트를 수신하거나 거부하도록 설정가능

### CloudWatch Container Insights
- 컨테이너(ECS, EKS, Fargate)로부터 지표와 로그를 수집, 집계, 요약하는 서비스
- 컨테이너로부터 지표와 로그를 받을 수 있음
- EC2에서 도는 쿠베네티스의 컨테이너에서도 CloudWatch Agent를 이용해서 로그를 받을 수 있다.

### CloudWatch Lambda Insights
- Lambda의 정보를 얻을 수 있음(콜드 스타트 등 시스템 수준의 지표를 얻을 수 있음)
- Lambda Layer에 제공됨

### CloudWatch Contributor Insights
- 상위 N개의 Contributor의 정보를 얻을 수 있음
- VPC, DNS 로그 등
- 불량 호스트를 걸러낼 수 있음
- 좋은 사용자인지 나쁜 사용자인지를 확인할 수 있음
- 규칙을 생성할 수 있음

### CloudWatch Application Insights
- 사용중인 어플리케이션을 분석
- 서비스에 문제가 있다면 분석해서 보여줌(내부에서 SageMaker를 사용함)
- 트러블 슈팅하는 시간을 줄일 수 있음

<br /><hr><br />

# CloudTrail

- AWS 계정의 거버넌스, 감사 및 규정 준수를 돕는다
- 기본적으로 활성화되어 있음 서비스에서 발생한 **API 호출 기록**을 받아 볼수 있음
- SDK, CLI, console, IAM의 모든 이벤트를 받을 수 있다
- Logs나 S3에 옮길 수 있음
- 모든 리전, 혹은 하나의 리전으로 부터 적용받을 수 있음
- AWS 리소스를 삭제했을때 알림을 받을 수 있음

3종류의 이벤트
- 관리 이벤트
  - 리소스에서 수행되는 작업(설정 등
  - 리소스 읽기 이벤트, 쓰기 이벤트
- 데이터 이벤트
  - 기본적으로 로깅되지 않음(볼륨이 높기 때문에)
  - 객체 수준의 데이터 이벤트
  - S3 GetObject, Lambda Invoke
- CloudTrail Insights 이벤트
  - 비용이 지불됨
  - 부정확한 리소스 프로비저닝, 서비스 한도도달 등 관리활동
  - 특이한 패턴을 감지함


### 보존기간
- 90일간 저장 뒤 삭제됨
- 감사목적으로 장기보존을 하고 싶을때는 S3로 전송하고 Athena로 분석하기

### EventBridge와 통합
- DynamoDB 테이블 삭제할때마다 알림을 받고 싶을때
- CloudTrail에서 받아서 EventBridge로 보내고
- EventBridge에서 특이 규칙을 찾아 SNS에 경고 보내기

<br /><hr><br />

# AWS Config
- 리소스에 대한 감사와 규정 준수 여부를 기록해줌
- **리소스 구성과 구성의 시간에 따른 변화**를 알 수 있음
- 해결할 수 잇는것
  - 보안 그룹에 제한되지 않은 SSH 접근이 있는가
  - 버킷에 공용 액세스가 있는가
  - 시간이 지나며 변화환 ALB구성
- 변화가 생길때마다 SNS 받을 수 있음
- 리전별 서비스이며 중앙화를 위해 리전, 계정 간 데이터 통합 가능
- S3에 저장가능

### Config Rules
- AWS 관리형 룰이 있음(75가지 이상)
- 커스텀 룰가능(lambda 사용해야함)
- 새 구성이 생길때마다 뜸
- 규정준수를 위한 것 행동을 차단하는 것은 아님
- 결제해야하며 비싸질 수 있음

리소스
- 시간별 볼 수 있음
- 리소스 구성도 시가별로 볼 수 있음
- CloudTrail과 연결해 API 호출을 볼 수 있음

Remediations
- SSM 자동화 문서를 이용해 규정을 준수하지 않은 리소스를 수정할 수 있음
- 재시도도 가능(최대 5번)

Notifications
- EventBridge로 규정미준수 알림 보내기 가능
- SNS로 바로 보낼 수도 있음

<br /><hr><br />  

# CloudWatch vs CloudTrail vs Config

CloudWatch
- 성능(성능 및 트래픽) 모니터링 & 대시보드
- 이벤트와 알림을 받을 수 있음
- 로그 집계, 분석 도구 사용 가능

CloudTrail
- 계정내 리소스들의 API에 대한 모든 호출 기록
- 글로벌 서비스

Config
- 구성 변경 및 규정 준수 규칙에따라 리소스를 평가
- UI로 보여줌