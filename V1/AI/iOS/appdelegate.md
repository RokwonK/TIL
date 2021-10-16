# AppDelegate 역할

<u>프로젝트 생성 시 애초에 만들어지는 함수들 설명</u>


App Delegatea : App의 대리자 즉, App이 할 일(Fore,background 진입, 외부에서의 요청)을 구현한다.  
(fore,background에 대한 요청은 ios13이후 Scenedelegate로 옮겨짐)

### **@UIApplicationMain**
이것이 붙은 Class에서 UIApplication(Run Loop라고 생각하면 됨)의 역할을 대신함

<br>

### 1. 앱 실행시 호출

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool
```

<br>

### 2. 새로운 scene(간단히 말해 view)이 만들어 지면 호출(만들어 졌다는 알림)

```swift
func application(_ application: UIApplication, configurationForConnecting connectingSceneSession: UISceneSession, options: UIScene.ConnectionOptions) -> UISceneConfiguration {
```

<br>

### 3. scene이 사라질때 호출

```swift
func application(_ application: UIApplication, didDiscardSceneSessions sceneSessions: Set<UISceneSession>)
```

<br>

### Coredata의 기능중 데이터 베이스에 접근 할 수 있게 해주는 변수
```swift
 lazy var persistentContainer: NSPersistentContainer
```

<br>

### Coredata를 통해 데이터베이스에 값 setting 해주는 함수
```swift
func saveContext ()
```