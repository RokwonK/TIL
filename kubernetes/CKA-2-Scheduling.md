# Scheduling

쿠버네티스 `Scheduler`가 동작하지 않는다면?
`Scheduler`는 Pod 생성 시 이를 생성할 nodeName을 자동으로 설정하고 Running 시켜준다.
`Scheduler`가 없다면 실행시킬 nodeName이 설정되지 않기 때문에 Pod은 Pending 상태가 된다. create 시 Pod을 실행하고 싶다면 yml에 nodeName을 명시해주면 된다.


## Pod 분류하기 - Label & Selector
Label은 Pod을 분류하기 위한 속성.
Selector에서 원하는 Label들로 필터링해 설정을 적용할 수 있음.(ReplicaSet-Definition 등)


## Taint와 Toleration
Taint(얼룩)는 Node에 적용 - Node가 Pod을 밀어낼 수 있음
Toleration(내성)은 Pod에 적용 - 스케줄러에게 특정 Taint를 가지는 Node에 Pod을 스케줄링할 수 있게 허용함.

Taint는 Node가 가지는 특정 제한. 아무 Pod이나 들어오지 못하도록 치는 방어막.
Toleration은 Pod이 특정 Taint에 대한 내성을 가지도록 하는 것. 이 내성을 가지면 해당 Taint를 가지는 Node에서 실행할 수 있는 권한을 가지는 것.
- Pod 이 특정 노드 Taint에 대한 Toleration이 있다고 다른 노드에서 실행 못하는건 아님. Taint가 있더라도 실행할 수 있는 권리를 가지는 것.
- Master Node는 다른 Pod들이 실행되지 못하도록 Taint가 존재함. 그래서 스케줄러가 마스터 노드에 Pod을 실행시키지 않음.

### Taint 적용
```bash
kubectl taint nodes node1 key1=value1:NoSchedule
```

### 특정 노드 확인
```bash
kubectl describe node node01
```


### Toleration 적용
```yml
# PodSpec

tolerations:
- key: "key1" # Key
  operator: "Equal" # =
  value: "value1" # Value
  effect: "NoSchedule" # NoExecute 등
```


## Node Selectors & Affinity
Pod이 특정 노드 에서만 실행될 수 있도록 설정

### Node Selectors
```yml
spec:
  # ...
  nodeSelector:
    size: Large
```

### Node Affinity
더 디테일한 설정을 할 수 있음
```yml
spec:
  # ...
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoreDuringExecution:
        nodeSelectorTerms:
          - matchExpressions:
            - key: size
              operator: Exists
```

## Resource Requirement
Pod별로 Node의 사용 Resource를 request, limit을 정할 수 있다.
- request는 초기 할당받을 양이다.
- limit 설정을 안하면 Node 메모리를 초과하는 순간 OOM(Out Of Memory)로 컨테이너가 죽는다.
- limit만 설정하면 k8s는 limit값을 request로 설정한다.

```yml
spec:
  # ...
  containers:
  - name: ''
    image: ''
    resources:
      requests:
        memory: "1Gi"
        cpu: 1
      limits:
        memory: "2Gi"
        cpu: 2
```

## Daemon Sets
복제 셋과 같지만 **모든 노드에 Pod을 하나씩 만드는 것**
- 유스케이스1 - 모니터링, 로그 뷰어 Pod
- 유스케이스2 - kube-proxy(Pod간 네트워킹을 위한 필요한 요소) 
- 유스케이스3 - 네트워킹 솔루션 Pod

```yml
kind: DaemonSet
spec:
  # ...
  selector:
    matchLabels:
      app: monitoring-agent
  template:
    metadata:
      labels:
        app: monitoring-agent
    spec:
      containers:
      - name: monitoring-agent
        imgae: monitoring-agent
```

### 어떻게 생성할까?
1.12 까지는 Daemon용 Pod이 nodeName을 가지는 식이었음
그 이후는??


## Static Pods
각 Node의 `/etc/kubernetes/manifests` 디렉터리에 직접 저장한 PodFile들. Kublet이 이를 읽어 실행함.
이런 파일이 들어가는 디렉터리는 `kublet.service` 파일에서 설정가능
- --config=kubeconfig.yaml
- kube admin들의 시스템 Pod들이 실행될때 이를 활용함


## Multiple Scheduler
scheduler.yml 파일을 만들고 config 파일을 따로 만들어서 다중 스케줄러를 설정할 수 있다.

## Scheduler Profile 설정
스케줄러의 동작과정
1. queue - Priority Sort
2. Filtering - Pod에 맞는 Node 필터링
3. Scoring - Node별 스코어링
4. Binding - 높은 Score의 Node에 추가

각 단계별 pre, post로 필터링 설정할 수 있음.