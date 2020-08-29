# Scenedelegate 역할

ios 13부터 생김.  
scene(화면의 뷰)의 생명주기를 담당하고 있음.

<br>

### 1. Scene(하나의 뷰)가 생성될 때 호출
```swift
func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions)
```

<br>

### 2. Scene이 지워질 때(스택에 쌓이는게 아니라 아예 없어질 때) 호출
```swift
func sceneDidDisconnect(_ scene: UIScene)
```

<br>

### 3. Scene이 foreground에 나왔고 사용자가 사용할 수 있게 되었다
```swift
func sceneDidBecomeActive(_ scene: UIScene)
```

<br>

### 4. Scene이 역할을 끝내고 background으로 들어갈 거다
```swift
func sceneWillResignActive(_ scene: UIScene)
```

<br>

### 5. scene foreground로 들어 갈거고, 사용자에게 보여질것이다.
```swift
func sceneWillEnterForeground(_ scene: UIScene)
```

<br>

### 6. scene background로 들어갔다.
```swift
func sceneDidEnterBackground(_ scene: UIScene)
```

<br>