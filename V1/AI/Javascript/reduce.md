# reduce()!

값을 계속 축적하면서 사용가능함!
```js
let data = [1,2,3,4,5]

// 축적값, 현재값, 현재 인덱스, 전체 배열
data.reduce ((accu,value,index,array) => {
   return accu + value
},10)// 초기 축적값 => 객체{} 배열[] 이것도 할 수 있음
```