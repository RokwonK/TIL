## ios 프로젝트 시 생기는 기본 파일들의 역할

scene => UI의 각 인스턴스를 관리,(view controller도 scene, 즉 하나의 화면이라고 생각하면 됨)  
scene들은 같은 메모리, 앱 프로세스 공간을 공유하면서 동시에 실행됨  
즉, 하나의 앱은 어려 scene과 scene delegate  

## AppDelegate.swift
- Process의 Lifecycle 관리(앱 시작, 앱 끝 등... 즉, 앱의 생명주기)
- Session의 Lifecycle을 맡는다.(앱에서 생성한 모든 scene 정보를 관리함)
    - Scene session(Scene의 life-time)을 통해 정보 업데이트를 받음
    - session(life-time )의 보고를 받음
- 앱의 데이터 구조 초기화
- scene을 설정함
- 앱을 타겟으로 하는 이벤트에 대응
- 푸쉬 알림 서비스 등의 서비스를 등록함

## SceneDelegate.swift
UI의 Lifecycle을 맡는다. 이들의 update를 AppDelegate에 보고함


## info.plist
앱의 기본 정보를 가지고 있음. meta데이터들 이라고 생각.  
앱의 이름, 카메라 접근 권한, 버전, wi-fi사용여부, 실행 가능한 아이폰 환경 등...

## Assets.xcassets
앱의 자산을 가짐. ex) 실행화면 이미지, 아이콘 이미지 등...