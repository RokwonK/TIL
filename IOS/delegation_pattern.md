# delegation pattern을 사용하는 이유는 뭘까

프로토콜이 있고
어떤 클래스가 있고
그 클래스를 구현하는 위임자 클래스가 있음.  

delegation pattern을 익히기에 앞서 디자인패턴에 대해서 먼저 알아보자.  
**디자인 패턴**이란, `자주 일어나는 문제들을 어떻게 해결할 것이냐?`에 대한 해결책.  
그렇다면, 디자인 패턴들을 어떠한 문제를 해결한 것들 이라고 볼 수 있다.  

**delegation pattern**은 하나의 객체가 다른 도우미역할의 객체를 사용하는 것이다.  
어떠한 일을 수행/제공에 있어서 다른 객체를 사용하는 것이 효율적이기 때문이다.  
Retain cycle을 회피하기 위해?

일부 클래스의 책임을 다른 클래스의 인스턴세에게 위임하는 디자인 패턴

- 델리게이트 패턴  
하나의 객체가 모든 일을 처리하는 것이 아니라 **일부를 다른 객체에 넘기는 것**을 뜻함.  


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


    // 대신해주는 함수 선언
    // UITextFiledDelegate의 메소드 구현체
    // textField에 글을 쓰고 (return 여기서 enter)를 하면 이 함수가 불려짐
    func textFieldShouldReturn(_ textField: UITextField) -> Bool {
        labe.text = textField.text
        return true;
    }
    

    @IBAction func buttonClicked(_ sender: Any) {
        // label.text = textField.text;
    }
}
```

## 사용하는 이유?
각 행동에 따라 분리되므로 재사용 및 유지 관리가 훨씬 쉬운 코드 작성 가능