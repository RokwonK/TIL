# Promise

자바 스크립트에서 비동기 처리에 사용되는 객체

```js
return new Promise((resolve, reject) => {
    fetch('url').then(response => resolve(resonse));
})
```

## Promise 3가지 상태
1. Pending(대기) => promise메서드를 호출시에 이 상태임
2. Fulfilled(이행) => resolve() 사용시 이행 상태가 됨 then으로 결과를 받을 수 있음
3. Rejueted(실패) => reject() 사용시 실패상태가 됨

## Promise 내장함수
1. Promise.all([1,2,3]) => 배열안의 Promise들이 전부 끝났을때까지 기다림 하나라도 오류나면 오류 반환함
