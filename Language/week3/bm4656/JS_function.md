# 🔥 함수

> 함수란 **어떤 작업을 수행**하기 위해 **필요한 문(statement)들의 집합**을 정의한 코드 블록

- 유지보수성, 재사용성, 높은 가독성
- **함수는 데이터가 아닌 하나의 특별한 값**이고 **그것을 할당**할 수 있다.

## 💡 함수의 정의와 호출

- 함수의 정의 방식 3가지
  1. 함수 선언문
  2. 함수 표현식
  3. Function 생성자 함수

## 💡 1. 함수 선언문

```jsx
//함수 선언
funtion 함수명(매개변수=인자, 콤마로, 구분) {
	//함수 본문
	return 반환 값;
};

//함수 호출
함수명(매개변수);
```

- 함수명 생략 X
- `return`문으로 결과값을 반환 가능 → 반환값(return value)

## 💡 2. 함수 표현식

- 함수의 **일급 객체** 특성을 이용하여 **함수 리터럴 방식으로 함수를 정의하고 변수에 할당**하는 방식

```jsx
var square = function (number) {
  return number * number;
};
```

- 함수명 생략 가능→ **익명 함수**
- 함수 표현식에선서는 함수명 생략이 일반적이다.

```jsx
// 기명 함수 표현식(named function expression)
var foo = function multiply(a, b) {
  return a * b;
};

// 익명 함수 표현식(anonymous function expression)
var bar = function (a, b) {
  return a * b;
};

console.log(foo(10, 5)); // 50
//기명함수의 함수명으로 호출 시 에러뜸
console.log(multiply(10, 5)); // Uncaught ReferenceError: multiply is not defined
```

> ‼️ **주의할점** ‼️
>
> - 할당된 변수는 함수명이 아니라 **할당된 함수를 가르키는 참조값을 저장**한다.
> - 함수가 할당된 변수를 사용해 함수를 호출하지 않고 기명 함수의 함수명을 사용해 호출하게 되면 에러가 발생!
>   → 함수 표현식에서 사용한 함수명은 외부 코드에서 접근 불가능하기 때문
> - 그렇다면 함수 선언문에서는??
>   → 자바스크립트 엔진에 의해 함수표현식으로 형태가 변경되었기 때문!
> - **결국 함수 선언문도 함수 표현식과 동일하게 함수 리터럴 방식으로 정의되는 것이다.**

## 💡 3. \***\*Function 생성자 함수\*\***

- Function.prototype.constructor 프로퍼티로 접근
  `new Function(arg1, arg2, ... argN, functionBody)`
- 함수 선언문과 함수 표현식의 함수 리터럴 방식
  → 이는 \*\*\*\*결국 내장 함수 Function 생성자 함수로 함수를 생성하는 것을 단순화시킨 short-hand(축약법)
- **Function 생성자 함수로 함수를 생성하는 방식은 일반적으로 사용하지 않는다.**

---

# 🔥 함수도 객체, 근데 이제 일급 객체를 곁들인….

- **자바스크립트에서 함수는 값으로 취급된다.**
- 함수가 객체이므로, 함수도 일반 객체처럼 취급될 수 있다.

## 🔆 메모리 주소를 '참조'한다.

- 함수의 object도 heap 메모리에 저장
- 함수의 이름은 함수 오브젝트가 담겨있는 메모리 주소를 가르키고 있다.**(=함수의 객체 주소를 가지고 있다.)**
- 함수를 다른 변수에 할당하면? → **같은 메모리주소(=객체)를 참조한다.**

```jsx
function add(a, b) {
  return a + b;
}
const sum = add;

console.log(add(1, 1)); //2
console.log(add(1, 1)); //2
```

## 🔆 일급 객체

- 함수 또한 일반 객체처럼 모든 연산이 가능한 것을 말한다.
- 함수와 다른 객체를 구분짓는 특징은 호출할 수 있다는 것이다.

> 일급 객체 조건
>
> 1. 무명의 리터럴로 표현이 가능하다.
> 2. 변수나 자료 구조(객체, 배열 등)에 저장할 수 있다.
> 3. 함수의 매개변수에 전달할 수 있다.
> 4. 반환값으로 사용할 수 있다.

## 🔆 매개변수(Parameter, 인자)

- 함수의 작업 실행을 위해 추가적인 정보 매개변수를 지정
- 매개변수(parameter)를 이용하면 **임의의 데이터를 함수 안에 전달**할 수 있다.
- 매개변수는 함수 내에서 변수와 동일하게 동작

