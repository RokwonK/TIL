# delegate pattern

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