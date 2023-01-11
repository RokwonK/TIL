# AWS DataSync

데이터를 동기화해 다른 곳으로 옮길 수 있다.  
- NFS, SMB, HDFS또는 다른 프로토콜을 연결해서 온프레미스나 다른 클라우드로 옮길 수 있다.  
- AWS의 다른 리소스로도 옮길 수 있다.
- 동기화 가능 서비스
  - S3와 Glacier
  - EFS, FSx
- 파일권한과 메타데이터도 보존한다
- 데이터를 받는 쪽에 DataSync Agent가 존재함(AWS DataSync와 TLS 연결함)  
