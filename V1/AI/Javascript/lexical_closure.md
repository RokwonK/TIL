# Lexical_scope 와 Closure

## Lexical_Scope (어휘적 범위 지정)
1. 말 그대로 어휘적으로 범위를 지정하는 것이다  
=> 함수가 중첩되었을때 parser가 어떻게 변수를 처리하는가?
2. 소스코드가 작성된 그 문백에 의해 결정  
=> 코드를 봤을때 그 함수가 선언된 곳을 기준
```js
function a(){
    let name = "g2";
    function b() {
        //this를 쓰면 b()의 변수를 쓰겠지만 안썻으므로 바로 상위인 a()의 변수 name을 쓴다
        console.log(name);
    }
}
```
즉, 중첩된 함수는 외부범위에서 선언한 변수에도 접근가능!



## Closure
1. 함수와 함수가 선언된 어휘적 환경의 조합, 내부함수가 외부함수의 맥락에 접근할 수 있는 것을 가르킴
2. 어렵게 생각하지말자 js의 function(arrow포함)은 Class 객체와 거의 동일
3. 함수의 인스턴스를 만드는 것(함수 하나 쓰는 인스턴스)이라 생각 즉, 멤버 변수를 사용가능하다는 뜻  
=> 여기서의 멤버변수는 name임
```js

function makefun() {
    let name = "dfdf"
    function fun() {
        console.log(name);
    }
    return fun;
}

const m = makefun(); //makefun의 인스턴스를 만드는것!
m();
```