### 🌼 **parameter = 매개변수 = 변수 = 인자**

```jsx
function add(a, b) {
  return a + b;
}
// 매개변수 a, b
```

- **함수 내부에 있는 인자**로써, 특정한 값으로 정해져 있는 것이 아니라, 함수가 호출하면서 건네준 argument의 값이 **변수(variable)**에 담기게 된다.
- 들어오는 인자가 매개체 역할을 하기 때문에 **매개변수**라고도 하며 영문으로는 **parameter**

### 🌼 argument = 전달인자 = 값 = 인수

```jsx
add(1, 2);

// 전달인자=인수 1, 2
```

- **함수를 호출할 때** 값을 전달한다고 해서 **전달인자**라고도 부른다.
- 매개변수와 달리 전달인자는 고정되어 있지 않고, 호출할 때마다 수시로 변하는 값(Value)이기 때문에 변수가 아닌 **값(Value)**으로 정의한다.

### 💦 \***\*매개변수(parameter, 인자) vs 인수(argument)\*\***

- **매개변수**는 함수 선언 방식 괄호 사이에 있는 변수(선언 시 쓰이는 용어).
- **인수**는 **함수를 호출할 때 매개변수에 전달되는 값**(호출 시 쓰이는 용어).
  → 즉, **함수 선언 시 매개변수를 나열하게 되고, 함수를 호출할 땐 인수를 전달해 호출한다!**
- 함수 호출 시 매개변수에 인수를 전달하지 않으면 그 값은 `undefined`
- 함수 선언 시 매개변수에 '기본값(default value)’ 지정 가능

```jsx
// 매개변수의 기본값은 무조건 undefined
// 매개변수의 정보는 함수 내부에서 접근이 가능한 arguments 객체에 저장됨
// 매개변수 기본값 Default Parameters a = 1, b = 1
function add(a = 1, b = 2) {
  console.log(a);
  console.log(b);
  console.log(arguments);
  console.log(arguments[1]);
  return a + b;
}
add();

/* 출력
1
1
[Arguments] {}
undefined
*/
```

## 🔆 콜백 함수

- 함수를 함수의 인수로 전달하고, 필요하다면 인수로 전달한 그 함수를 "나중에 호출(called back)"하는 것이 콜백 함수
- **고차함수**
  - 인자로 함수를 받거나(콜백함수)
  - 함수를 반환하는 함수

```jsx
// 콜백함수
const boy = name => console.log(`🙇‍♂️ Hi ${name}!`);
const girl = name => console.log(`🙇‍♀️ Hi ${name}!`);

// 전달된 action -> 콜백함수
// 전달될 당시에 함수를 바로 호출해서 반환된 값을 전달하는 것이 아니라
// 함수를 가리키고 있는 함수의 레퍼런스(참조값) 전달!
// 따라서 함수는 고차함수안에서 **필요한 순간에 호출**이 나중에 된다.
function welcome(name, action) {
  if (typeof name !== 'string') {
    console.log('wrong name type!');
    return;
  }
  let result = action(name);
  return result;
}

welcome('John', boy); //🙇‍♂️ Hi John!
welcome('Reira', girl); //🙇‍♀️ Hi Reira!
```

- 콜백함수는 외부로부터 전달받아서 나중에 함수안에서 호출하는 것
- 함수를 호출하지 않고(not `boy()`) 함수가 가리키는 **주소**를 전달

## 🔆 반환값 return

- 함수는 자신을 호출한 코드에게 수행한 결과를 반환(return)할 수 있다. → **반환값(return value)**
- 배열 등을 이용하여 한 번에 여러 개의 값을 리턴 가능
- `return` 생략 시 `undefined` 반환
- **그냥 `return`만 있을 경우, 함수가 즉시 종료**
- 만일 `return` 이후에 다른 구문이 존재할 경우, 그 구문은 실행되지 않는다.

---

✨ Post by Bomin Kim
[더 추가된 내용의 블로그 글로 가기](https://velog.io/@newjin46/JS-%ED%95%A8%EC%88%98-%EC%9D%BC%EA%B8%89-%EA%B0%9D%EC%B2%B4-%EB%A7%A4%EA%B0%9C%EB%B3%80%EC%88%98%EC%99%80-%EC%9D%B8%EC%88%98-%EC%BD%9C%EB%B0%B1%ED%95%A8%EC%88%98)
