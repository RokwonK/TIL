# Arrow function에서의 this

arrow function에서는 lexical scope의 this를 가지고 있다!  
일반 function은 dynamic scope를 사용함 => this가 자기 자신을 가르킴  
ㄴ 이것을 위해 var that = this 이렇게 바인딩 후에 더 안쪽 함수에서 that.value 이렇게 사용했음

```js

function a() {
    this.value = 0;

    const b = () => {
        console.log(this.value);
    }
    b();
}
a();
```
b안에서의 this.value 가 없으므로 그 밖의 this.value를 가져옴!