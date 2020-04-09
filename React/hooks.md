# Recat - Hooks

## react class component의 단점

생성주기들이 마운트될때 -   
너무 많이 사용하다 보면 얽히고 얽혀서 react의 장점인 재사용가능 컴포넌트를 잘 사용하지 못할 때가 생김


## useState

```js
import React, { useState } from 'react';

function App() {
  //변수와 설정하는 함수
  const [count, setCount] = useState(0);//변수 초기 값
  // 첫 번째 인자는 state값
  // 두번째 인자는 인자를 받아 값을 바꾸어 주는 설정 함수!!

  return (
    <div className="App">
      <header className="App-header">
        <button onClick={() => setCount(count+1)}>{count}</button>
      </header>
    </div>
  );
}

export default App;
```

## useEffect

- 클래스 컴포넌트에서 제공되던 Life Cycle APi는 useEffect로 사용가능함  
(API요청, DOM조작 등이 side effect이기 때문에 useEffect란 이름의 API가 됨)

- render가 방생할때마다 실행됨 ( componentDidMount, componentDidUpdate )  
두번째 파라미터인 [inputs]울 통해 상태가 update되었을 때만 effect가 샐행  
즉, [inputs]을 []로 넘기면 componentDidMount할때만 실행!(한번 만 실행된다는 것)


- componentWillUnmount => useEffect에 return값으로 줌(이것은 cleanup함수로 인식함)

cleanup함수란  
새로운 effect의 실행전에 매번 호출된다는뜻(말 그대로 unmount되기전에 깔끔하게 만들어준다는 것)


```javascript
import React, {useState, useEffect } from "react"
import axios from 'axios'

const App = () => {
  const [data, setData] = useState({ hits: []});
  const [query, setQuery] = useState("react");

  //해당하는 렌더링이 조간을 충족되었을때 Effect가 발동

  //어떤조건으로 어떻게 실행할 것인가
  //비동기적으로 실행됨
  useEffect(() => {
    
    let completed = false

    async function get() {
      const result = await axios(`https://hn.algolia.com/api/v1/search?query=${query}`)
      if (!completed) setData(result.data)
    }

    get()

    return () => {
      completed = true;
    }

  }, [query]) //해당하는 값이 변경되었을때 Effect함수가 실행됨
         // ui의 업데이트가 필요한가를 보는것 비어있으면 할게 ㅇ벗음


  return (
    <>
      <input value={query} onChange={e => setQuery(e.target.value)} />
      

      <ul>
        {data.hits.map(item => (
          
          <li key={item.objectId}>
            <a href={item.url}> {item.title}</a>
          </li>
        ))}
      </ul>
    </>

  )

}

export default App;
```