# View Controller

View Controller 말 그대로 뷰들을 컨트롤하는 책임을 가진다.  
ios에서 View Controller에 해당하는 것은 당연 swift  
- 데이터의 변화에 응답해 뷰의 컨텐트들을 update
- 이벤트 핸들링(사용자와의 대화에 응답)
- 뷰의 인터페이스 레이아웃을 관리
- 여러 뷰컨트롤러들과 앱을 구성

## 뷰컨트롤러의 타입
하나의 화면을 관리하는가 여러개의 하면을 관리하는가에 따라 나눠짐.

1. Content View Controller
    - 자신의 모든 뷰를 스스로 관리
2. Container View Controller
    - Navigation 및 TabBar같은 여러개의 View Controller(자식들)를 제어
    - 자신의 뷰, 자식들의 뷰 커트롤러들의 root view들을 관리(크기 조절, 위치조절)만 직접 관리

<br>

## 생명 주기
뷰컨트롤러 => 뷰컨이라고 하겠음.  

뷰 컨트롤러간에도 전환이 필요 (1번에서 2번으로 넘어가고 그런 전환)  
이전 뷰컨의 뷰가 유지되는 경우도 아닌 경우도 있음 or 이전 뷰 컨의 정보를 이어받아 작업을 해야할 수도 있음.  
그 해당지점을 이벤트화 해 각 시점에서 해야할 동작을 지정함.  

이제부터 생명주기
1. init : 뷰가 생성
2. loadView : 뷰를 만드는 중
3. viewDidLoad : 뷰가 로드됨
    - 초기화면 구성 즉, 맨 처음 한 번만 사용됨
4. viewWillAppear : 뷰가 화면에 나타날 것이다.
5. viewDidAppear : 뷰가 화면에 나타남
6. viewWillDisappear : 뷰가 사라질 것이다.
7. viewDidDisappear : 뷰가 사라짐.

ex) 첫번째화면 => 두번째화면
1_willdisappear => 2_didload =>  
2_willappear => 1_diddisappear => 2_didappear 