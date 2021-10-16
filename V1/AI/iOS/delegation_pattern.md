# delegation pattern을 사용하는 이유는 뭘까

프로토콜이 있고
어떤 클래스가 있고
그 클래스를 구현하는 위임자 클래스가 있음.  

delegation pattern을 익히기에 앞서 디자인패턴에 대해서 먼저 알아보자.  
**디자인 패턴**이란, `자주 일어나는 문제들을 어떻게 해결할 것이냐?`에 대한 해결책.  
그렇다면, 디자인 패턴들을 어떠한 문제를 해결한 것들 이라고 볼 수 있다.  

**delegation pattern**는 일부 클래스의 책임을 다른 클래스의 인스턴스에게 위임하는 디자인 패턴  
하나의 객체가 다른 도우미역할의 객체를 사용하는 것이다.  
왜냐하면, 어떠한 일을 수행/제공에 있어서 다른 객체를 사용하는 것이 효율적이기 때문이다.  
왜 그럴까??  
delegate가 필요한 객체에 그냥 자신이 모든 걸 구현하게 한다면,  
그 셀을 나중에 사용하지만 다른 기능을 가져야할 때, 셀 자체를 아예 새롭게 구현해야한다.  
즉, 재사용성이 현저히 떨어진다는 뜻이다.  
반면에, 상황마다 달라지는 행동을 프로토콜로 따로 정의 해놓고, 해당 프로토콜 타입의 delegate 변수를 선언해 둔다면,  
그 프로토콜를 준수한 모든 클래스들을 넣을 수 있다. 그렇게 하면, 셀과 delegate로 넣을 수 있는 수의 관계는 1:N이 되고, 즉 그 셀에서 어떤 delegate를 채택하냐에 따라 다른 행동을 취할 수 있게 된다.  
바로 셀의 재사용성이 증가. 즉, 훨씬 더 유연한 사용이 가능하다는 뜻이다.  
여기서 중요한 것은 delegate 변수를 약한 참조로 선언해야한다는 것!  
default(강한 참조)로 선언하게 되면 Retain cycle이 일어난다는 것은 다 알고 있을 것이다.(짧게 말해서 클래스 인스턴스 내부에 강한참조가 있으면 ARC가 메모리 해제를 해주지 못함)


## 과정 
1. 프로토콜을 채택
2. 위임자를 정해줌(누가 이 일을 맡을 것인지)
3. 대신해주는 함수를 선언

```swift
// UITextFiledDelegate 프로토콜을 채택
class ViewCon : UIViewController, UITextFiledDelegate  {
    @IBOutlet weak var label : UILabel!;
    @IBOutlet weak var textField : UITextField!;

    override func viewDidLoad() {
        super.viewDidLoad();

        // textFiled의 위임자는 바로 나(ViewCon)
        // 이벤트가 발생하면 프로토콜에 따라 너에게 응답해 줌
        textField.delegate = self;
        
    }


    // TextFiledDelegate => 구현해야하는 프로토콜
    // 이 프로토콜을 준수한 클래스를 UITextFiled의 delegater가 될 수 있음
    func textFieldShouldReturn(_ textField: UITextField) -> Bool {
        labe.text = textField.text
        return true;
    }
    

    @IBAction func buttonClicked(_ sender: Any) {
        // label.text = textField.text;
    }
}
```