# struct의 mutating

값 타입의 경우 인스턴스가 생성되고 나서, 해당 인스턴스 Property를 변경할 수 없게 되어 있다. 그러나, 변경할 경우가 있기에 변경할 수 있도록 허락해준다는 의미에서 메서드 앞에 mutating이라는 키워드를 붙여준다. 이 메서드 안에서는 인스턴스 Propert를 변경할 수 있다.  
(class는 참조 타입, struct는 값 타입)

```swift
struct Person {
    var a = 3;
    mutating func change(ca : Int) {
        a = ca;
    }
}

var p = Person();
p.change(6);
```