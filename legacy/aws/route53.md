# Route53  

DNS
- 호스트 이름을 IP주소로 번역
- 계층적 구조가 있음
  - . (rott)
  - .com (TLD)
  - example.com (SLD)
  - api.example.com (Sub Domain)
- Domain Registar
  - Route 53, GoDaddy
- DNS Records
  - A, AAAA, CNAME, NS...
- Zone File
  - 모든 DNS 레코드를 가지고 있음
  - 호스트 이름을 Ip주소로 바꾸는 역할을 함
- Name Server
  - DNS 쿼리를 실제로 해결하는 서버
- Top Level Domain(TLD)
  - .com, .us 등
- Second Level Domain(SLD)
  - google.com, naver.com 등

### DNS 동작
1. 도메인 이름을 DNS 등록
2. 도메인에 접근(주소창에 example.com을 입력)
3. Local DNS Server에 물어봄(보통 회사나 ISP 제공자에 의해 할당되고 관리됨)
4. ICANN에 의해 관리되는 Root DNS Server에 물어봄
  - .com은 알아요! .com은 NS 레코드임. 공인 IP 어디로 가라고 알려줌
6. IANA(ICANN의 하위)에서 관리하는 TLD DNS Server에 물어봄
  - example.com 이라는 서버는 알아요! NS 레코드임. 공인 IP 어디로 가라고 알려줌
7. 여기서 우리가 등록한 도메인 관리서버(Route53 등)로 이동함 SLD DNS Server
  - 나 알고있어! A레코드 IP주소를 던저줌
8. Local DNS Server에 캐시  

<br />  

## Route 53
- 고가용성, 확장성을 갖춘, 완전히 관리되고 있는 DNS
- 레코드별 상태확인 가능
- SLA 가용성 제공
- 53은 전통적인 포트번호
- Routing Policy - route53 어떻게 쿼리에 응답하는지
- TTL(DNS Resolvers에 캐싱되는 시간)

### 레코드별 의미
A - 호스트네임과 IPv4 맵핑
AAAA - 호스트네임과 IPv6 맵핑
CNAME - 호스트 네임과 호스트네임을 매핑(subDomain만 가능)
NS - Name Server로 DNS 쿼리에 응답가능함

### Hosted Zone
네임서버와 서브도메인에 대한 레코드 정의 가능

퍼블릭 호스팅 존
- IP가 뭔지 알 수 있음

프라이빗 호스팅 존
- VPC만이 URL을 리졸브할 수 있음
- VPC 내부에서 사용하는 것


### AWS 리소스와 연결
hostname을 제공하는 AWS 리소스들(Load Balancer, CloudFront)에 Sub Domain을 연결하고자할때  
CNAME과 A레코드의 차이점

CNAME
- 호스트네임을 다른 호스트네임으로 연결
- 서브도메인일때만 가능

A
- AWS 한정이지만 호스트네임을 다른 호스트네임에 연결할 수 있다.
- 무료, 상태확인이 가능함
- DNS 네임스페이스의 상위 노드로 사용될 수 있다.(Zone Apex 즉, example.com에도 가능하다는 뜻)
- EC2의 DNS 이름에 대해서는 별칭 레코드를 설정할 수 없다.

<br />  

### 라우팅 정책
- DNS 쿼리를 돕는다
- Simple, Weighted, Failover, Latency based, Geolocation, Multi-Value Answer, Geoproximity

Simple
- 단일 리소스로 보내는 것
- 하나의 호스트네임에 여러 IP등록도 가능
- IP모두 응답하고 셋 중 하나로 라우팅
- 상태 확인 하지 않음

Weighted(가중치)
- %로 라우팅 ex) 70% 20% 10% 로 디라이렉트 시키기
- 상태 확인 가능
- 모두 0이면 동일한 모두 가중치를 가짐

Latency based(대기 시간)
- 가장 가까운 리소스로 리디렉션
- 유저와 리전간 걸리는 시간을 기준으로 함
- 상태 확인 가능

Failover(장애조치)
- 기본레코드와 이어져있음
- 장애가 난다면 Secondary로 보냄

Geolcation(지리적 위치)
- 사용자의 실제 위치를 기반
- 일치하는 위치가 없을때 가는 기본 레코드가 있어야함

Geoproximity(지리적 근접성)
- 사용자와 리소스의 지리적 위치를 기반 트래픽을 리소스로 라우팅함
- 편향값을 사용함
- 편향값을 이용해 지리적으로 경계를 나눔

Multi-Value(다중 값)
- 트래픽을 다중 리소스로 라우팅할 때 사용
- 상태 체크 가능
- 최대 8개
- 클라에서 이 중 한 개를 선택하는 것


### Health Check(상태 확인)
공용 리소스의 상태를 확인하는 방법(only public resources)
1. 공용 엔드 포인트
2. Cloudwatch 알림
3. 계산된 상태확인
  - 여러 상태 체크를 하나로 합치는 것
  - Route 53에서 여러 EC2를 하나씩 모니터링 함

private hosted zones
- cloudwatch 지표를 만들고 알람설정하기