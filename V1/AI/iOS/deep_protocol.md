# Protocol 및 Delegation pattern

스위프트의 protocol이라고 하면 기능을 구현하지 않고 정의 만을 해놓은 것.  
구조체, 클래스, 열거형이 이 프로토콜을 채택(Adopted)해서 사용할 수 있음. 만약 프로토콜이 AnyObject를 상속 받는 다면 클래스만이 이 프로토콜을 채택할 수 있다.  
이때 프로토콜에 정의해놓은 기능들을 모두 구현해야 한다. 이러한 객체를 프로토콜을 준수한다(Conform)고 말한다.

<br><br>

### **Protocol Composition**
여러 프로토콜을 단일 요구사항, 즉 하나로 결합할 수 있다. 예를 들어  
```swift
protocol A {}
protocol B {}
class C : NSObject {}
func makeObject1(value : A & B) {}
func makeObject2(value : C & B) {}
```  
위의 경우 makeObject의 인자 value에는 A와B를 준수하는 타입이어야 한다.  

<br>

### **Optional Protocol Requriements**
프로토콜에 대해 선택적으로 요구사항을 정의 할 수 있다.  
단, 선택적 요구사항 앞에는 optional 이라는 키워드가 붙고 프로토콜과 해당 요구사항에 @objc(Objective-C 클래스(NS들 등)나 다른 @objc 클래스를 상속받은 클래스에서만 사용가능함)를 붙어야 한다.
```swift
@objc protocol P {
    @objc optional func f()
    @objc optional var a : Int { get }
}
```

<br>

### **프로토콜의 Extension**
프로토콜에도 extension을 할 수가 있다.  
이 extension에서 이 프로토콜을 구현 할 수 있는데, 저장프로퍼티는 구현하지 못하고 연산프로퍼티는 구현가능하다.  
그리고 그렇게 구현된 기능들은 다른 객체들이 프로토콜을 상속했을때, 다시 정의할 필요가 없다는 것!
```swift
protocol P{
    var a : Int {get}
}

extension P{
    var a : Int {
        return 1
    }
}
// 프로토콜 P를 구현하지 않아도 된다는 것!!!
class A : P {}
```
