# RDS, Aurora, Elastic Cache

Relational Database Service(SQL을 사용하는 DB를 관리해주는 서비스)  
- Postgres, MySQL, MariaDB, Oracle, Aurora, Microsoft SQL Server

### ec2에 DB를 배포하는 것과의 차이점
- 관리형 서비스임
- 지속적 백업 생성
- 성능을 모니터링 가능
- 읽기 전용 복제본 생성 가능
- 다중 AZ
- 수직, 수평 확장 가능
- EBS (gp2, io1)
- SSH 접근 불가능

스토리지 오토 스케일링이 있음(모든 DB에서 가능)

### 일기 전용 복제본, 다중 AZ
읽기 전용 복제본
- 복제본 5개까지 가능
- 동일 AZ, 다른 AZ, 다른 Region에도 가능
- 일관적인 비동기식 복제가 발생
- 복제본을 데이터베이스로 승격시킬 수도 있음
- 다른 AZ로 데이터가 이동할때 비용이 든다(다른 region일 경우에만!)

Multi AZ(재해 복구)
- Sync 복제
- 마스터가 바뀔 수 있음

### 단일 AZ -> Multi AZ
- 다운타임이 전혀 없음(즉 전환시 멈추지 않는다)
- 내부적으로 RES가 자동으로 스냅샷을 생성
- 대기중인 DB에 restore
- 이후 Sync 복제함


### RDS Custom
- Oracle과 Microsoft SQL Server는 기저 OS나 사용자 지정 기능에 액세스 가능
- SSH, SSM 등으로 기저 EC2에 접근 가능

<br /><hr><br />  

## Aurora
- AWS 고유의 기술
- Postgre, MYSQL과 호환
- 스토리지 자동 확장(10GB에서 시작 128TB까지 커짐)
- 읽기 복제본의 경우 15개까지 가능, 복제 속도도 빠름
- 장애 조치도 즉각적
- 3개의 AZ에 걸쳐서 6개의 사본을 저장함.(가용성이 높음) 하지만 마스터는 1개임
- 단일이 아닌 수백개의 볼륨을 사용함
- Cross Region Replication 지원


### Aurora DB Cluster
- 라이터 엔드포인터를 제공함(알아서 마스터로 연결
- 리더 엔드포인트를 제공함(자동으로 읽기 복제본과 연결, 복제본 오토 스케일링도 추가할 수 있음) 
- 마스터와 복제본은 storage volume을 공유함(여기서 읽어들이는 것)  


### Aurora Serverless
- 예측 불허한 워크로드에 좋음
- Client는 Proxy Fleet과 소통

### Aurora Multi-Master
- 모든 노드에서 쓰기 가능
- 하나가 잘못되면 다른 Master로 넘김

### Global Aurora
- 주요 리전 한개
- 복제본은 리전 5개까지
- 리전 당 16개의 복제본 생성가능
- 지연시간 및 재해복구에 탁웧마
- 리전에 걸쳐 데이터를 복제하는데 1초미만의 시간을 사용함  

### Aurora Machine Learning
다른 AWS 리소스와 안전하게 통합가능

<br /><hr><br />  

## Backups

### RDS Backups
- 자동 백업
  - 5분마다 트랜잭션 로그 백업
  - 시간마다 데이터베이스 백업
  - 1일에서 35일까지 보관 설정 가능
- 수동 스냅샷
  - 유저가 직접 백업하는 방법
  - 원하는 만큼 오랫동안 보관가능

Restore 방법
- 스냅샷 새로운 데이터베이스가 생성되는 것
- S3으로부터 복구

Cloning
- 프로덕션에 영향을 주지 않음

## RDS & Aurora Security
저장된 데이터를 암호활 수 있음.
데이터가 볼륨에 암호화된다는 뜻
- 클라이언트는 AWS TLS 루트 인증서를 이용해야함
- IAM 사용
- SG 사용

## RDS Proxy
연결풀을 형성하고 공유할 수 있음.
많은 서비스가 다 각각 RDS에 연결하는 것이 아닌 RDS Proxy를 통하여 RDS Proxy에서 연결풀을 관리함.  
- RDS가 연결이 많아질때 생기는 CPU, RAM 사용률을 최적화 가능하다.
- 완전 서버리스 다중 AZ기능, 문제 발생 시 대기 인스턴스로 실행(66%까지 장애 조치 시간을 아낄 수 있음)
- MYSQL, Postgres, MariaDB
- DB에 IAM 인증 강제 가능
- RDS에 퍼블릭 액세스 불가능  

<br /><hr><br />  

## ElastiCache
캐시 기술을 관리할 수 있는 서비스  
여러 어플리케이션 바로 뒤에 ElastiCache를 두면서 User Session Store로써 역할을 할 수 있음  

Redis  
- 자동 장애 조치 Multi AZ
- 읽기 전용 복제본은 스케일리에 사용됨
- 백업과 복구 기능이 있다.

Memcached
- 샤딩
- 가용성이 높지 않고 복제도 발생하지 않는다.
- 지속적인 캐시가 아니다
- 백업과 복구가 되지 않는다.
- 멀티스레드 구조이다.

Cache Security  
- IAM 인증을 지원하지 않는다.(API 수준 보안에만 사용됨)
- Redis AUTH
  - SSL 보안
- Memcached
  - SASL-based auth

Cache 데이터를 불러오는 패턴
1. Lazy Loading
  - 모든 읽기 데이터가 캐시
2. Write Through
  - DB에 쓸때 데이터 캐시
3. Session Store
  - TTL 기능이 있다.

ElastiCache 사용사례
1. 실시간 순위
  - Redis Sorted set을 사용함
  - 요소가 추가되면 실시간으로 순위가 정해진다
  - Redis 클러스터가 있다면 Client가 Redis에 직접 연결해 실시간 순위 정보를 받을 수 있다.