# Fetch vs Axios

흔히 우리는 백엔드 또는 서드파티 API에 네트워크 요청(HTTP Requests)이 필요한 애플리케이션을 개발할 때 Axios 및 Fetch와 같은 HTTP 클라이언트를 사용한다.

## **Fetch 및 Axios에 대한 간략한 개요**

## **Fetch API**

Built-in APIs 로서 별도의 설치 없이 모던 브라우저에서 사용이 가능하다.
네트워크 요청을 위해 `fetch()` 라는 메서드를 제공하는 인터페이스이다.

## Axios

- 서드파티 라이브러리로 CDN 혹은 npm 이나 yarn과 같은 패키지 매니저를 통해 설치하여 프로젝트에 추가해야한다. ([https://axios-http.com/kr/docs/intro](https://axios-http.com/kr/docs/intro))
- Axios는 브라우저 혹은 node.js 환경에서 실행한다.

Fetch 와 axios는 모두 promise 기반의 HTTP 클라이언트 → 즉, 이 클라이언트를 이용해 네트워크 요청을 하면 이행(resolve) 혹은 거부(reject)할 수 있는 `promise`가 반환된다.

## Promise란?

<p align="center"><img width="180" alt="2" src="https://user-images.githubusercontent.com/65716445/204488251-173cd5a6-b408-4943-a60f-e248b51fd32e.png"></p>

응답 보따리이다. axios나 fetch로 요청을 보냈을 때 보따리를 피면 보낸 요청에 대한 상태 state, 결과에 대한 값 result을 확인할 수 있다.

### **Promise 간단한 원리 >**

```jsx
new Promise((성공했을 때 쓸 함수 resolve, 실패했을 때 쓸 함수 reject)) => {
		(내부처리코드)
				↓
			얘가 성공 -> resolve(성공결과 값) 돌려줘~
				↓
			얘가 실패 -> reject(실패 에러) 돌려줘~
}
```

이 객체 내에서 resolve(value)라는 함수가 호출이 되면서 상태가 pending → fulfilled(이행, 성공)으로 바뀜과 동시에 result인 resolve 함수 안에 담겼던 **value**를 돌려주고,

reject(error)라는 함수가 호출이 되면 상태가 pending → rejected(실패!)로 바뀌면서 result에 reject 함수 안에 담겼던 **error**를 돌려준다.

### **Promise 다루는 방법은?**

바로바로 then >

```jsx
프로미스
  .then(
    res => {} // fufilled 되었을 때 실행할 함수
  )
  .catch(
    err => {} // rejected 되었을 때 실행할 함수
  )
  .finally(
    () => {} // fulfilled이던 rejected던 완료 후 항상 실행
  );
```

## Fetch와 Axios의 기능 비교

## 문법

### **fetch(url, 객체)**

- 첫 번째 인자는 가져오고자 하는 리소스의 URL
- 두 번째 인자로 설정 옵션을 넘기지 않을 경우
  `fetch(url);` → 기본적으로 GET 요청을 생성한다.
- 두 번째 인자는 요청의 설정 옵션을 포함하는 객체로 선택적 인자
  - 설정 옵션을 넘기면 다음과 같이 요청에 대해 커스텀 설정할 수 있다.

```jsx
fetch(url, {
  method: 'GET', // 다른 옵션도 가능합니다 (POST, PUT, DELETE, etc.)
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({}),
});
```

### **axios(url, 객체)**

- fetch 와 비슷하나 다양한 방법으로 요청할 수 있다.

```jsx
axios(url, {
  // 설정 옵션
});
```

- 아래와 같이 HTTP 메서드를 붙일 수도 있다.

```jsx
axios.get(url, {
  // 설정 옵션
});
```

- fetch 메서드처럼 HTTP 메서드 없이 요청할 경우 기본적으로 GET 요청을 생성한다.

```jsx
axios(url);
```

- 두 번째 인자를 사용해서 커스텀 설정하는 것도 가능하다.

```jsx
axios(url, {
  method: 'get', // 다른 옵션도 가능합니다 (post, put, delete, etc.)
  headers: {},
  data: {},
});
```

- 아래처럼 작성할 수도 있다.

```jsx
axios({
  method: 'get',
  url: url,
  headers: {},
  data: {},
});
```

## JSON 데이터 처리

[JSONPlaceholder라는 REST API](https://jsonplaceholder.typicode.com/)에 GET 요청을 통해 투두 리스트의 아이템을 가져오는 예제를 통해 fetch와 Axios의 차이점을 알아보자.

### **Fetch API**

```jsx
const url = 'https://jsonplaceholder.typicode.com/todos';

fetch(url)
  .then(response => response.json())
  .then(console.log);
```

<p align="center"><img width="" alt="2" src="https://user-images.githubusercontent.com/65716445/204490483-f8eb9d80-d8d5-44b5-8f1b-951ec108a597.png"></p>

1. `fetch()`는 `.then()`메서드에서 처리된 promise를 반환
2. 이 때는 아직 우리가 필요한 JSON 데이터의 포맷이 아니기 때문에 응답 객체의 `.json()`메서드를 호출
3. 그러면 JSON 형식의 데이터로 이행(resolve)된 또 다른 promise를 반환

   → 따라서 일반적인 fetch 요청은 두 개의 `.then()`호출을 갖는다.

### **Axios**

```jsx
const url = 'https://jsonplaceholder.typicode.com/todos';

axios.get(url).then(response => console.log(response.data));
```

- axios를 사용하면 응답 데이터를 기본적으로 JSON 타입으로 사용할 수 있다.
- 응답 데이터는 언제나 응답 객체의 `data` 프로퍼티에서 사용할 수 있다.
- 다음과 같이 설정 옵션을 통해 responseType을 지정하여 기본 JSON 데이터 타입을 재정의 할 수도 있다.

```jsx
axios.get(url, {
  responseType: 'json', // options: 'arraybuffer', 'document', 'blob', 'text', 'stream'
});
```

<aside>
💡 fetch 요청은 필요한 JSON 데이터의 포맷을 얻기 위해 일반적으로 두 개의 .then() 호출을 갖는 반면, Axios 는 응답 데이터를 기본적으로 JSON 타입으로 사용할 수 있다.

</aside>

## 자동 문자열 변환(stringify)

이번엔 [JSONPlaceholder](https://jsonplaceholder.typicode.com/) API를 사용해서 데이터를 전송해보자. 이를 위해서는 **데이터를 JSON 문자열로 직렬화**해야 한다.

### **Axios**

`POST` 메서드로 JavaScript 객체를 API로 전송하면 Axios가 자동으로 데이터를 문자열로 변환해준다.

```jsx
const url = 'https://jsonplaceholder.typicode.com/todos';

const todo = {
  title: 'A new todo',
  completed: false,
};

axios
  .post(url, {
    headers: {
      'Content-Type': 'application/json',
    },
    data: todo,
  })
  .then(console.log);
```

- Axios로 `post`요청을 할 때 요청 본문(request body)으로 보내고자 하는 `data`는 data 프로퍼티에 할당한다.
- 컨텐츠 유형 헤더도 설정할 수 있는데, 기본적으로 axios는 `Content-Type`을 `application/json`으로 설정한다.

<p align="center"><img width="" alt="2" src="https://user-images.githubusercontent.com/65716445/204488269-0cd2237d-e4ed-4e45-a875-df2078375331.png"></p>

- 응답 객체 속 응답 데이터는 다음과 같이 `response.data`에 있다.

```jsx
.then(response => console.log(response.data));
```

### **Fetch API**

Fetch API를 사용한다면 `JSON.stringify()`를 사용하여 객체를 문자열으로 변환한 뒤 `body`에 할당해야 한다.

```jsx
const url = 'https://jsonplaceholder.typicode.com/todos';

const todo = {
  title: 'A new todo',
  completed: false,
};

fetch(url, {
  method: 'post',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify(todo),
})
  .then(response => response.json())
  .then(data => console.log(data));
```

- 또한 Fetch를 사용하면 명시적으로 `Content-Type`을 `application/json`으로 설정해야 한다.

<aside>
💡 fetch로 post 요청을 할 경우에는 `JSON.stringify()`를 사용하여 객체를 문자열로 변환한 뒤 본문에 할당해야하는 반면, Axios는 자동으로 데이터를 문자열로 변환해준다.

</aside>

## 에러 처리

Fetch와 axios는 모두 이행(resolve) 되거나 거부(reject)된 promise를 반환한다.Promise가 거부(reject)되면 `.catch()`를 사용하여 에러를 처리할 수 있다.

Axios로 에러를 처리하는 방법은 Fetch에 비해 더 간결합니다.

### **Axios**

```jsx
//.catch()를 사용한 일반적인 에러 처리
const url = 'https://jsonplaceholder.typicode.com/todos';

axios
  .get(url)
  .then(response => console.log(response.data))
  .catch(err => {
    console.log(err.message);
  });
```

- Axios의 promise는 상태코드가 2xx의 범위를 넘어가면 거부(reject)한다.

다음과 같이 에러 객체에 응답(response) 또는 요청(request) 프로퍼티가 포함되어 있는지 확인하여 에러에 대한 자세한 정보를 확인할 수 있다.

```jsx
.catch((err) => {
// 에러 처리
if (err.response) {
// 요청이 이루어졌고 서버가 응답했을 경우

    const { status, config } = err.response;

    if (status === 404) {
      console.log(`${config.url} not found`);
    }
    if (status === 500) {
      console.log("Server error");
    }

  } else if (err.request) {
    // 요청이 이루어졌으나 서버에서 응답이 없었을 경우
    console.log("Error", err.message);
  } else {
    // 그 외 다른 에러
    console.log("Error", err.message);
  }
});
```

- 에러 객체의 `response` 프로퍼티 → 클라이언트가 2xx 범위를 벗어나는 상태 코드를 가진 에러 응답을 받았음을 나타낸다.
- 에러 객체의 `request` 프로퍼티 → 요청이 수행되었지만 클라이언트가 응답을 받지 못했음을 나타낸다.
- 요청 또는 응답 속성이 모두 없는 경우는 네트워크 요청을 설정하는 동안 오류가 발생한 경우이다.

### **Fetch API**

Fetch는 404 에러나 다른 HTTP 에러 응답을 받았다고 해서 promise를 거부(reject)하지 않는다. Fetch는 네트워크 장애가 발생한 경우에만 promise를 거부(reject) 한다.

따라서 `.then`절을 사용해 수동으로 HTTP 에러를 처리해야 한다.

```jsx
const url = 'https://jsonplaceholder.typicode.com/todos';

fetch(url)
  .then(response => {
    if (!response.ok) {
      throw new Error(
        `This is an HTTP error: The status is ${response.status}`
      );
    }
    return response.json();
  })
  .then(console.log)
  .catch(err => {
    console.log(err.message);
  });
```

- 응답 블록에서 응답의 ok 상태가 false인 경우 → `.catch` 블록에서 처리되는 커스텀 에러를 발생시킨다.

다음과 같이 응답 객체에서 사용할 수 있는 메서드를 살펴볼 수 있다.

```jsx
.then(console.log)
```

<p align="center"><img width="" alt="2" src="https://user-images.githubusercontent.com/65716445/204488272-a20e8759-0cca-4aa3-b605-1a9c08d84037.png"></p>

- 위 사진은 fetch가 성공했을 때의 스크린샷이다.
- 만약 잘못된 URL 엔드포인트를 요청했을 경우 `ok`와 `status` 속성은 각각 `false`와 `404`값을 가지게 된다.
- 이에 에러를 발생시키고 `.catch()`절에서 커스텀 에러 메세지를 출력한다.

<aside>
💡 fetch는 HTTP 에러 응답을 받았다고 해서 프로미스를 거부하지 않고 네트워크 장애가 발생한 경우에만 거부하는 반면, Axios는 상태코드가 2xx의 범위를 넘어가면 거부한다.

</aside>

## 성능

- [measurethat.net](https://www.measurethat.net/)을 사용하여 성능을 측정할 수 있다. -> [확인해보기](https://www.measurethat.net/Benchmarks/Show/17766/0/axios-vs-fetch)
- 두 클라이언트 모두 비동기 promise 기반이기 때문에 크게 중요하지 않을 수 있지만 fetch 가 Axios 보다 살짝 더 빠르다.
- axios 요청이 많을 수록 fetch가 유리할 수는 있다.

<aside>
💡 프로젝트에서 어떤 방법을 선택할 지는 개인의 선호도와 사용 편의성에 따라 달라진다!

</aside>
