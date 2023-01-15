# API Gateway

Client도 Lambda를 Invoke(호출)하고 싶다면 어떻게 해야할까?  
AWS가 바라는대로라면 Client가 IAM Policy를 가진 유저여야한다.  
하지만 그 수많은 Client에게 모두 권한을 줄 것인가?  
이러한 문제를 해결하기 위해 Client는 Lambda에 직접 접근하는것이 아닌 대리인을 통하여 접근하게 된다. 대리인 서비스 즉, Proxy의 역할을 해주는 서비스가 API Gateway이다.  
(당연 API Gateway에는 Lambda를 사용할 수 있는 IAM Role을 가지고 있어야한다.)  

<br />  

### API Gateway의 기능  
API Gateway는 단순 프록시의 기능만 하지 않는다.  
- 인증
- 사용량 계획 설정가능
- stage(운영, 개발 단계 라우팅 구분가능)

<br />  

### API Gateway의 장점  
- Lambda와 함께 완전 서버리스 애플리케이션 구축
  - 인프라를 관리할 필요가 전혀없음!(API Gateway의 설정 덕분)  
- WebSocket Protocol
  - 두 가지 방법의 실시간 스트리밍이 가능하다.
- Handle API versioning  
  - API 버저닝을 다룰 수 있다.
- Handle security  
  - 인증, 권한부여 등 수많은 보안 기능을 활성화시킬 수 있다
- API Key, handle request throttling  
  - API Gateway에 클라이언트 요청이 과도할 때 요청을 스로틀링할 수 있다.  
- Swagger, Open API import
  - 신속히 API를 정의하여 가져올 수 있고 내보낼 수 있음
- API Gateway 수준에서 유효성 검사
  - 요청, 응답을 변형, 유효성 검사, 호출 처리가능
- API 응답 캐시 가능  

<br />  

### API Gateway와 통합시키기
- Lambda integration
  - REST API
- HTTP integration
  - 백엔드의 HTTP 엔드포인트와 통합가능하다.(온프레미스나 클라우드 서버 등)
  - 속도제한 기능, 캐싱, 사용자 인증, API 키 등의 기능을 추가해서 처리해줄 수 있다.
- AWS Service
  - 어떤 AWS API라도 노출 가능
  - 예를 들어 Step Function workflow를 시작하거나 SQS 메세지를 보낼 수도 있다.
  - AWS 서비스에 인증, 속도 제한을 추가하기 위해 통합하여 사용  

<br />  

### API Gateway 배포방법(Endpoint Types)  
- Edge-Optimized(default)
  - 전 세계 누구나 API Gatetway에 액세스가능
  - 모든 요청이 Cloudfront Edge location을 통하므로 지연시간이 개선됨
- Regional
  - 모든 사용자는 API Gateway를 생성한 리전과 같은 리전에 있어야 한다.  
  - 하지만 자체 Cloudfront 배포를 생성할 수 있음(Edge-Optimized와 동일한 결과, 하지만 캐시 권한 등 더 많은 설정을 할 수 있음)
- Private
  - VPC 내부에서만 접근가능(ENI 등 이용)
  - API Gateway에 액세스를 정의할 때는 리소스 정책 사용

<br />  

### API Gateway의 보안  
- 유저 인증
  - IAM Role(내부 리소스에서 접근할때)
  - Cognito(모바일, 웹 어플의 외부 사용자에 대한 보안 조치)
  - Custom Authorizer(자체 로직 실행 즉, lambda)
- Custom Domain Name HTTPS(ACM과 통합)
  - Edge-Optimized를 사용할려면 ACM은 us-east-1에 있어야함
  - Route53에 CNAME이나 C레코드를 설정해 도메인 및 API Gateway를 가리키도록 해야함