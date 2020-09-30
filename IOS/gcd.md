# Grand Central Dispatch - GCD

하드웨어에서 동시성 코드 실행(동기, 비동기, 직/병렬 처리)을 지원하는 기능이다.  
Thread 관련 작업(멀티쓰레드?, 멀티 코어?, 동기처리?, 비동기 처리?)를 처리하는 것이 GCD임.  
Dispatch(배포)라는 뜻과 같이 클로져를 Queue넣고 쓸레드로 내보내 처리한다.  
DispatchQueue 함수를 사용한다면 Thread-safe(Thread 충돌x) & 효율적인 멀티쓰레드 작업 수행이 가능하다!  
오래 걸리는 작업에 UI 업데이트를 막지 않기 위해 주로 사용된다!  

<br>

## DispatchQueue  
이 함수에서 Queue의 종류가 3가지 있다. - main, global, custom

### **main**
메인 쓰레드에서 작동되는 큐이다. (Serial Queue - 직렬 작업을 하나씩 처리함)  
UI 업데이트 작업이 있다면 여기서 처리해야 한다!

### **global**
우선순위에 따라 처리 (Concurrent Queue - 병행 작업)  
QoS(Quality of Service) 우선순위  
1. userInteractive
2. userInitiated
3. utility
4. background


### **custom**
직접 만드는 큐. 뒷단에서 serial로 돌아갈 필요가 있는 작업을 수행

<br>



Queue를 선택 후, sync(동기)로 처리할 것인지, async(비동기)로 처리할 것인지도 알려주어야한다.  
- sync : 작업이 끝날때까지 기다림 (하나가 완벽히 끝나고 다음 작업을 끝냄)
- async : 작업이 끝난걸 보증하지 않고 다음 작업을 실행

>Serial 과 Concureent (직렬과 병행) - 쓰레드 1 or 다수  
sync와 async - 쓰레드 안에서의 흐름

그 후, Closure안에 어떤 작업을 처리할 것인지를 명시하면 된다.  
```swift
// .메인큐.동기/비동기
DispatchQueue.main.async {
    // 처리할 작업
}
```

<br>

## 번외 - OperationQueue

사실 이러한 동시성 코드 실행을 해주는 API는 DispatchQueue를 제외하고 OperationQueue라는게 있다.  
OperationQueue는 GCD 위에서 돌아가는 좀더 고차원의 API다.  
고로 DispatchQueue보다는 느리다는 단점이 있다.  
OperationQueue는 작업 중 멈추기, 다시시작, 취소 등의 기능을 할 수 있다.  

장단점이 중요하므로 각 특성에 맞게 사용하는 것이 좋을 듯 싶다.  
Operation은 보다 복잡한 작업을 처리할때,  
Dispatch는 간단한 작업을 비동기 처리시에 좋다.  

<br>
<br>

*참고 URL_1 - [http://jdub7138.blog.me/220949191761](http://jdub7138.blog.me/220949191761)*  
*참고 URL_2 - [https://devmjun.github.io/archive/1-GCD](https://devmjun.github.io/archive/1-GCD)*