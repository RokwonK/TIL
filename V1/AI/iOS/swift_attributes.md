# Swift Attributes

Attributes는 1.선언과 2.타입에 적용해 추가적인 정보를 제공하는 예약어이다.  
이 Atrributes는 '@'symbol을 이용하여 표시한다. @IBAction, @IBOutlet 등 등...  
@는 컴파일러에게 어떤 속성을 가지고 있다고 전하는 역할을 하는 예약어이고,  
이 @가 붙은 명령어에게는 어떠한 Attribute가 부여되었다고 말한다.  


1. **선언**  
available => 선언형 Attributes는 Swift의 버전, 플랫폼에 관련해서 제약사항을 걸 수 있다.(두 개 이상의 argument를 가짐)  
여러가지의 선언형 Attributes가 존재한다.  

```swift
// iOS는 11버전 이상, macOS는 10.12버전 이상 나머지 플랫폼은 전부다 가능
@available(iOS 11.0, macOS 10.12, *)
class a {}
```
<br>

2. **Type**  

- @IBAction, @IBOutlet
Interface Builder에서 사용될 수 있고, UI로 연결이 가능해진다는 뜻.  
Action은 Event가 일어나면 호출된다는 뜻이고, Outlet은 그 객체에 접근 가능한 변수랑 얘기다.  


## 사용해 볼만한 Attribute

@IBDesignable, @IBInspectable

1. @IBInspectable  
story보드나 xib 파일을 열면 우측에보이는 설정창을 Inspector라고 한다.  
@IBInspectable은 Inspector에 해당 UI설정 기술을 추가해주게 하는 속성이다. 연산 프로퍼티를 사용해야 한다는 점!  
그러나 이렇게 만들어진 UI값들은 런타임에서 적용되므로 xib에 바로 적용되지 않는다. 고로 컴파일타임에서 즉시 실시간으로 확인하기 위해서 @IBDEsignable Attributes를 클래스 가장 상위에 써 컴파일타임으로 바꿀 수 있다.







