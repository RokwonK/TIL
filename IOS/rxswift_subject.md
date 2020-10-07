# RxSwift - Subject

Subject는 옵저버블이자 옵저버!!!  
다른 소스로부터 이벤트를 전달 받을 수도 있고,  
다른 소스로 이벤트를 전달할 수도 있음.  
종류로는 아래 4개가 존재함.  
- [AsyncSubject](#AsyncSubject)
- [PublishSubject](#PublishSubject)
- [BehaviorSubject](#BehaviorSubject)
- [RelaySubject](#BehaviorSubject)

### **왜 필요할까?**  
그냥 Observable은 unicast다. 즉, Observer와 1대1로만 subscribe이 가능하다.  
But, Subject는 multicast. 하나의 Subject에 *다수의 Observer*들이 subscribe을 할 수 있다.  
또한, Observer와 Observable의 성질을 다 가지고 있어 중간 다리 역할도 할 수 있다.  
어떤 Observable에 subscribe해 이벤트를 받으면 자신이 기능을 수행 후 다른 Observer들에게 이벤트를 넘겨줄 수 있다.  


<br/>

## AsyncSubject
- complete가 나기 직전 마지막 이벤트만 발생하고 종료한다.
```swift
let disposeBag = DisposeBag()

let asyncsubject = AsyncSubject<Int>()
asyncsubject.subscribe {
    print($0)
}.disposed(by : disposeBag)

asyncsubject.onNext(1);
asyncsubject.onNext(2);

asyncsubject.onNext(3);
asyncsubject.onCompleted();

// 3만 전달됨.
```


<br/>

## PublishSubject
- 옵저버가 구독이후 생기는 이벤트를 전부 전달
```swift

// 메모리 회수
let disposeBag = DisposeBag()

// Subject 선언
let publishsubject = PublishSubject<Int>()

publishsubject.onNext(0);
publishsubject.subscribe {
    print($0)
}.disposed(by : disposeBag)

publishsubject.onNext(1);
publishsubject.onNext(2);
publishsubject.onNext(3);
publishsubject.onCompleted();

// 0은 구독전이므로 전달이 되지 않음.
// 1,2,3 전부 전달됨
```

<br/>

## BehaviorSubject
- 초기값을 가지는 Subject 생성시에 처음 초기값을 가지며,  
Observer가 구독시 바로 초기값을 return해 준다.
```swift
let disposeBag = DisposeBag()

// 초기값을 설정
let behaviorsubject = BehaviorSubject<Int>(value : 1)

behaviorsubject.subscribe {
    print($0)
}.disposed(by : disposeBag)

behaviorsubject.onNext(2)
behaviorsubject.onNext(3)

behaviorsubject.subscribe {
    print($0)
}.disposed(by : disposeBag)


// 1,2,3,3으로 전달된다.  
// 첫번째 구독으로 1 그리고 onNext로 2,3이 전달된다.  
// 여기서 마지막값을 Subject가 가지고 있다.
// 다른 observer가 구독시 마지막으로 저장된 값을 전달해준다.
```

<br/>


## RelaySubject
- 지정한 개수의 이벤트를 저장 후 subscirbe이 되는 시점에 모든 이벤트를 전달한다.  
전에 일어났던 이벤트를 저장하고 구독되는 시점에 모아놓은 값을 전부 전달한다.  
구독 후 일어나는 이벤트는 바로 바로 전달이 된다.  
생성함수는 2가지가 있다.  

```swift
let disposeBag = DisposeBag()


// buffferSize만큼의 이벤트만 저장
let relaysubject = RelaySubject<Int>.create(bufferSize : 2)
// or
// 모든 이벤트를 저장함
let relaysubject = RelaySubject<Int>.createUnbounded()

relaysubject.onNext(1)
relaysubject.onNext(2)
relaysubject.onNext(3)
relaysubject.subscribe {
    print($0)
}.disposed(by : disposeBag)
// create의 경우 2,3만 전달
// createUnbounded의 경우 1,2,3을 전달한다.
```


<br/>

추가적으로 ~Subject 대신에 ~Relay `ex) PublishRelay<Int>` 가 있다.  
~Relay는 completed, error를 발생하지 않고 Dispose 되기 전까지 계속 작동한다.  
끊김이 없으므로 UI Event에 사용하기 좋다!
