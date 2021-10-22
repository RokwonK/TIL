# pm2 모듈

p(process) m(manager) : 프로세서(cpu)를 관래해주는 역할!  
간단하게 서버를 원활하게 운영하기 위해서 사용하는 모듈이다.  

## 역할
1. 서버가 에러로 인해서 종료되었을때, 서버를 다시 켜준다.
2. 멀티 프로세싱을 지원한다. 명령어를 통해 가동할 코어의 수를 정할 수도 있다.
    - 여러 요청이 들어왔을때, 요청을 프로세서에 고르게 분배함!
    - BUT, 자원을 공유하지 못함(db 사용하면 괜찮음!)
3. 노드 프로세서를 백그라운드에서 돌림!

<br>

### 쓰기 좋은 옵션들!
- --watch : 코드에 변동이 있으면 알아서 재시작!
- list : 실헹중인 프로세서들을 보여줌
- monit, logs : 로그들을 보여줌
- flusht : 모든 로그 비우기
- deploy(전개) => 이름.config.js 만들어서 안에 자세한 옵션값들 지정가능!
    - 그 후 pm2 start 이름하면 개꿀~


### pm2의 좋은 기능(추가적으로 Clsutering!, Load Balancing!)
- **Clustering**
    - 똑같은 구성의 서버가 병렬된 연결된 상태임 (멀티 프로세싱)
    - 로드 발란서가 일을 넘겨주면 각 클러서터링된 서버들이 처리함
    - **클러스터링 데이터들은 어떻게 동기화?**
        - Sync(동기화)방식 : 동기화 되는 시간 때문에 시간이 꽤나 걸림
        - OS공부...
- **Load Balancing**
    - 서버에 로드되는 것을 클러스터링 된 서버별로 균등하게 나누어 주는 또다른 서버!
    - 처리량에 등에 따라 차별적으로 서버 요청을 나눠줌
    - 서버에 이상이 있다면? 그 서버로의 분배를 멈춤!
    - **Load Balancer에 이상이 생기면?**
        - 로드밸랜서도 서버들 2개로 구성됨.
        - Master로 돌리다 이상시 Standby로 넘긴다(동작 이름은 Fail Over)