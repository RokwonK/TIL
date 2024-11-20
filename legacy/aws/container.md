# Container

도커의 사용사례 - MSA

서버 위 -> 도커 에이전트 실행 -> 도커컨테이너 띄우기  
도커와 가상머신의 차이
- 가상화기술 이긴하지만 리소스를 호스트와 공유
- 덜 안전하지만 많은 컨테이너를 사용가능

## ECS
ECS Cluster에서 ECS Task를 실행하는 여러 Type들이 있다.

EC2 Launch Type
- ECS 클러스터에 등록하기 위해서 Instance들은 ECS Agent를 실행해야함
- 이후 ECS가 도커컨테이너를 각 EC2에 알맞게 띄어줌  


Fargate Launch Type
- 인프라를 프로비전이 하지 않아 관리할게 없음!
- 서버리스 컨테이너 서비스
- ECS 태스크를 정의하는 태스ㅌ크 정의만 생성 필요한 CPU, RAM에 따라 AWS가 대신 실행함
- 그냥 실행됨 확장할려면 Task 수만 늘리면됨


ECS IAM
- EC2 Instance Profile
  - EC2에 지정하는 Role
- ECS Task Role
  - 각 Task(컨테이너)마다

ECS - Load balancer Integrations
- ALB를 각 Task와 연결가능
- NLB는 높은 성능이 필요할때 사용

ECS - Data Volumes (EFS)  
- Task에 파일 시스템을 직접 마운트할 수 있다.
- S3는 ECS의 파일 시스템으로 마운트 불가 


ECS Service Auto Scaling  
- 태스크를 자동으로 늘리거나 줄일 수 있음
- CPU 사용률, 메모리 사용률, 타겟당 요청수에 따라
- Target Tracking(Cloudwatch Metric)
- Step Scaling
- Scheduled Scaling


EC2 Auto Scaling 방법
1. ASG
  - Cloudwatch Metric, Alarm을 이용한 확장
2. ECS Cluster Capacity Provider
  - ASG와 연결
  - 새 태스크를 실행할 용량이 부족하면 자동으로 ASG 확장
  - RAM이나 CPU가 부족하면 EC2 추가  


## EKS

Amazon Elastic Kubernetes Service  
- AWS에 관리형 쿠버네티스 클러스터를 실행할 수 있는 서비스  
- 컨테이너 어플리케이션의 자동 배포
- 두가지 실행모드
  1. EC2
  2. Fargate
- 쿠버네티스는 대부분의 클라우드에서 지원한다.
- 시나리오
  - 각 AZ에 EKS node 실행(ASG로 관리함)
  - 각 node는 EKS Pods 실행
  - ECS와 마찬가지로 LB와 연결

Node Types
1. Managed Node Groups
  - EC2를 생성하고 관리
  - 온디맨드와 스팟 인스턴스를 지원
2. Self-Managed Nodes
  - 사용자 많은 경우
3. Fargate 
  - 노드를 사용하지않고 컨테이너만 실행하면 됨

EKS Data Volumes
- Container Storage Interface(CSI)라는 규격 드라이버를 활용
  - EBS, EFS(with Fargate)
  - FSx for Lustre, FSx for NetApp ONTAP  

## App Runner
- 완전 관리형 서비스
- 간편 앱 배포 서비스
- 컨테이너 이미지나 소스코드와 환경 설정(auto scaling, vCPU, RAM) 후 배포하면 끝