# Flux

mvc패턴을 문제점을 발견하고 해결하기 위한!  

단방향 아키텍쳐

action( CRUD연산들 ) -> store( 데이터 저장소 ) -> React -> 다시 action으로

이러한 상태변화유발을 side effect라고 함 
action하는 곳은 곳곳에 흩어져 있을텐데 그럼에도 불구하고 단방향 흐름을 유지할 수 있을까?  
- Reducers



데이터의 불변성을 유지하기 위해서 reducer를 사용  
기존의 데이터에대한 수정없이 새로운 데이터를 반환!

--------------------------------------------
redux 개념적 접근

단일 store를 사용함
새로운 데이터를 다시 store에 저장

- one immutable store(한가지 불변적인 store)
- Actions trigger changes( action이 상태변화를 유도한다 )

reducer => 변수의 값을 변화시키는게 아니라 새로운 값을 만들어 업데이트(불변성)
기존 state를 받고 요청한 action을 받음
병합해서 새로운 상태를 만들어 업데이트

----------------------------------------------

react-router-dom => route, switch(스위치문과 같음!), redirect

exact path to component 

-----------------------------------------------

styled-components

css에 javascript를 넣을 수 있다는 것!
조건문 css렌더링

-----------------------------------------------

side effect => 상태변화를 유발하는 모든 요소들
관리하는 모듈 - redux-saga 다양한 메서드 지원

-----------------------------------------------
stateless function => 클래스없이 함수로만 이루어진 함수

