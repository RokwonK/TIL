# Database

- RDBMS : RDS, Aurora 
- NOSQL : Dynamo DB, ElastiCache, Neptune, DocumentDB, Keyspaces
- Object Store: S3, Glacier
- Data Warehouse(= SQL Analytics, BI) : Redshift, Athena, EMR  
- Search : OpenSearch(JSON)
- Graphs : Neptune
- Ledger : Quantum Ledger Database
- Time series

### RDS
- 인스턴스 크기, EBS 볼륨유형 사이즈를 프로비저닝해야함(스토리지 계층에 오토 스케일링 기능이 있어도 해야함)
- 읽기 전용 복제본 지원
- 고가용성 목적으로 다중 AZ에 둘 수 있음
- 보안은 IAM, SG, KMS, SSL
- 자동 백업 옵션 (최대 35일)
- 장기 보존 백업이 필요한 경우 수동 DB 스냅샷 사용
- 유지 관리 기능을 예약
- RDBMS와 온라인 트랜잭션 처리 데이터베이스(OLTP)에 사용  

### Aurora
- PostgreSQL, MySQL과 호환됨
- 컴퓨팅과 스토리지가 분리된 특별한 서비스
- 백그라운드에서 자가복구처리
- 오토 스케일링이 내장되어 있음
- 클러스터가 있어 라이터 포인트와 리드 포인트가 존재
- RDS와 동일한 보안
- 백업 및 옵션에 대해서 알아야한다.
- Serverless 기능 - 예측불가능할때
- Multi-Master 기능 - 쓰기 고가용성
- Global - 리전마다 최대 16개 읽기 전용 데이터베이스 가능(복제 하는데 1초 이하로 걸림)
- Machine Learning - SageMager 등등으로 머신러닝 수행 가능
- Cloning - 스테이징 기능(스냅샷보다 빠름)

### ElastiCache
- 관리형 Redis.Memcached
- 인 메모리임 1밀리초 미만의 지연시간
- 캐시를 위해 EC2 프로비저해야함
- Multi AZ 가능
- IAM, SG, KMS, Redis Auth 지원
- 백업 및 스냅샷 지정시간복구 기능 존재
- 관리, 예약 유지 관리가 가능
- RDS와 소통할려면 코드를 수정해야함
- 세션데이터, 키밸류 저장소
- SQL 사용 불가

### DynamoDB
- 밀리초 수준의 지연시간
- 프로비저닝된 용량모드, 온디맨드 용량모드(수요 급증시) 존재
- 세션 데이터를 저장하는 좋은 방법
- 가용성이 높음, 다중 가용영역에 걸쳐 있기때문에
- 쓰기와 읽기가 완전히 분리되어있음
- DAX(호환성 읽기, 읽기 지연시간이 백만분의 1초)
- IAM으로 보안
- 이벤츠 처리 용량 Streams로 변경을 확인가능 + Lambda와 통합가능
- Kinesis와도 연결가능
- 자동 백업(35일까지 가능), 온디맨드 백업
- 작은 서버리스 앱개발, 서버리스 캐시 분산시 사용
- NoSQL이니까

### S3
- 큰 객체를 저장할때 좋음
- 최대 크기는 5TB
- 스토리지 계층도 다양함
- 버저닝, 암호화, 복제, MFA 삭제, 액세스 로그 기능이 있음
- IAM, 버킷 정책, ACL, 액세스 포인트, Object Lambda(전송 전 변환), Object/Vault Lock
- SSE 암호화 설정 가능
- S3 버킷에 있는 모든 파일을 한 번에 작어 - Batch operations
- 파일 목록은 S3 인벤토리 사용
- 파일을 병력식 업로드가능한 multipart, S3 Acceleration, S3 Select
- 정적파일, 웹사이트 호스팅 가능

### Neptune
- 완전 관리형 그래프 데이터베이스
  - 관계성 높은 데이터에 사용
  - 소셜 네트워크, 좋아요, 댓글, 게시글들 전부 연결되어 있는 부분에서 사용하기 좋음
- 3 AZ에 걸쳐 가용성이 높음, S3 backup
- RDS와 동작방식이 비슷함
- 그래프 데이터 저장시 성능이 높음
- 그래프 = Neptune

### Keyspaces(for Apache Cassandra)
- 관리형 Apache Cassandra(NoSQL 분산 데이터베이스) 보조
- 서버리스 서비스, 확정성, 가용성이 높음
- 트래픽에 따라 오토 스케일링
- 여러 AZ에 걸쳐 세 번 복제됨
- 쿠리 수행시 CQL 사용해야함
- 지연시간이 짧음
- 두가지 용량모드 - 온디맨드 모드(크기가 자동 조정), 프로비전 모드
- 최대 35일까지 지정 시간 복구가 가능
- IoT 장치 정보, 시계열 데이터 저장 등
- Apache Cassandra = Keyspaces

### TimeStream
- 관리형 시계열 데이터베이스(시간 정보를 포함하는 포인트의 모음)
- 용량을 자동확장 축소
- SQL과 완벽 호환
- 과거 데이터는 비용 효율적이 스토리지 계층에 저장
- IoT 앱, 실시간 분석 등에 사용
- QuickSight, SageMaker, Grafana JDBC connection 등에 연결가능함

# Data & Analytics

### Athena
- S3에 저장된 데이터 분석
- SQL씀(Presto 엔진에 빌드됨)
- S3에서 바로 분석함!
- CSV, JSON 등 다양한 형식 제공
- 스캔된 데이터의 TB당 고정가격임
- QuickSight와 같이 사용(대시보드)
- 임시쿼리수행, 분석 및 보고, 모든 로그를 쿼리하고 분석가능
- 서버리스 SQL 엔진을 S3 데이터 분석이라믄 Athena

