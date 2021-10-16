# RxSwift - Binding


Binding에는
**Data Producer(데이터 생산자)** => Observable : Observable 타입을 채용한 모든 형식  
**Data Consumer(데이터 소비자)** => Lable, view같은 UI Component, Observer임  

Binder는 UI Binding에 사용되는 특별한 Observer. 즉, Data Consumer  
UI code는 Main Thread에서 실행되어야 한다.  
Binder는 Main Thread에서 실행되는 것을 보장한다.  

RxCocoa에는 각 UI Compoenet들을 Observable, Binder들로 wrapping되어 있음.  
즉, 각 UI Component들을 wrapping된 형식으로 사용가능함.  

```swift
//textfield의 값을 textlabel에 보여주는 예시

@IBOutlet weak var label : UILabel!
@IBOutlet weak var field : UITextField!

override func viewDidLoad() {
    super.viewDidLoad()

    field.text = ""
    // UITextField에 포커스가 맞춰짐
    field.becomeFirstResponder()

    // UITextField의 Observable
    field.rx.text
        .observeOn(MainScheduler.instance) // Main Thread에서 실행
        .subscribe(onNext : { [weak self] str in
            self?.label.text = str
        })
        .disposed(by : disposeBag)

    // or
    field.rx.text
        .bind(to : label.rx.text) //label의 Binder => Main Thread에서의 실행이 보장됨
        .disposed(by : disposeBag)

}


```