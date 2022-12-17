---
title: CloudFront 살펴보기
author: 김록원
date: 2022.12.17
category: AWS
---

# CloudFront 살펴보기
짧은 지연시간과 빠른 속도로 데이터, 동영상, 애플리케이션 및 API를 전송하는 고속 콘텐츠 전송 네트워크(CDN)  

> **💡 CDN(Content Delivery Network)**
페이지, 이미지, 동영상 서버에서 받아와 캐싱  
빠른 컨텐츠 제공  
서버로 요청이 필요 없기 때문에 서버의 부하를 낮추는 효과  

<br />  

### 동작 방식

---
이미지  

---

1. 클라이언트로부터 요청 받은 컨텐츠가 엣지 로케이션에 있다면 전달  
2. 요청 받은 컨텐츠가 엣지 로케이션에 없다면 `Origin` 으로부터 제공받아 전달(이때 캐싱)  

> **💡 엣지 로케이션**  
컨텐츠가 캐싱되고 유저에게 제공되는 지점.  
즉, cloudFront가 물리적으로 존재하는 공간  

<br />  

### CloudFront의 구성  

**`Origin`**  
CloudFront의 실제 컨텐츠가 존재하는 근원 리소스이다.
1. S3
  - 파일을 배포하고 엣지에서 캐시할때 자주 사용
  - OAI(Origin Access Identity)를 이용하여 S3와 CloudFront 사이의 보안강화(S3가 오직 CloudFront하고만 소통할 수 있도록 해줌)
  - 엣지 로케이션이 S3에 접근하기 위해선 IAM 역할인 OAI 신분을 사용해야 함, 버킷 정책이 이 역할의 액세스를 허용해야함
  - CloudFront가 S3로 접근하기 위한 입구로 사용될 수 있음
2. Custom Origin(HTTP)
  - HTTP 접근이 가능한 리소스(ELB, S3에 업로드된 웹사이트, EC2)들의 Origin이 될 수 있음
  - ELB나 EC2의 경우 엣지로케이션이 접근하기 위해선 Public해야하며 엣지 로케이션의 IP를 허옹해야함
  - CloudFront가 HTTP로 접근하기 위한 입구로 사용될 수 있음
  
<br />  

**`Distribution`**  
- CloudFront의 CDN 구분 단위
- 여러 엣지 로케이션으로 구성된 컨텐츠 제공 채널  

<br />  

**`주요 설정 및 용어`**
- TTL(Time To Live) : 캐싱된 아이템이 살아있는 시간
- 파일 무효화(invalidate) : TTL이 지나기 전에 강제로 캐시 삭제
- OAI(Origin Access Identity) : CloudFront가 근원 리소스에 접근하기 위한 신분증(IAM 대신), 각 정책은 이 신분증에 대해 접근을 허용해야함.

<br />  

### 장점
- DDos 공격을 방어하는 방화벽을 제공해줌
- 인증서를 로드하여 HTTPS 엔드포인트를 노출하고 해당 트래픽을 암호화해야하는 경우 내부 HTTPS에서 애플리케이션에 내부적으로 통신하게끔 해줌
- 지역 제한을 걸 수 있음
  - **화이트리스트**를 이용해 허용된 국가의 유저들만 사용하게 할 수 있음
  - **블랙리스트**를 이용해 특정 국가 유저들은 접근불가하게 만들 수 있음
  - 수신되는 IP와 리스트를 비교하여 국가를 알아냄

<br />  

### 가격 등급/요금
- 엣지 로케이션마다 데이터 전송 비용이 다름  
- 엣지 로케이션 수를 줄이는 방법(등급)
  - Price Class All
  - Price Class 200 대부분 리전은 사용가능
  - Price Class 100 저렴한 리전만 사용가능

<br /><hr><br />  

## CloudFront의 기술

### Cache Invalidations(캐시 무효화)
백엔드에서 값을 업데이트할때 CloudFront의 각 엣지 로케이션은 해당 업데이트를 모름. 단지 TTL이 만료되었을때 캐시를 지울뿐이다.  
이러한 문제점으로 TTL이 지나기 전에 저장되어있는 캐시를 지우는 방법을 지원해준다.  

### CloudFront Signed URL
요청을 수행하는데 필요한 제한된 권한과 시간을 제공하는 URL이다. 콘텐츠에 액세스할 수 있는 대상을 제어하는 기능으로써 동작한다. 개별 파일에 대한 액세스를 제공한다.  
S3 버킷뿐만 아니라 백엔드 등에서 원하는 것을 일정 기간동안 사용할 수 있다.  
유료 동영상 접근 권한과 같은 곳에 사용할 수 있다.  

`S3 pre-signed URL`과 유사하지만 이 기능은 S3 버킷에 한정되며 S3에 직접 액세스한다는 차이가 있다.  


<br />  

### CloudFront Signed cookies  
마찬가지로 콘텐츠 액세스할 수 있는 대상을 제어하는 기능이다.  
제한된 파일 여러개에 대한 액세스 권한을 제공하고자 할때 사용한다.  

<br /><hr><br />  

## CloudFront vs 다른 서비스

### CloudFront vs S3 cross region replication  

||CloudFront|S3 cross region replication|
|:---|:---|:---|
|특징     | 엣지 로케이션에 캐시    | 여러리진에 S3 복사본 존재, 실시간 업데이트  |
|사용목적  | 정적컨텐츠의 빠른 로딩   | 읽기전용으로 읽기 속도향상 |
|사용처   | 캐시를 이용한 성능향상   | 동적컨텐츠 로드          |

<br />

### CloudFront vs AWS Global Accelerator
두 서비스 모두  
글로벌 네트워크로 전세계의 엣지로케이션을 사용한다는 것과
DDos 보호를 위해 AWS Shield를 사용한다.  
그렇다면 차이점은 무엇일까

||CloudFront|AWS Global Accelerator|
|:---|:---|:---|
|특징      | HTTP,HTTPS 컨텐츠 캐시로 빠른 전송      | TCP, UDP의 모든 트래픽에 대해 빠른 전송 |
|접근방식   | DNS를 통한 실시간 최적의 엣지 로케이션 IP 제공     | Anycast IP를 통한 클라이언트의 접근   |
|요청처리   | 엣지로케이션, 캐시없을 경우 어플리케이션     | 어플리케이션                        |
|사용목적   | 캐시를 이용한 성능향상  | 네트워크 라우팅에 의한 지연 최소화, 신속한 리전 장애 조치가 필요할때 |
|사용처     | 이미지, 동영상같은 정적컨텐츠 로딩         | 캐시불가, 게임이나 IoT, Voice Over Ip 등에 사용 |


> **💡 Unicast IP와 Anycast IP**  
Unicast IP : 하나의 서버가 하나의 IP 주소를 가지는 것  
Anycast IP : 모든 서버가 동일한 IP 주소를 가지며 클라이언트가 접근시 가장 가까운 서버로 라우팅 됨.  

<br /><hr><br />

## 참고자료
- [AWS CloudFront 공식문서](https://docs.aws.amazon.com/ko_kr/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)
- [유데미 강좌 - AWS Certified Solutions Architect Associate](https://www.udemy.com/course/best-aws-certified-solutions-architect-associate/)
- [다양한 컨텐츠를 빠르게 전송하기](https://dev.classmethod.jp/articles/summit_korea_rapidly_transfer_content/)
- [Cloud Front](https://velog.io/@combi_jihoon/CloudFront)