퍼포먼스 향상
- 열 기반 데이터에 사용(Apache Parquet, ORC)
  - AWS Glue를 사용해서 형식 변환가능
- 특정 열을 계속 검색하면 파티션 나누기
- 큰 파일을 사용해서 오버헤드를 최소화

기능
- Federated Query(연합 쿼리)
  - S3뿐만 아니라 다른 곳도 가능
  - 데이터 원본 커넥터(+ Lambda)를 사용 


### Redshift
- 분석 엔진, OLTP에 사용되지는 않음
- 분석과 데이터 웨어하우징에 사요 PB 용량까지 확장됨
- 성능향상 가능(병렬 쿼리 엔진임)
- SQL 사용 가능
- QuickSight와 통합 가능
- Athena와 비교
  - 데이터를 직접 로드 해야함
  - 로드후에는 빠름
  - 조인과 통합을 빠르게 할 수 있음(인덱스 존재)

Redshfit Cluster
- 리더 노드
  - 쿼리를 계획하고 결과를 집계
- 컴퓨팅 노드 
  - 쿼리 실행 및 결과를 리더노드에 보냄
- 예약 인스턴스 사용 가능

스냅샷과 재해복구
- 클러스터가 한개의 가용영역에 있음. 즉 스냅샷사용해야함
- S3 내부에 저장되고 증분됨
- 스냅샤에는 두가지 모드
  - 수동 (직접 삭제전까지 스냅샷 유지)
  - 자동화(예약)
- 스냅샷 자동 복제로 다른 리전에 생성하기

데이터를 주입하는 방법
- Kinesis Firehose
  - S3 - Firehose - Cluster
- S3
  - 인터넷으로 데이터 다운
  - 가상 프라이빗 클라우드에 비공개 상태로 유지하고 싶다면 Enhanced VPC Routing 사용
- EC2 JDBC driver

Redshift Spectrum
- S3에 있는 데이터를 로드하지는 않음
- 쿼리를 시작할 클러스터가 필요
- From S3로 쿼리 -> Spectrum이 S3를 일고 컴퓨트 노드에 전달
- 클러스터로 데이터를 보내지 않아서 성능 향상됨

### OpenSearch
- ElasticSearch의 후속 서비스
- 부분적으로 일치하는 필드를 포함해 모든 필드를 검색가능
- 쿼리를 분석할 수도 있음
- 사용하기 위해서 인스턴스의 클러스터를 생성해야함
- 자체 쿼리언어를 사용함
- Cognito, IAM을 이용해서 보안
- OpenSearch 데이터를 시각화 가능
- 패턴
  - DynamoDB - DynamoDB Stream - Lambda - OpenSearch
  - logs - filter - lambda - opensearch


### EMR
EMR은 Elastic MapReduce의 약어  
- 빅 데이터 작업을 위한 하둡 클러스터 생성에 사용
- 방대한 양 분석에 사용
- 전체 클러스터를 자동을 확장가능
- 스팟 인스턴스와 통합
- 기계학습, 웹 인덱싱 빅 데이터 작업이 있음

Node types * 모드
- 마스터 노드 - 클러스터 조절
- 코어 노드 - 테스크 실행, 데이터 저장
- 태스크 노드 - 테스크만 실행
- 온디맨드, Reserved(최소 1년), 스팟

EMR에 배포할때는 장기 실행 크러스터에서 예약 인스턴스를 사용하거나 임시 클러스ㅌ를 사용해 특정 작업실행

### QuickSight
관리형 비즈니스  인텔리전스 - 대화형 대시보드(데이터소스와 연결)
- 오토스케일가능
- 분석, 시각화, 특정목적 분석수행가능
- RDS, Aurora, Athena 등과 연결 가능
- SPICE를 사용
- 훌륭한 사용자 수준의 기능을 제공

Standard Version - Define User
enterprise Version - Groups  

### AWS Glue
- 추출과 변환 로드 서비스(ETL 서비스)  
- 완전 서버리스 서비스
- 예시로 Athena를 위한 Parquet format으로 바꾸기

Glue Data Catalog
- Glue Data Crawler 이용해 호환 데이터베이스(S3, RDS, Dynamo DB, JDBC)에 연결

중요한 기능
- Glue Job Bookmarks - 이전 데이터의 재처리를 방지
- Glue Elastic Views - SQL을 사용해 여러 데이터 스토어의 데이터를 결합하고 복제
- Glue DataBrew - 사전 빌드된 변환을 사용해 데이터를 정리하고 정규화함
- Glue Studio - ETL 작업을 생성, 실행 및 모니터링하는 GUI
- Glue Streaming ETL - 스트리밍 작업으로 실행할 수 있음

### AWS Lake Formation
- 데이터 레이크 생성을 돕는다
  - 레이크 : 데이터 분석을 위해 데이터를 모아주는 중앙 집중식 저장소
  - 수개월 작업을 며칠만에 만들 수 있음
  - 수집, 정제, ML 기능을 도움
- 블루프린트 제공
- 행, 열 수준의 세분화된 액세스 제어를 할 수 있음
- Glue 위에서 동작하지만 Glue와 상호작용을 하지는 않음

시험
- 중앙화된 권한, 한 곳에서 보안설정 가능


### Amazon Managed Streaming for Apache Kafka (Amazon MSK)
- Kinesis의 대체제 두 서비스 모두 데이터 스트리밍
- EBS 볼륨에 데이터 자장 가능
- MSK Serverless 사용 가능

