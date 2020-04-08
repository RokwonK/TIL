# Js의 Array Method들

1. forEach  
- 배열의 각각의 요소에 접근하는 것
- return값이 없음 함수 내에서 할 일을 처리해야함
```js
let data = [1,2,3]
//인자로 값, 인덱스, 배열 그 자체 값을 줌
data.forEach( (value, index, array) => console.log(value,index,array))
```

</br>

2. map
- 배열의 각각의 요소에 접근
- return 값이 있음
```js
// 원소들의 값을 3씩 증가 시켜줌
// 새로운 배열을 리턴! so, 담을 배열을 만들음
let a = data.map(x => return x+3)
```

</br>

3. filter
- 배열 안에서 값을 거르는 것
- return 해줌 / reject는 filter의 반대(false인 것만 담음)
```js
//true or false반환 함 true인 요소만 것만 담음
let a = data.filter(value => return value % 2 != 0)
```

</br>

4. every
- 배열 안의 것이 모두 true인가?
```js
data.every(value => val % 2!=0)
```

</br>

5. some
- 배열안의 값이 하나라도 true인가
```js
data.some(value => val % 2!=0)
```

</br>

6. find
- 배열안에서 값을 찾음 (가장 첫번째 요소를 반환)
```js
// 없으면 undefined
data.find(value => value > 10)
```

</br>

7. includes
- 배열에 값이 있는지 없는지()
```js
data.includes(1)
```

</br>
