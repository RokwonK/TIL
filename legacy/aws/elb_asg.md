# 고가용성을 위한 Elastic Load Balancer ELB와 Auto Scaling Group

확정성
- 수직
  - 인스턴스 크기 확장
  - 분산되지 않는 데이터베이스 등에 쓰임
- 수평(탄력성)
  - 시스템 수를 늘리는 것
  - 분배시스템이 있다는 것
  - EC2같은 클라우드가 있어서 가능


고강용성
  - 두개 이상의 가용영역 시스템이 돌고 있는 것
  - 데이터 센테에서의 손실에서 살아남는 것이 목적


## ELB
ELB는 각 서버들로 전달하는 역할(로드 밸런싱 하는 역할)
- 부하 분산(단일 액세스 지점 노출)
- 클라측은 어디랑 연결되어 있는지 모름
- SSL 가능
- 개인, 공공 트래픽을 나눌 수 있음


여러 서비스와 통합 가능함 - EC2, ACM watch 53 waf 등


ec2로 health check(port, route로 연결)  
기록하고 가능하다면 트래픽을 분산시켜줌

### 종류
- 클래식(예전 버전, deprecated)
  - http, https, secure tcp 등
  - 예전 버전 권장하지 않음
- ALB
  - http, https, websocket
- NLB
  - TCP, UDP
- GWLB
  - IP Protocol

### LB Security Groups
- 유져는 전부 들어올 수 있도록 해야함
- EC2 보안그룹규칙에 로드밸런서 보안규칙과 연결해야함(LB가 접근가능하게)  

### Application Load Balancer(V2)
- Layer 7 - HTTP
- 타겟그룹으로 전송(target groups)
- HTTP/2, Websocket 지원
- 리다이렉트 지원
- 경로(paht)에 따라 라우팅 가능
- hostname에 따라 라우팅 가능(one.example.com & other.example.com)
- 쿼리와 헤더에 따라 라우팅 가능
- 도커와 ECS와 궁합이 좋음. 포트매핑 기능이 있어서
- target group으로 가능한 리소스
  - ec2, ecs, lambda, IP주소들(사설)
- 고정 호스트 이름이 부여됨
- ALB와 연결되 target group에서는 Client의 주소를 알지 못함
  - X-Forwared-For/Port/Proto 등의 헤더를 통해서 확인가능

<br />  

### Network Load Balancer
- Layer 4 - TCP, UDP
- ALB에 비해 지연 시간도 짧음 400 : 100
- 각 AZ에 생성하며 각각 고정 IP를 가지고 있다.
- 프리티어가 지원하지 않음
- target groups
  - ec2, ip(사설), alb(고정 IP를 얻을 수 있고, alb 덕분에 HTTP 유형 처리 가능)
- 헬스체크에 tcp, http, https를 지원함

<br />  

### Gateway Load Balancer
- Layer 3 - IP
- 모든 트래픽이을 검사 - 침임방지, 방화벽에 쓰임(검사 후 트래픽 드롭 가능)
- GENEVE 프로토콜 6081포트사용
- 투명 네트워크 게이트, 로드밸런서 기능
- 유저의 요청이 VPC로 들어오고 route table을 통해 GWLB로 들어감
- target groups
  - ec2, ip(사설), 타사의 클라우드  

<br />  

### Sticky Sessions
고정 세선, 세션 밀집성  
동일한 클라에서의 요청을 동일한 ec2로 트래픽을 넘겨주는것
- ALB에서 설정 가능
- 그 원리는 쿠키
- 고정성 만료기간이 있음. 이후 다른 EC2로 갈 수 있음
- 분산 불균형이 일어날 수 있음

> 쿠키 2가지 종류
Application-base Cookies
- 커스텀 쿠키 - AWSALB 등의 이름을 사용하면 안됨
- 어플리케이션 쿠키 - 로드밸런서에 의해 생성됨 `AWSALBAPP`
Duration-based cookie
- 로드밸런서에 의해 생성, `AWSALB`, `AWSELB라는` 이름을 가짐

