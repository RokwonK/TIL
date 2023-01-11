# AWS Snow Family
보안성이 뛰어난 휴대용 장치 모음
두가지 경우에 사용함
- 엣지에서 데이터 수집시(Edge conputing)  
- AWS 안밖으로 데이터 이동시(Data migration)  

<br />  

## Data migration
- 네트워크로 데이터 전송시 시간이 오래거림(제한된 연결,대역폭)
- 오프라인으로 데이터 마이그레이션하는 장치
- Snowcone, Snowball Edge, Snowmobil  

Snowball Edge
- 커다란 상자로 TB, PB전송에 용이
- 전송건마다 비용청구
- block storage나 
- Storage Optimized - 80TB 하드웨어 디스크 용량 제공
- Compute Optimized - 42TB HDD 용량 제공
- AWS 데이터를 백업함으로서 재해복구에 대응

Snowcone
- 가볍고 견고하고 안전함
- 용량이 비교적 작음(8TB)
- 공간의 제약을 받는 경우(드론에 설치 등)
- 배터리와 케이블 준비해야함
- 네트워크(AWS DataSync)나 오프라인으로 전송가능

Snowmobile
- 트럭임 개당 100PB
- 1EB = 1000 PB = 1000000TB 가능  

<br />  

## Edge computing
- 엣지 로케이션(배, 광산 등). 즉, 연결이 제한되어 있거나 인터넷 액세스, 컴퓨팅 할 수 없는 곳
- Snowcone, Snowball Edge
- 데이터가 생성되는 곳을 사용자의 가까이에서 처리하고, 데이터를 AWS로 재전송해야할때
- AWS IoT Greengrass라는 서비스를 이용해 Lambda 실행가능
- 1년에서 3년 빌리면 가격 할인을 받을 수 있다.

Snowcone
- CPU 2개, 4GB, 유무선 액세스  

Snowball Edge - Compute Optimized
- vCPU 52개, 208GB, 선택적 GPU(영상차리, 머신러닝을 위한), 42TB 메모리  

Snowball Edge - Storage Optimized
- vCPU 40개, 80GB, 


## OpsHub
기존에 Snow Family 장치를 사용할때 CLI를 사용해야했음
AWS OpsHub를 이용해 Snow Family 장치를 쉽게 이용가능(소프트웨어)  

<br /><hr><br />  

# Amazon FSx
AWS에서 타사 고성능 파일 시스템을 사용하는 완전 관리형 서비스  
- `FSx for Lustre`
- `FSx for NetApp ONTAP`
- `FSx for Windows File Server`
- `FSx for OpenZFS`  

<br />

### FSx for Windows File Server  
- Windows File Server 공유 드라이브
- SMB 프로토콜과 Widnows FTFS를 지원
- 사용자 보안을 추가할 수 있고, ACL도 가능
- Linux EC2 인스턴스에도 마운트 됨
- Microsoft Distrivuted File System(DFS)을 이용해 파일 시스템을 그룹화할 수 있다.  
  - 온프레미스 윈도우와 결합하는 것  
- Storage Option
  - SSD로 지연시간이 짧은 워크로드 가능
  - HDD로 넓은 스펙트럼의 워크로드 가능
- 프라이빗 연결 가능
- S3에 매일 백업됨  

<br />  

### FSx for Lustre  
- Lustre(Linux + Cluster, HPC에 사용함)는 원래 분산 파일 시스템
- 동영상 처리, 금융 모델링, 전자 설계 자동화 등에 쓰임  
- Storage Option
  - SSD - 낮은 지연시간, IOPS 많거나 무작위 파일 작업이 많을때
  - HDD - 크기가 큰 시퀀스 파일 작업
- S3와 Seamless 연결가능
  - S3를 파일 시스템처럼 읽을 수 있음
  - FSx의 출력값을 S3에서 사용할 수 있음
- VPN을 통해 온프레미스와 통합가능

<br />

