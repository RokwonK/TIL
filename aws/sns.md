# SNS
Simple Notification Service
- 메시지 하나를 많은 수신자에게 보낸다고 할때
- 수신자 하나하나를 보내는 쪽과 직접 연결하기는 힘듬

Pub/Sub 구조로 중간에 SNS Topic(주제)를 놓고
생산자는 SNS로 publish, SNS를 subscribe을 하고 있는 구독자들에게 전송함
- 전송된 모든 메시지를 받는다(메시지 필터링 기능을 추가할 수 있음
- 주제별 1200만 구독자까지 가능함
- 계정당 10만개의 주제를 가질수 있음
- SNS Subscriber 서비스 : 직접 이메일 보내기, 직접 SMS/모바일 알림, HTTPS 엔드포인트로 직접 데이터보내기, SQS와 같은 서비스, Lambda, Firehose를 통해 S3나 Redshift
- SNS Publisher 서비스 : CloudWatch, ASG, CloudFormation(state change), BUudge, S3, DMS, Lambda ...


### SNS가 Publish 하는 방법
- Topic Publish
  - SDK를 이용해 publish
- Direct Publish(mobile app sdk)
  - Google GCM, Apple APNs, Amazone ADM

### Security
- HTTPS
- KMS키를 사용한 저장데이터 암호화
- 클라에서 암호화 및 복호화
- IAM
- SNS 액세스 정책