#### store

- data를 저장하는 곳
- redux의 createStore 메서드를 통해서 생성한다.
- createStore에는 reducer라는 함수가 필요하다.
- reducer는 현재 state에 주어진 action을 한 다음 state tree 값을 리턴하는 함수이다. 
- store 안의 data를 변경하는 유일한 방법은 dispatch() 메서드를 사용하는 것이다.
- reducer가 반환하는 것이 다음 state 값이 된다. 
- state는 하나만 가능하기 때문에 여러 값을 사용하려면 하나의 state 안에서 나눠야 한다.
- action의 값은 항상 object여야 하며, object의 키값에 항상 type이 있어야 한다.

```react
import { createStore } from "redux";
const reducer = (state = initialstate, action) => {
    if(action.type === "타입")
        // 기타 실행.
    return state;    
};
const store = createStore(reducer);
store.dispatch({ type:"타입" });
```



```react
const reducer = (state = initialstate, action) => {
    switch(action.type){
        case "a": return state;
        case "b": return state;
        default: return state;
    }
};
```

- 일반적으로 if문보다 switch문을 사용하여 가독성을 높인다.



#### subscribe

- store 안의 data값에 변화가 생길때 실행된다.

```react
store.subscribe(()=> console.log("there was a change on the store."));
```



#### state

- state의 값은 변경( mutate ) 되어서는 안된다.
- 불변성을 통해 업데이트를 subscribe 하도록 하고, redux-devtoos를 통한 관리도 가능하다.

```react
const reducer = (state = [], action) =>{
    state.push({ /*추가내용*/ })
    return state
}
```

- 위 예시는 state를 직접 변경하기 때문에 틀렸다.

```react
const reducer = (state = [], action) =>{
    return [...state, { /*추가내용*/ }] 
}
```

- 위처럼 기존 state의 변경 없이 새로운 state 객체를 반환해야 한다.