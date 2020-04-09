# 이벤트 버블링? 


요소가 중첩되었을때 해당하는 이벤트가 중첩되어서 일어나는 현상!  
그것을 방지하는 것이 이벤트.preventDefault()

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
    // 클릭 시 상위 태그의 event도 같이 발생할 수 있음
    //        e.preventDefault로 막음
    return (
        <div onClick={newclick}>
            <button onClick={clickto}>button</button>
        </div>
    )
}

```