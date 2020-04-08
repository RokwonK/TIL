## ES5와 그 이후...

</br>

1. **템플릿 리터럴( ` ` )이 생김!**
```js
let name = "rokwon";

// 백틱을 사용하여 문자열 안에서 변수를 사용할 수 있게 됨
// ${ } => 문자열 인터폴레이션
// *인터폴레이션(보간법) = 알려진 데이터 지점안에서 새로운 데이터 지점을 구성하는 방식
console.log(`hello ${name}`);
```

</br>

2. **Arrow Function**의 등장

* this를 사용할 수 없음! (사용한다면 무조건 전역이 되는 것!)
```js
// 기존 function보다 짧게 가능!
// 한 줄일 경우 return 안써줘도 됨
// 두줄 이상일 경우{ }로 감싸줌
const func = () => console.log("arrow")
```

</br>

3. **this**

- 생성자 함수(new로 만든 객체)와 선언식 함수에 따라 역할이 달라짐

```js
const value = 3;

// 생성자 함수일때
function a(){

    //여기서의 this는 객체(생성된 인스턴스 자체를 가르킴)
    console.log(this.value)
}
// aaa의 인터스턴스 변수 라는 것!
let aaa = new a();


// 선언식 함수일때
function b(){
    // 함수를 호출한 객체 자체가 this
    // 그 뜻은 이 함수 바로 바깥의 블록 이라는 뜻
    // 여기선 전역이므로 바깥 맨 위 선언된 value
    // console.log(this.value)

    // 선언식 함수에서는 this가 전역에 해당
    // this를 쓰지 않는다면 클로져 함수가 되는 것!
    return function() {
        console.log(this.value)
    }
}

// 바인드 할 수 있음
const obj = {value : 10}
// obj에 b()를 바인딩(빌려줌)
b().call(obj);
```

</br>

4. **변수 선언에 let, const, var**  
- let은 블록변수 const는 상수로!

</br>

5. **모듈화**  
- 기존에는 하나의 스크립트 파일을 모듈화가 불가능했음(코드의 재활용이 힘들음)

    import로 불러오고  
    export로 아웃풋 모듈을 만들 수 있게 되엇음
```js
// Node.js
//      모든 것을 가져오고 a라는 이름으로 사용한다
import * as a from "./modules__"
import a from "dfdf"

//      모듈안에서 b와 c를 그대로 가져와서 그이름대로 사용한다
import {b,c} from "___"
//


// ES6
const a = require("./mudules__");



//module.exports 와 exports 둘다 동일한 객체를 바라보고 있음
//또 반환 값은 항상 module.exports 다 exports 는 module.exports 를 참조하는 형태임 exports 자체는 //반환되지 않음!
// exports와 export는 다름!
// exports와 module.exports와 연결됨

// export는 그냥 다른 아웃풋 구현 방법
// 즉, exports와 module.exports가 하나
// export와 export default

// 1
//  1-1
exports = module.exports = {}
exports.add = {무엇을 넣어줌}

//  1-2
module.exports 객체


// 2
//  2-1
 export default '모듈' // import 시 {}안하고 불러 올 수 있음 따로가져올려면!

//  2-2
 export a             // import 시 {a}라고써야함
 export fucntion
```

</br>

6. **Class**가 생김  
- 부모로부터 상속 및 prototype 이 아닌 클래스 객츠를 사용함
    

