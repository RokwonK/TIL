# RxSwift


ReactiveX란  
1. 이벤트를 발생하는 Observable
2. 이것을 관찰하는 Observer

이 두가지를 이용해 비동기 이벤트들을 쉽고 안전하게 처리할 수 있는 라이브러리.  
이 기능을 RxSwift를 통해 사용가능!  
RxCocoa를 통해 UI의 이벤트들도 Rx로 확장해 사용가능!  


## RxSwift를 사용하면 좋은 장점?
이 라이브러리 사용했을때의 장점에 대해 먼저 알아보자구!  
1. 일관성 없는 비동기 코드 해결! - [GCD](https://github.com/RokwonK/til/blob/master/iOS/gcd.md)를 하나로 통일!
2. 코드가 깔끔해진다(가독성도 굿~)!
3. 옵저버 패턴에 사용 하기 편히하다!
4. 멀티쓰레드 처리가 간편하다 (GCD 사용이 용이해진다!)
5. Operator(이벤트를 변환 및 처리 가능 => fiter(거르기), map(변환하기) ) 가능!
6. 데이터 변경에따라 연관성 없는 서로 다른 두 뷰가 바뀌어야 한다면? - [Notification Center](https://github.com/RokwonK/til/blob/master/iOS/notification_center.md)보다 간단하게!  

<br>

## Observable과 Observer

Observable은 직역하면 `관찰 가능한`,  
Observer는 직역하면 `관찰자`이다.  

Observer는 이 Observable을 subscribe(구독 : Observable과 Observer를 연결)하고 이벤트를 전달하는 것을 감시한다.  
- (rx.tap과 같이 tap 이벤트가 발생되면 이벤트가 발생되게 할 수 도 있고)  
- (onNext("말~"),onError를 이용하여 직접 이벤트 방출을 할 수도 있다.)  

위의 두가지 방법을 사용하면 Observer가 이벤트 발생을 알고 subscribe에 정의해논 코드를 실행한다.  
```swift

Observable.onNext("parameter")
Observable.subscribe(onNext : { parameter in
    // 작업코드
    print(parameter)
})
// 또는
Observable.subscribe { event in
    switch event {
        case .next(let parameter) {
            print(parameter)
        }
    }
}
button.rx.tap.subscribe { _ in
    // 작업코드
    // 이 경우 onNext() 이베트만 있지 파라미터는 없음
}
```

## Observable의 이벤트
- **onNext()**  
다음 값을 방출함(전달함).  
- **onError()**  
나와야 할 값이 나오지 않음.(값 방출에 있어서 에러)  
종료됨. 
- **onComplete**
다음 값이 없음.(모든 값 방출 완료)  

dispose, disposable, disposeBag 등...

## Single

## Operators

## Subject

## Schedulers
dispatch queue와 동일함



<br>





참고 블로그 출처 - [https://brunch.co.kr/@tilltue/2](https://brunch.co.kr/@tilltue/2)