# Node-EventEmitter

행사 발포자라는 뜻  
Node는 입출력 처리 시 이벤트 기반(비동기)으로 처리 한다!

즉, 이벤트가 일어난 것을 듣고 실행해주는 것 (**js의 이벤트리스너**)



```js
const express = express()

//express모듈에서는 이벤트리스너를 연결하는데 use를 사용!
express.use('event_name', () => {
    //이벤트 발생시 실행시킬 함수 => 콜백함수!
})


// 기본적인 Node에서의 이벤트에미터(이벤트리스너)
// 사용될때 마다 호출
.on()
// 최초 한번만 사용하고 제거함
.once()
// 이벤트를 제거
.removeListener()

```