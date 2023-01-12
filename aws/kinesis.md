# Kinesis  
- 실시간으로 데이터를 수집, 처리분석
- 지표, 
- 4개의 컴포넌트
Data Streams - 캡처 처리 저장
Firehose - 데이터 스트림을 저장
Analytics - SQL

### Kinesis Data Streams
- 샤드로 구성되어 있음. 갯수는 조절가능
- 샤드가 많을 수록 스트림에서의 처리량을 높일 수 있음
- 처리할 데이터는 Producer가 보냄(SDK, Agent, Client, Application)
- 레코드를보냄(Partition key, DataBloc(1MB) 로 구성)
- 각 샤드는 초당 1MB 또는 1000개의 메시지를 전송

- consumer 소비자(SDK, Lambda, Firehose, analytics)
- 레코드를보냄(Partition key, Sequence no, DataBloc(1MB) 로 구성)
- 소비 매커니즘은 두개
  - 공유 소비 - 모든 소비자에게 샤드당 2MB
  - 향상된 팬아웃 소비 - 각 소비자에게 샤드당 2MB

- 보존기간은 1~365일
- 재처리하거나 재생가능
- 저장된 데이터 삭제가 불가능(불변성)
- 같은 파티션 키를 갖는 메시지는 같은 샤드를 통해 전송

두가지 용량모드가 존재
- provisioned mode
  - 사용령울 예상가능할때
- On-demand mode
  - 사용량을 예상하기 어려울때


### Kinesis Data Firehose
목적지까지 전송되는 데이터를 저장
- 생산자를 통해서 Firehose로 옴
- Lambda를 통해서 데이터 수정 가능
- Batch로 한번에 데이터베이스 같은 곳으로 전송함
- 거의 실시간 서비스
- 목적지로 S3, Redshift, ElasticSearch
- 타사 목적지로 Datadog, MongoDB 등
- HTTP Endpoint로 가능
- 처리된 모든 데이터를 저장하고 싶다면 S3 backup bucket을 이요할 수 있음

특징
- 자동으로 확장됨
- 처리한 데이터만 비용청ㅇ구


### Data Streams vs Firehose
Data Streams
- 대규모로 데이터를 수집
- 샤드의 수에 따라 성능이 결정
- 데이터가 1~364일간 보존

Horse
- S3, Redshift, ElasticSearch 등으로 보내는 것을 목적
- 완전관리형
- 배치로 쓰임
- 데이터 저장소가 없음

### Kinesis Data Analytics
- SQL 앱을 위한것
- Data streams나 Firehose에서 데이터를 받음 
- SQL로 데이터 실시간 분석
- 새로운 Data streams나 Firehose로 보냄

- 비용은 처리한 만큼만 부과
- 실시관 쿼리(시계열 분석, 실시간 대시보드에 사용)

<br /><hr><br />  

# SQS vs SNS vs Kinesis  

SQS
- 메시지를 요청해서 데이터를 가져오는 방법
- 소비자 대기열에서 직접 삭제
- 작업자, 소비자의 수에 제한이 없음
- 관리형 서비스


SNS
- 게시/구독 모델
- 모두가 다 메시지를 받음
- 1250만 구독자 까지 가능
- 제대로 전달되지 않는다면 데이터를 잃을 수 있음


Kinesis
- 소비자가 pull받는 표준모드
- 팬아웃(소비자에게 푸시)

### Amazon MQ
- 관리형 서비스가 아닌 직접 큐 어플리케이션을 사용해야할때
- MQTT, AMQP, STOMP, WSS, Openwire 등의 개방향 프로토콜을 사용
- RabbitMQ, ActiveMQ 두 가지 기술을 위한 관리형 서비스
- 서버에서 실행하기 때문에 서버 문제 발생가능
- 