### FSx for NetApp ONTAP
- NetApp OnTAP
- NFS, SMB, iSCSI 프로토콜과 호환 가능
- 오프레미스 시스템의 ONTAP, NAS에서 실행 중인 워크로드를 AWS로 옮길 수 있다.
- 호환 가능한 폭이 넓음
- 스토리지는 오토스케일링 가능
- 비용이 적게 들며 데이터 압축, 데이터 중복제거도 가능
- 지정 시간 복제 기능이 있음

<br />  

### FSx for OpenZFS
- NFS 프로토콜과 호환가능
- 내부적으로 AWS로 옮길때사용됨
- Linux, Mac, Windows에서 사용가능
- 데이터 중복 기능은 없음
- 스냅샷과 압축을 지원, 비용이 적게든다
- 지정 시간 복제 기능이 있음

<br />  

## FSx File System Deployment Options  
- Scratch File System
  - 임시 스토리지로 데이터 복제 되지 않음 - 기저 서버 오작동시 파일 유실
  - High burst 기능이 있음(영구 파일 시스템보다 성능을 여섯배 높일 수 있음)
  - 단기 처리 데이터에 쓰임
  - 비용 최적화 가능
- Persistent File System
  - 장기 스토리지 동일한 가용영역에 데이터 복제 - 기저 서버 오작동시 몇 분만에 교체
  - 민감함 데이터의 장기 처리 및 스토리지
  - 데이터 사본이 2개생김

<br /><hr><br />  

# AWS Storage Gateway
AWS는 AWS + 온프레미스로 Hybrid Cloud를 지향함
- 보안, 규정준수 등이 힘들기때문에
- Gateway는 온프레미스 쪽에 존재함(AWS의 시스템과 통신하기 위함)

온프레미스 데이터와 클라우드 간의 가교 역할을 하는게 AWS Storage Gateway  
- 재해복구 목적으로 클라우드에 백업 가능
- 지연시간을 줄이기 위해 온프레미스 캐시
- Storage Gateway의 여러타입이 있음

### S3 File Gateway
- 버킷에 원하는 클래스 두고(Glacier는 안됌)
- 온프레미스에 S3 File Gateway를 두고 서버와 NFS/SMB 통신, S3와 HTTPS 통신을 함
- 사용된 데이터는 S3 File Gateway에 캐시로 저장도 됨
- 버킷에 액세스하려면 S3 File Gateway마다 IAM 역할을 생성해야 함

### FSx File Gateway
- FSx for Windows File Server에 네이티브 액세스를 제공함
- FSx File Gateway를 사용하는 이유는 캐시때문에!

### Volume Gateway
- 블록 스토리지로 S3가 백업하는 iSCSI 프로토콜을 사용
- 볼륨이 EBS 스냅샷으로 저장
- 캐시 볼륨 
- 저장볼륨
  - 데이터 세트가 온프레미스에 있으며 S3에 주기적으로 백업됨

### Tape Gateway
- 백엡에 테이프 대신에 클라우드를 활용해 백업
- S3와 Glacier를 사용함  

<br /><hr><br />  

# Storage 비교  
1. S3 : Object Storage
2. S3 Glacier : Objet Archival
3. EBS 볼륨 : IO1,2만 EC2의 다중연결 지원
4. Instance Storage : 고성능
5. EFS : 다중 가용 영역에서 File System 공유
6. FSx for Windows : Window 파일시스템
7. FSx for Lustre : High Performance Computing
8. FSx for NetAppp ONTAP : 높은 OS 호환성
9. FSx for OpenAFS : 관리형 ZFS 파일 시스템
10. Storage Gateway : 온프레미스와 AWS 간 스토리지 연결 방법
11. Transfer Family : FTP, FTPS, SFTP 로 데이터 전송
12. DataSync : 온프레미, AWS에서 AWS로 데이터 동기화
13. Snow Family : 물리적인 데이터 이동
14. Database : 인덱스와 쿼리 작업을 필요로 하는 특수한 워크로드