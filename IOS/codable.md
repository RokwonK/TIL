# Codable

외부 표현을 우리가 지정한 형식으로 변환할때 사용함!  
ex) JSON 데이터를 내가 만든 객체에 맴핑해서 쓰기 쉽게 바꿈!  

Codable은 Encodable과 Decodable이 있음.  
- Encodable : 자신이 만든 객체를 외부표현(JSON)으로 바꿈.  
- Decodable : 외부표현(JSON)을 내가 만든 객체로 바꿈.  

<br>

- 참고  
Data => 바이너리 데이터로 저장 및 복원할 수 있는 타입  
"".data(using : .utf8) => utf8로 string을 바이너리형식으로 저장
```swift

// String을 바이너리 파일(Data 타입)로 바꿈!
let data : Data = """
{
    "a" : "aa",
    "c" : "bb"
}
""".data(using: .utf8)!



struct Value : Codable {
    var a : String
    var b : String

    // JSON에 있는 key를 내가 사용할 key와 매핑한다
    // 기존 json에 key c를 이 객체에서 b에 매핑한다.
    private enum CodingKeys : String, CodingKey {
        case a
        case b = "c"
    }

}

// 바이너리 파일을 json형식으로 바꿈!
let p = try! JSONDecoder().decode(Value.self, from: data)
print(p.a)
```

JSON을 주고 받을 때 형식을 지정해 두고 더 간편하게 사용가능하다!