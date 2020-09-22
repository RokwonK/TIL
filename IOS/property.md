# 프로퍼티

클래스, 구조체, 열거형에 연관된 값 - 저장, 연산이 가능함.  
연산 Property, 저장 Property로 나뉨.  

- 저장 : 인스턴스 저장, 타입 저장, 지연 저장
- 연산 : 인스턴스 연산, 타입 연산
<hr>

- 인스턴스 Property : 인스턴스에 속하는 Property  
- 타입 Property : 타입(class, struct, enum)에 속하는 Porperty 앞에 static이 붙음  

마찬가지로 인스턴스 메서드, 타입 메서드도 존재


## 정의
- 인스턴스 저장 Property
    ```swift
    struct Person {
        var name : String = "rok"
        var Age : Int = 24;
    }
    ```
- 타입 저장 Property
    ```swift
    struct Person {
        static var Occupation : String = "developer"
    }
    ```

- 인스턴스 연산 Property => 읽기(get), 쓰기(set)  
(읽기만 전용은 가능 But, 쓰기는 불가능)  
    ```swift
    struct Person{
        var apple : Int = 3;

        var numOfApple : Int {
            get{
                return apple;
            }
            set(value) {
                apple = value;
            }
        }
    }
    ```


## 프로퍼티 감시자

- 값이 변결될때 원하는 동작을 수행가능 (하나의 옵저버임)
- get, set과 함께 사용 불가능
- willSet(변경될 값) {}
- didSet(변경되기 전 값) {}
- ()안써주면 기본으로 newValue 와 oldValue로 후,전 데이터에 접근 가능
    
```swift
struct Person {
    var value : Int = 3 {
        willSet{
            print(\(newValue))
        }
        didSet{
            print(\(oldValue))
        }
    }
}

```


## 지연 저장 프로퍼티

처음 사용할 때까지 초기 값이 계산되지 않는 Property임.  
호출이 있을때 값을 초기화 앞에 lazy를 사용하여 선언함.  

- 주로 복잡한 연산기능, 오래걸리기 때문에 필요한 경우가 아니면 수행되서는 안되는 때에 사용
- 잘 사용한다면 성능저하, 공간 낭비를 줄일 수 있음