# 열거형 enum에 관하여

Enumeration의 약자 즉, 열거라는 뜻이다.  
열거란 `어러 가지 예나 사실을 늘어놓은 것`이다.  

Swift에서 enum안에 값들을 나열해 놓는다.  
(case 문을 사용하여 값들을 나열!)

```swift

// 각 값마다 case문을 쓰는 것, 또는 ','를 사용하여 나눈다 
enum Person : String {
    // 원시값 초기화
    case student= "학생"
    case doctor
    case teacher, child
}

var person : Person;

// person에 enum 타입을 저장해 두었으므로
// 그냥 .case값 으로 가졍로 수도 있다.
person = Person.student;
person = .student;
```

- .rawValue(원시값)를 사용하여 각 case에 저장된 값을 불러올 수 있다.(Optional 값으로 넘어옴)
- 각 case에 원시값을 초기화 시켜주지 않았다면 기본적으로 case 이름과 같은 값이 들어가있음(타입이 String인 경우)





## enum의 메서드

enum안에서 개별 메서드 구현이 가능하다.  
메서드 안에서 self를 통해 분기처리를 함!

```swift
enum Person : String {
    // 원시값 초기화
    case student= "학생"
    case doctor
    case teacher, child

    func age() -> Int{
        switch self:
            case .student return 13;
            case .doctor return 30;
            default return 1;
    }

    // 정적 메서드
    static func type() -> Int {
        return 1;
    }
}

print(Person.student.age());
print(Person.type());

```