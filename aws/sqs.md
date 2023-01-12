# SQS

여러개의 서비스를 이용중이라면 이 서비스들은 서로 소통할 것이다.  
서비스들이 정보와 데이터를 공유한다는 것이다.  

1. Sync Communication
  - 서비스와 서비스가 연속적으로 연결됨
  - 연결된 서비스가 할일이 많다면 앞단의 서비스에 트래픽 많아지면 과부하발생 - decoupling 필요
  - SQS, SNS, Kinesis(실시간 스트리밍, 대용량 데이터)
2. Async/Event based
  - 서비스와 서비스사이에 Queue 존재, Queue(대기열)에 데이터를 넣고 다른서비스는 Queue에서 값을 꺼내 할 일을 처리한다

## SQS
- 대기열
- 대기열에 메시지를 보내는 주체를 producer (여러개 일수도 있음)
- 대기열의 메시지를 처리하는 대상을 consumer, 메시지 처리 후 삭제(여러개 일수도 있음)
- 생상자와 소비자 사이를 분리하는 버퍼 역할을 한다

SQS - Standard Queue
- 가장 오래된 서비스(10년이 넘음)
- 완전 관리형 서비스
- 무제한 처리량을 가짐
- 처리량에 제한이 없고 대기열에 수도 제한이 없음
- 메세지가 대기열에 보존되는 시간은 4~14일
- 지연시간이 짧음(게시 및 수신시 10밀리초)
- 메시지는 1~256KB
- 메시지를 중복으로 가질 수 있음

Producing Messages
- SDK를 이용하여 SQS에 메시지를 보냄(SendMessage)
- 삭제(메시지 처리) 될때까지 메시지 존재

Consuming Messages
- EC2, 온프레미스 서버, Lambda도 가능
- poll Message 한번에 최대 10개를 받을 수 있음
- DeleteMessage API를 이용하여 Queue에서 메세지를 삭제

Mulitiple EC2 Instances Consumers
- 여러 consumer들이 동시에 메세지를 처리할 수 있다.

CloudWatch Metric(지표) - Queue Length
- ApproximateNumberOfMessages라고 부름
- 큐의 길이가 일정 수준 이상이 되면 알람을 보냄(CloudWatch Alarm 설정)
- 알람으로 ASG EC2를 늘리든지 함  

SQS Security
- HTTPS API를 사용함(메시지를 보내고 생성함으로써 )
- KMS 키를 사용
- 클라이언트가 자체적으로 암호화 및 암호 해독을 수행해야함
- IAM
- SQS 액세스 정책(cross-account, 다른서비스가 SQS를 사용할때)


Message Visibility Timeout
- 소비자가 폴링 후에는 이 시간동안은 다른 소비자한테는 해당 메세지가 안보임
- 이 시간이 초과 경과되고 메시지가 삭제되지 않았따면 메시지는 대기열에 다시 넣는다
- ChangeMessageVisibility API를 이용해서 시간을 더 달라고 얘기할 수 있음
- 높은 값으로 하면 재처리에 시간이 오래걸릴 것이고 낮은 값이면 동시처리의 문제가 있을 수 있다.  

Long Polling
- 대겨열에 아무것도 없으면 없다고 응답하지 않고 조금 기다리게 됨
- 지연 시간을 줄이기 위해서, SQS로 보내는 API 호출 횟수를 줄이기 위해서
- 1~10초까지 선택할 수 있다.
- SQS 에서 폴링하는 아무 소비자로부터 롱 폴링 활성화
- WaitTimeSeconds를 지정해 소비자가 직접 롱폴링하도록 할수도 있음

### SQS FIFO Queue
- 대기열 내의 순서가 정렬됨 poll message에서도 순서대로 받음
- 처리량에는 제한이 있음 초당 300개, 묶음이라면 초당 3000개
- 중복 방지 설정도 있음(5분 이내에 같은 메세지가 오면 무시 가능)


### SQS with Auto Scaling Group
대기열의 크기에 따라 인스턴스 증가하기 위함.

이를 이용한 패턴
- 대대적인 세일 행사
- 분리나 급격히 증가한 로드 혹은 시간초과 등의 문제