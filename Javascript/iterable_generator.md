# iterable & Generator

## iterabe,iterator
배열과 문자열은 내장 이터러블임 즉, 즉, symbole안써도 알아서 해줌 즉, for(..of..)를 그냥 써도됨  
1. 이터러블 객체 : 반복가능한 객체(for으로 이어서 접근 가능하다는 뜻)(객체니까 for…of )
2. 객체를 이터러블로 만들려면(for…of가 동작하게 하려면) Symbol.iterator를 추가해야함)
3. 객체[Symbol.iterator] = function() { return{ next(){ return} }   }  
    - 반환형식은{done : Boolean, value : 값} done이 true면 반복문 끝났다는 뜻
4. 위의 것을 내장을 바꿀 수 있음
```js

//이터러블 객체로 만들기 - 이터레이터 사용
let range = {

    form : 1,

    to: 5,

    [Symbol.iterator] () {
        this.cur = this.from;
        return this;
    },

    next() {
        if (this.cur <= this.to)
            return {done:false, value:this.cur};
        else return {done:true};
    }
}

``` 

단점 : 두개를 동시에 쓰는 것이 불가능함 하나의 이터레이터를 공유하기 때문

명시적 이터레이터 사용 :  
```js
 str = "dfdf" 
 iterator = str[Symbol.iterator]();
```
</br>

## Generator
1. Function* name{} => *를 붙여서 표현   
사용자의 요구에 따라 다른 시간 간격으로 여러 값을 반환가능하며, 내부 상태를 관리할 수 있음
2. yield와 next를 통해 정지 및 다시 시작할 수 있다.
3. 이터러블을 따르는 함수(반복가능 next()로)
4. arrow function으로는 표현이 불가능 하다
```js
function* test() {
    yield 10;
    yield 20;
    yield 30;
}

const a = test();
console.log(a.next());
console.log(a.next());
console.log(a.next());
```