타겟그룹에서 stickiness 세션에서 설정가능  

<br />  

### Cross-Zone Load Balancing
- 교차 영역 로드밸런싱  
- 전체 가용영역에 분산된 모든 ec2에 트래픽을 분산함.(가용 영역별 ec2의 갯수가 다를때 사용하기 좋음)  
- NLB의 경우 돈내야함(가용 영역간 전송값)
- ALB는 기본으로 켜져있음  

<br />

### SLS/TLS
- 클라와 로드밸런서 사이의 데이터 암호화(발신자와 수신자만 해독가능)  
- SSL은 보안소켓 계층 - 연결을 암호화하는데 사용
- TLS는 SSL의 최신 버전으로 전송 계층 보안  


로드 밸런서의 SSL 증명  
- X.509 certificate라고 불리는 TLS 서버인증서를 사용함
- ACM을 통해 관리할 수 있음
- HTTPS 리스너를 설정할때 기본 인증서
- 클라이언트는 SNI(Server Name Indicattion)를 이용해 도달하는 호스트 네임을 지정할 수 있다.  


SNI란  
- 단일 웹 서버(여러 웹 사이트를 지원하는)가 여러개의 SSL 증명서를 로드하는 문제를 해결하기 위해 존재
- 최신 프로토콜 클라가 SSL handshake시 대상 서버의 호스트 이름을 명시해야 한다.  


SSL 지원  
- CLB는 한개만 지원
- ALB는 다수 가능, SNI 사용함
- NLB는 다수 가능, SNI 사용함  

<br />  

### Connection Draining
CLB - 연결 드레이닝  
ALB, NLB - Deregistration Delay  

인스턴스가 등록 취소, 비정상적인 상태에 있을때 어느정도 시간을 주어 활성요청을 완료할 수 있도록 해주는 것.
(ALB는 해당 인스턴스로 요청을 보내지 않음)

기본 300 이며 0이면 드레이닝이 일어나지 않음

<br /><hr><br />  

## Auto Scaling Group
방문자가 많아지면? 서버를 빠르게 생성하고 종료할 수 있음(자동적으로)  
- 증가함에 따라 스케일 아웃을 해줌.
- 최소, 희망, 최대 갯수를 정할 수 있다.
- 자동으로 만들어진(스케일 아웃 시) 인스턴스도 로드밸런서와 연결이 된다.
- 인스턴스가 비정상이면 종료 후 다시 만듬
- Lauch Template을 작성함(스케일 아웃시 적힌 구성대로 EC2 만듬. AMI, type, EBS 볼륨, SG, Subnet 등)
- 스케일링 Policy도 존재
- CloudWatch 기반으로 스케일 아웃 조절 가능

<br />  

### Scaling Policies
동적 스케일링 정책  
1. Target Traking Scaling
  - 쉬운 설정
  - 평균 CPI 사용률을 40%를 유지하게 함
2. Simple/ Step Scaling
  - CloudWatch 알림이 발생했을때
3. Scheduled Actions
  - 예상 패턴으로 스케일링

예측 스케일링 정책(Predictive Scaling)  
1. 과거 로드를 분석하고 스케일링 예약(AWS가 머신러닝으로 알아서 처리해줌)  

대상 추적 정책  
1. EC2 인스턴스로의 평균 연결의 수를 유지  


<br />  

### 언제 스케일 조작을 할까
- CPU 사용률 확인
- 대상별 요청 수(테스트를 기반)
- 평균 Network I/O
- CloudWatch로 확인


### Scaling Cooldowns(냉각 기간)
- 스케일리 조치 후 냉각 기간을 갖음(이를 통해 지표들을 안정화시킬 수 있음)
- 냉각 기간 동안 EC2 실행/종료가 일어나지 않음
- 냉각기간(기본 300초)



### 종료정책
1. AZ에 걸친 밸런싱 시도
2. 실행 구성이 
