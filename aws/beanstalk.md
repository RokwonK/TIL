# Beanstalk

인프라 관리, 코드 배포, 리소스 형성 등 할게 많음.  
이러한 것들을 모두 관리해주는게 Beanstalk  

- 개발자 중심의 서비스, 하나의 인터페이스로 EC2 ASG, ELB, RDS, 상태 모니터링 등 모두 다해줌. 개발자는 코드만 신경 쓰면됨!  
- 자체는 공짜이나 활용하는 리소스들의 비용을 지불함  


- Application Components의 묶음으로 되어 있음
- Application Version 존재 
- Environment
  - 두가지 티어가 있음 : Web Server(전통적인 아키텍쳐 모습 배포), Worker(SQS 대기열 등 배포)
  - 다수의 환경 만들기 가능(dev, test, prod)

사용 순서
App 만들기 -> Upload Version -> Lauch evironment -> Manage

내부적으로 CloudFormation을 사용하여 리소스들을 생성함