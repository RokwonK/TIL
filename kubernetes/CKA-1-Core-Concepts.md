# Core Concepts

- Master Node - Cluster 제어 매니징
  1. ETCD 클러스터 : 운영정보에 대한 key value 스토어
  2. kube-scheduler : Worker Node에 컨테이너 적재 
  3. Controller-Manager : Node, Replication 등의 컨트롤러 존재
  4. kub-apiserver : 위 정보들을 통제할 수 있는 API 제공
- Worker Node - 하나의 물리 or 가상 머신
  1. Container Runtime : Docker, ContainerD, rkt
  2. Kubelet : Worker Node에 존재하며 kub-apiserver 부터 오는 요청을 받을 준비를 함.
  3. Kube-proxy : 클러스터 내 서비스와 소통을 가능하게 해줌


### 컨테이너 런타임과 쿠버네티스의 관계
- OCI(Open Container Initiative)는 컨테이너 이미지와 런타임에 대한 표준이다.
- CRI(Container Runtime Interfaces)는 쿠버네티스에서 컨테이너 런타임과 통신(Kubelet과 통신하기 위한)하기 위한 인터페이스
  - CRI는 쿠버네티스가 OCI 호환 런타임과 상호 작용할 수 있도록 하는 고수준 API를 제공

1. CRI 도입 전 쿠버네티스는 Docker만을 컨테이너 런타임으로 지원
2. CRI 도입 후 Docker와의 호환을 위한 임시적으로 DockerShim을 만들어 지원
  - Docker에는 컨테이너 런타임 뿐 아니라 여러가지 기능이 존재하는데 이를 지원하기 위해서 특별히 DockerShim을 사용
3. 1.24 버전 이후 쿠버네티스가 DockerShim 지원을 포기
4. Docker의 여러 기능(빌드, 스웜 등) 중 컨테이너 런타임인 ContainerD 만 독립적으로 사용하면 쿠버네티스와 호환가능
  - ContainerD는 OCI 사양을 준수. 따라서, 쿠버네티스 CRI와 호환됨


### ETCD
분산 key value Store.
- node, pod, config, secret, account role 정보를 가지고 있음.


### Kube-apiserver
kube cli 를 사용하면 kube-apiserver로 전달됨.
kube-apiserver 가 요청을 처리함
1. 유저 인증
2. 요청 검토
3. 데이터 검색
4. ETCD 업데이트
5. Scheduler 동작(Worker Node의 Kubelet에 명령)


### Kube-Controller-Manager
Status를 모니터링, 상황을 개선
- Node-Controller : Kube-apiserver 를 통해 Node들 헬스케어
- Replication-Controller : 지정 갯수만큼 잘 띄워져 있는지 확인
- 등등 존재


### Kube-Scheduler
Node에 띄운 Pod을 관리. 어느 Node에 Pod을 띄울지 관리.
1. Filter Nodes(가능한 Node를 찾음)


### Kubelet
Node(배)의 선장과 같은 역할
1. Worker Node의 Kubelet은 Node를 등록하는 용도
2. Create Pods
3. Monitor Node & Pods


### Kube-Proxy
Pod은 다른 Pod에 접근할 수 있다.(Pod Network)
- IP 매핑 및 포워딩 해줌


### Pods
쿠버네티스가 관리하는 가장 작은 오브젝트.
- YAML 파일로 정의


### Replication-Controller
Pod이 고장나거나 다운된다면? 유저가 접속을 못함
- 고가용성 제공

1. Replication Controller (Legacy)
2. Replica Sets
    - 어떤 Pod들을 모니터링 할 것인지 selector에 정의할 수 있음(Pod 설정에 정의된 Label로 필터링 가능)


### Deployments
어떻게 프로덕션 환경에 배포할 것인가(무중단 배포와 같은 것)
- Replica Set보다 하이레벨 개념.
- 중지, 변화, 재게 등


### Services
구성요소 그룹 간의 연결을 담당(미들웨어 느낌)
- Pod - Pod, User - Pod, External - Pod
- 다른 네트워크간 연결을 할 수 있도록 지원
1. Node Port (Node에 존재)
  - Target Port : Pod이 열어놓은 포트
  - (Service) Port : Target Port에 매핑되는 Service의 포트
  - Node Port : 외부에 열어놓은 포트
2. Cluster IP
  - 다양한 포드 그룹. 모든 포드가 가지는 IP는 고정이 아님.
  - 포드를 하나로 묶고 접속할 수 있는 하나의 인터페이스가 있으면 좋음. Cluster가 그 역할
3. Load Balancer
  - 단일 URL을 제공하고 연결된 포드 그룹에 로드 밸런싱


### NameSpaces
클러스터를 논리적으로 나눈 서브 클러스터
