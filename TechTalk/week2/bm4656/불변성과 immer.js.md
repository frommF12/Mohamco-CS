# immer.js

- React에서 불변성을 유지하는 코드를 작성하기 쉽게 해주는 라이브러리
- React에서 배열이나 객체를 업데이트 해야 할 때에는 직접 수정하면 안되고 불변성을 지켜주면서 업데이트를 해주어야 한다.

## React 기본 속성

> 기본적으로 리액트는 부모 컴포넌트가 리렌더링을 하면 자식 컴포넌트도 리렌더링하게 된다. 즉, 얕은 비교를 통해 새로운 값인지 아닌지를 판단한 후 새로운 값인 경우 리렌더링을 하게 된다. 여기서 얕은 비교란 객체, 배열, 함수와 같은 참조 타입들을 실제 내부 값까지 비교하지 않고 동일 참조인지(동일한 메모리 값을 사용하는지)를 비교하는 것을 뜻한다.

- state가 얕은 비교를 통해 컴포넌트가 리렌더링 된다. → react에서 요소를 직접 변경하면 안된다.

1. 우리가 컴포넌트를 리렌더링 해야하는 상황이 있다고 가정하고, 타입이 배열인 state를 바꾼다.
2. 이때, `state.push(1)`을 통해 state 배열에 직접 접근하여 요소를 추가한다.
3. 우리는 push 전과 다른 값이라고 생각하지만, 리엑트는 state라는 값은 **새로운 참조값이 아니기 때문에** 이전과 같은 값이라고 인식하고 리렌더링 하지 않는다.

- 즉, 위 이유로 우리가 state를 바꾸고 돔을 다시 만들려면, 새로운 객체 or 배열을 만들어 새로운 참조값을 만들고, react에게 이 값은 이전과 다른 참조값임을 알려야한다.

> 위 과정은 가상 dom에서만 이뤄지는 렌더링이며, 렌더링을 마치면 react의 비교 알고리즘에 의해 변화가 일어난 컴포넌트만 실제로 업데이트되어 우리 눈에 보이게 되는 것이다.

## **불변성 지키면서 state 바꾸기**

### **배열에 추가**

```jsx
setUsers(state.array.concat(user));
```

### **배열에서 삭제**

```jsx
const onRemove = id => {
  // user.id 가 id 인 것을 제거
  setUsers(users.filter(user => user.id !== id));
};
```

### **배열에서 수정**

```jsx
const onToggle = id => {
  setUsers(
    users.map(user =>
      user.id === id ? { ...user, active: !user.active } : user
    )
  );
};
```

### **객체에서 추가**

```jsx
setState(state => {...state, key: value})

```

### **객체에서 제거**

```jsx
setState(state => {..._.omit(state, 'deleteKey')})
```

### **객체에서 수정**

```jsx
setState(state => {...state, key: newValue})
```

- 위에 방법대로 배열 혹은 객체를 바꾸면 불변성을 유지하면서 변경이 가능하다.
- 대부분의 경우 ... 연산자 또는 배열 내장 함수를 사용하는건 그렇게 어렵지는 않지만 데이터의 구조가 조금 까다로워지면 불변성을 지켜가면서 새로운 데이터를 생성해내는 코드가 조금 복잡해진다.

## **immer**

- Immer 를 사용하면 우리가 상태를 업데이트 할 때, 불변성을 신경쓰지 않으면서 업데이트를 해주면 Immer 가 불변성 관리를 대신 해준다.

```jsx
import produce from 'immer';

const baseState = [
  {
    todo: 'Learn typescript',
    done: true,
  },
  {
    todo: 'Try immer',
    done: false,
  },
];

const nextState = produce(baseState, draftState => {
  draftState.push({ todo: 'Tweet about it' });
  draftState[1].done = true;
});
```

- immer에서 우리가 쓸 함수 `produce`만 알면 된다.
- 2가지의 param을 가져오고 첫번째는 **수정하고 싶은 객체/배열**, 두번째는 첫번째 파라미터에 할당된 객체/배열을 **바꾸는 함수**이다.

## **redux에서 Immer 쓰기**

- immer 쓰기 전 reduce 코드

```jsx
const initialState = [{ name: 'nkh', address: { city: 'seoul' } }];

export default function auth(state = initialState, action) {
  switch (action.type) {
    case SET_INFO:
      return {
        ...state,
        name: 'kkkk',
        address: {
          ...state.address,
          city: 'kkkk',
        },
      };
    default:
      return state;
  }
}
```

- immer 쓴 reduce 코드

```jsx
const initialState = [{ name: 'nkh', address: { city: 'seoul' } }];

export default function auth(state = initialState, action) {
  produce(state, draft => {
    switch (action.type) {
      case SET_INFO:
        draft[0].name = action.data.name;
        draft[0].address.city = action.data.city;
        break;
      case ADD_INFO:
        draft.push({ name: 'hhh', address: { city: 'zzz' } });
      default:
        return draft;
    }
  });
}
```

- immer 공식 예제

```jsx
const byId = (state, action) =>
  produce(state, draft => {
    switch (action.type) {
      case RECEIVE_PRODUCTS:
        action.products.forEach(product => {
          draft[product.id] = product;
        });
        break;
    }
  });

const byId = (state, action) => {
  switch (action.type) {
    case RECEIVE_PRODUCTS:
      return {
        ...state,
        ...action.products.reduce((obj, product) => {
          obj[product.id] = product;
          return obj;
        }, {}),
      };
    default:
      return state;
  }
};
```

> 참고자료
> [https://react.vlpt.us/basic/23-immer.html](https://react.vlpt.us/basic/23-immer.html)
> [https://kyounghwan01.github.io/blog/React/immer-js/#불변성-지키면서-state-바꾸기](https://kyounghwan01.github.io/blog/React/immer-js/#%E1%84%87%E1%85%AE%E1%86%AF%E1%84%87%E1%85%A7%E1%86%AB%E1%84%89%E1%85%A5%E1%86%BC-%E1%84%8C%E1%85%B5%E1%84%8F%E1%85%B5%E1%84%86%E1%85%A7%E1%86%AB%E1%84%89%E1%85%A5-state-%E1%84%87%E1%85%A1%E1%84%81%E1%85%AE%E1%84%80%E1%85%B5)
