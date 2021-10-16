# RxSwift - Operators(연산자)

연산자란 함수라고 할 수 있음.  
Observable 클래스에 포함되어있는 함수임.  
함수형 프로그래밍 원리에 따르므로 메서드 보다는 함수에 가까움.  
(side effect[멀티 쓰레드 안에서 경쟁조건에 빠져 잘못된 결과가 나오는 것]가 없는 순수 함수)  

<br/>

## Observable을 생성하는 연산자

### just
하나의 항목만 방출하는 Observable을 생성  
파라미터로 전달받은 요소를 그대로 방출함

```swift
Observable.just(1)
.subscirbe{
    print($0)
}.disposed(by : disposeBag)
// 1을 전달하고 바로 Completed됨.

Observable.just([1,2,3])
.subscirbe{
    print($0)
}.disposed(by : disposeBag)
// [1,2,3]을 전달하고 바로 Completed됨.
```

### of
2개 이상의 항목을 방출 가능한 Observable을 생성

```swift
Observable.of(1,2,3)
.subscirbe{
    print($0)
}.disposed(by : disposeBag)
//1,2,3을 전달 후 completed

Observable.of([1,2,3], [4,5,6])
.subscirbe{
    print($0)
}.disposed(by : disposeBag)
// [1,2,3], [4,5,6]을 전달 후 completed
```

### from
배열 형태를 각 항목으로 쪼개서 방출 가능한 Observable을 생성

```swift
Observable.from([1,2,3])
.subscirbe{
    print($0)
}.disposed(by : disposeBag)
// 1,2,3을 전달 후 completed
```

<br/>

### 여러 연산자들

``` swift

// 원하는 데이터만 걸러서 방출
// true를 리턴하는 값만 방출됨
.fiter{ $0.isMultiple(of : 2)} // 2의 배수만 방출


// 데이터에 값을 덧붙여 방출
.map{ $0 + 3 }


// 원본 Observable이 방출하는 항목을 새로운 Observable을 방출
// 각 Observable이 하나의 Observable로 합쳐짐
// 이 합쳐진 Observable이 구독자에게 방출됨
// 네트워크 요청을 구현할때 많이 쓰임
.flatMap{ $0.asObservable() }


// 2개의 Observable을 결합 (개수는 여러개 가능)
// 각 Observable에서 가장 최근에 방출된 항목들을 결합하여 
// Observable을 만듬(result Observable)
// 이 Observable에서 값을 방출
// 인자로 설정된 모든 Observable이 completed되었을 때, completed가 됨.
.combineLatest(x, y) { x_n, y_n -> String in
    return "\(x_n) \(y_n)"
}
```