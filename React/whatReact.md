# React를 왜?

Virtual DOM(Document Object Model)

정확히 웹 페이지에 대한 인터페이스(HTML 요소들의 구조화된 표현)

리액트는 모든 것을 view로 본다!

리액트에서는!
Only View 단방향성!
이것을 가능케 하는 것이
flux 아키텍쳐 => redux

SPA!

웹 페이지가 만들어 지는 과정
HTML 문서를 읽음 - 스타일을 입힘 - 대화형 페이지로 만들어 뷰포터에 표시 => (Critical Rendering Path)


하나 하나의 요소(객체 => 태그) 이것을 DOM으로 봄
웹 페이지의 객체 지향 표현

Reusable Components
- 재사용가능한 컴포넌트(경량화 가능!)
- react의 기반은 컴포넌트
- react의 Hook => 함수형 기반

Hot reloading
SSR지원(SPA의 검색 엔진 최적화 문제 해결SEO )


React는 Library!!!
Angular는 Framework!!!

Library => 장점 수행능력이 좋음, 
Framework =>


------------------------------
JSX와 Fragment

비워있는 태그 =>  <> </> Fragment임 하나로 감싸주기위해 사용함

--------------------------------
rendering

--------------------------------
State 와 Props
//stateless : 상태가 없는
//state : 상태!

props 다른 컴포넌트와 상호교환을 목적으로 하는 것
state 내부 컴포넌트에서의 사용만 목적! => 객체로 초기화함

-----------------------------------------
event handling
onClick={handleClick}

e.preventDefault() => 두번 이상(이벤트 버블링을 방지해줌)

버츄얼 돔에서 해당하는 렌더링 동적으로 랜더링하는 요소에 대해서 디핑하는 과정(버츄얼 돔에서 돔으로 옮기는 과정)중에 key라는 값이 필요함
랜덤하게 지으면 안댐 예측가능한 데이터를 사용하기!!!

```js
function App() {

    const newclick = () => {
        e.preventDefault()

        ///
    }

    const clickto = () => {
        e.preventDefault()

        ///
    }

    return [
        <div onClick={newclick}>

            클릭 시 상위 태그의 event도 같이 발생할 수 있음
            e.preventDefault로 막음
            <button onClick={clickto}>button</button>
        </div>
    ]
}

```

---------------------------------------

key warning 해결하기

key={item}

왜 굳이 key를 선언하게 만들었을까?


virtual DOM을 통해서 업데이트하는데
React가 자체적으로 하는게 아니라 어떠한 분기점에서 업데이트를 할 것인가를 사용자에게 기회를 주는 것


-----------------------------------------
state Hook

react component의 단점
생성주기들이 마운트될때 - 너무 많이 사용하다 보면 얽히고 얽혀서 react의 장점인 재사용가능 컴포넌트를 잘 사용하지 못할 때가 생김

함수형 프로그래밍의 장점 - reducer

import React, { useState } from 'react';
import logo from './logo.svg';
import './App.css';
```js
function App() {
  //변수와 설정하는 함수
  const [count, setCount] = useState(0);//변수 초기 값
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