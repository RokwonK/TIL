## Closure

**swift에서의 클로저 람다함수와 비슷하다고 할 수 있음.(익명 함수)**  
(javascript의 Closure랑 다름)  
```swift
// 이름이 없기에 타입이 맞는 함수 어디에나 넣을 수 있다.
 { (Int, Int) -> Int in 
    //코드
 }
```

### 더 알아보기
- 후행 클로져  
    - 클로져 부분(마지막 매개변수 일때만 가능)을 뒤로 빼기
- 반환타입 생략
    - 함수에 선언된 방식에 의하여 이미 컴파일러가 클로져함수의 반환값을 알기에 생략가능
- 단축 인자 이름
    - 클로져함수의 인자를 굳이 표기안하고 넘김
- 암시적 반환 표현
    - 맨 마지막 줄이 return이다 라는 것을 알기에 안써도 됨
```swift
// 이걸 사용하는 방법
func cal(a:Int, b:Int, method:(Int,Int)->Int) -> Int {return method(a,b)}

var result : Int = 0;
// basic
result = cal(1,2, method: { (l:Int, r:Int) -> Int in return l+r } );

// 후행 클로져
result = cal(1,2){ (l:Int, r:Int) -> Int in return l+r };

// 후행 클로져 + 반환 타입 생략
result = cal(1,2){ (l:Int, r:Int) in return l+r };

// 단축 인자 이름
// $0은 1번째 인자값이라는 뜻. $1은 두번째 인자값
result = cal(1,2){ return $0 + $1 };

//암시적 반환표현
result = cal(1,2){ $0 + $1 };

```