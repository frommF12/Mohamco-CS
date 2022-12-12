# 0. 리덕스가 뭐야?

[코딩애플님의 리덕스 간단 설명](https://www.youtube.com/watch?v=QZcYz2NrDIs)
[리덕스 용어 정리](https://velog.io/@psb7391/%EB%A6%AC%EC%95%A1%ED%8A%B8-%EB%A6%AC%EB%8D%95%EC%8A%A4-%EC%A0%95%EB%A6%AC)

# 1. 리덕스 설치

리덕스를 사용하기에 앞서 리덕스를 설치한다.

> $ npm i --save redux react-redux

**`react-redux` 는 리액트 환경에서 리덕스를 사용할 수 있도록 도움을 주는 도구**

### 폴더 구조

![](https://velog.velcdn.com/images/psb7391/post/ade6088e-2961-42b9-87cf-b7f6889ba0ab/image.png)

# 2. 액션 함수 생성

## CountAction.js

```
export const setIncrease = () => ({
  type: "INCREASE",
});

export const setDecrease = () => ({
  type: "DECREASE",
});
```

상태에 어떠한 변화가 필요하게 될 때 액션을 발생시키기 위해 액션 함수를 생성한다.
액션 객체는 `type` 필드를 필수적으로 가져야 하며 그 외의 값은 상황에 따라 자유롭게 넣을 수 있다.

# 3. 리듀서 생성

리듀서는 상태에 변화를 일으키는 함수이다.
현재의 상태와 액션을 인자로 받아 새로운 상태로 업데이트한다.
즉 만들었던 `Action` 의 임무를 수행한다.

## index.js

루트 리듀서

```
import { combineReducers } from "redux";
import Count from "./CountReducer";
const rootReducer = combineReducers({
  Count,
});
export default rootReducer;
```

`combineReducer` 함수를 통해 서브 리듀서들을 통합해 루트 리듀서의 역할을 한다.

## CountReducer.js

카운터 상태 관리를 위한 서브 리듀서

```
/* 리덕스에서 관리 할 상태 정의 */
const init = {
  counter: 0,
};
export default function (state = init, action) {
  switch (action.type) {
    case "INCREASE":
      return { ...state, counter: state.counter + 1 };
    case "DECREASE":
      return { ...state, counter: state.counter - 1 };
    default:
      return state;
  }
}
```

action의 데이터를 가져오려면 action에서 정의한 객체를 `action.data` 와 같은 형식으로 가져올 수 있다.

> `{...state ~}`　를 사용하는 이유는 리덕스는 오브젝트로 이루어져있고 리액트는 오브젝트의 값을 변경할 때 깊은 복사를 해야 하기 때문이다.
> 동작이 **배열 함수인 reduce(prev, cerrent)와 유사하다** 그래서 이름도 비슷한가?

# 4. 리액트 프로젝트에 적용

## index.js

```
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import { legacy_createStore as createStore } from "redux";
import { Provider } from "react-redux";
import rootReducer from "./redux/reducer";

const root = ReactDOM.createRoot(document.getElementById("root"));
const devTools = window.__REDUX_DEVTOOLS_EXTENSION__;
const store = createStore(rootReducer, devTools && devTools());

root.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
);
```

### devTools란?

**[Redux DevTools](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?page=1&hl=ko&itemlang=en-GB)** 로 리덕스 개발자 도구이다.
현재 스토어의 상태를 개발자 도구에서 조회 할 수 있고 지금까지 어떤 액션들이 디스패치 되었는지, 그리고 액션에 따라 상태가 어떻게 변화했는지 확인 할 수 있다.. 추가적으로, 액션을 직접 디스패치 할 수도 있다.

### store란?

스토어는 현재 앱의 상태, 리듀서 및 여러 내장 함수들을 갖고 있고
스토어는 상태를 수시로 확인해 뷰한테 변경된 사항을 알려준다.

### Provider란?

리액트 앱에 `store` 를 쉽게 연결하기 위한 컴포넌트이다.

# 5. 리덕스 사용하기

## App.js

```
import { useDispatch, useSelector } from "react-redux";
import { setDecrease, setIncrease } from "./redux/actions/CountAction";

function App() {
  const dispatch = useDispatch();
  const number = useSelector((state) => state.Count.counter);

  return (
    <div>
      <h1>{number}</h1>
      <div>
        <button onClick={() => dispatch(setIncrease())}>+</button>
        <button onClick={() => dispatch(setDecrease())}>-</button>
      </div>
    </div>
  );
}

export default App;
```

### useSelector란?

`useSelector` 는 위의 `Provider` 로 감싼 store에서 state인 `counter` 를 가져온다.

### dispatch란?

`dispatch` 는 액션을 처리해 상태를 업데이트한다. 즉 액션을 발생시켜주는 함수

# 6. 결과

![](https://velog.velcdn.com/images/psb7391/post/d609db01-0630-49ee-a204-fae1f0ecb87b/image.gif)
