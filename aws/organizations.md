# AWS Organizations

- 글로벌 서비스
- 여러 계정을 관리할 수 있음
- 멤버 계정은 한 조직에 소속
- 모든 계정의 비용을 통합 결제가능
- 모든 계정에 대해 집계된 사용량에 대해 할인을 받을 수 있음
- 계정간 할인이 공유됨
- API 존재하여 계정을 쉽게 생성 가능

1. Root Organizational Unit(OU)
2. 이 안에 관리 계정 및 다수의 OU가 있음

장점
- 단일 계정에 비해 보안이 뛰어남
- 모든 계정에 대해 CloudTrail을 연결가능
- 보안측 서비스 제어정책(SCP) 사용가능
  - 모든 대상에 적용, 관리 계정에는 적용되지 않음
  - 구체적인 허용 항목을 써야 작동함
  - 차단목록과 허용전략이 있음

# IAM Conditions
- IAM 내부의 정책을 어디에 적용할 것인지
- 정책내부에 Condition이라고 존재
- aws:SourceIP, aws:RequestedRegion 등 존재

### IAM Roles vs Resource Based Policies
- Roles을 맡으면 기존의 권한을 모두 포기하고 해당 역할에 할당된 권한을 상속하게 됨
- Resource Based Policies는 기존의 권한을 포기하지 않는다. 리소스가 처리하기 때문에

EventBridge를 쓸때 차이가 많이 남


### IAM Permission Boundary
- user와 role에만 지원되는 기능 group에는 지원되지 않는다
- IAM 개체의 최대권한을 정의하는 것
- IAM Policy와 겹치지 않게 권한을 준다면
  - 아무 권한도 생기지 않음
  - 1차로 IAM Policy를 확인
  - 2차로 IAM Permission Boundary를 확인
- AWS Organizations SCP와도 같이사용 가능

 
# AWS Cognito(인식)
- 웹, 모바일 앱(외부 사용자)에게 자격 증명을 부여함
- Cognito User Pools사용자풀
  - 가입
  - 로드 밸런서와 통합됨
- Cognito Identity Pools
  - 일부 AWS 리소스에 직접 연결가능하게 해줌

### Cognito User Pools
- 간단한 로그인 절차 정의 가능
- 멀티팩터 인증도 가능
- 소셜 로그인도 가능
- LB와도 통합가능

### Cognito Identity Pools
- Cognito User Pools, 소셜로그인과 통합해서 사용가능
- 임시 자격증명으로 AWS 리소스에 접근 가능
- 자격 증명 풀 서비스에 사용가능한 서비스 정의

# AWS Directory


### Microsoft Active Directory(AD)
- AD 도메인 서비스를 사용하는 Windows 서버에 사용되는 서비스
- 중앙 집중신 관리임(하나의 루트가 여러 친구들을 관리함)
- 모든 객체는 트리로 구성됨

### AWS Directory
Windows를 실행하는 EC2 인스턴스와 디렉터리를 가까이 위치시키기 위해서 사용
3가지 유형이 존재
1. AWS Managed Microsoft AD
  - 로컬에서 관리가능
  - 멀티팩터 인증을 지원
  - 독립 실행용, 온프레미스 AD와 함께 사용가능(둘 모두에서 관리가능)
2. AD Connector
  - 멀티팩터 지원
  - 첫번째 유형과는 다르게 온프레미스 AD의 프록시 역할만 함
3. Simple AD
  - Microsoft 디렉터리를 사용하지 않는다.
  - 온프레미스 AD가 없을 때 사용
  - 액티브 디렉터리가 필요한 경우 사용