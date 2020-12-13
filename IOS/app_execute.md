# iOS 앱 실행과정

1. 처음 main()함수가 실행된다.
2. main()함수에서 UIApplicationMain()함수를 실행함. - @UIApplicationMain Attributes를 찾아 실행하게 됨.
3. 위 Attributes는 Application 객체(싱글톤 객체임)를 생성하고 그 객체의 delegate로 위 Attributes가 붙어있는 class(AppDelegate)를 지정함.
4. 이러한 설정이 완료되면 AppDelegate의 applicationDidFinishLaunching 메서드를 실행함. - 어플 구동 후 설정준비가 끝났다는 뜻.

<br>

## 이제부터는 App life-cycle!

1. **Not Running**  
앱의 시작되지 않았음. or 시스템에 의해 종료됨

2. **Inactive**  
Foreground - 전면에서 실행 중이지만 아무런 이벤트를 받지 않음
    

3. **Active**  
Foreground - 이벤트를 받고 있음
    - applicationDidBecomeActive

4. **Background**
Background - 코드가 실행되고 있는 상태(앱이 인터페이스에 보이지는 않음)  
    - applicationDidEnterBackbround
    - applicationWillTerminate

5. **Suspend**
앱이 메모리를 차지하고 있지만 실행되는 코드가 없음. (Background의 한 형태)
