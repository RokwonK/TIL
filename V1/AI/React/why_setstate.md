# 왜 setState API를 따로 만들었을까?

자바스크립트는 피연산자의 값이 아닌 reference 값을 기준으로 true/false를 리턴함  
```js
a = 3; b = 3 
a === b false


a = 3; b = a 
 a === b true
```

즉, 그냥 state값을 직접 변경하면 값만 바뀔뿐 reference가 바뀌는 것이 아님

setstate는 비동기로 동작  
하지만 promise객체를 반환하는 것이 아니어서 async, await을 사용할 수 없음  
즉, callback함수를 사용해야함 (다음 이어지는 동작을 원할 시)