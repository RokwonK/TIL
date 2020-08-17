## ios 프로젝트 시 생기는 기본 파일들의 역할

scene => UI의 각 인스턴스를 관리,(view controller도 이 scene안에 포함됨.)  
scene들은 같은 메모리, 앱 프로세스 공간을 공유  
즉, 하나의 앱은 어려 scene과 scene delegate

## AppDelegate.swift
Process의 Lifecycle,  
Session의 Lifecycle을 맡는다.(앱에서 생성한 모든 scene 정보를 관리함)  



## SceneDelegate.swift
UI의 Lifecycle을 맡는다