# var vs let vs const

과거의 ES5까지는 var 키워드로만 변수 선언이 가능하였는데 여러 문제점이 있는 var의 단점을 보완하기 위해 ES6가 등장하면서 let과 const키워드를 도입하였다.

`var`, `let`, `const` 는 변수를 선언하는 키워드다.

JavaScript에서 사용하는 이 세가지 변수 선언 키워드의 차이점에 대해 소개하고자 한다.

## 1. 변수 중복 선언 금지

`var` 키워드로는 동일한 이름을 갖는 변수를 중복해서 선언할 수 있었다.
하지만, `let` 키워드로는 동일한 이름을 갖는 변수를 중복해서 선언할 수 없다.

### 1.1. 스코프(Scope) 란?

<span style="color: red">**스코프**</span> 는 참조 대상 식별자(identifier, 변수, 함수의 이름과 같이 어떤 대상을 다른 대상과 구분하여 식별할 수 있는 유일한 이름)를 찾아내기 위한 규칙이다.

대부분의 프로그래밍 언어들은 블록 레벨 스코프(Block-level scope)를 따르지만 <span style="background-color: #F2E2A2">&nbsp;자바스크립트는 함수 레벨 스코프(Function-level scope)를 따른다.&nbsp;</span>

- `var` : 함수 레벨 스코프
- `let`, `const` : 블록 레벨 스코프

**함수 레벨 스코프(Function-level scope)**
함수 내에서 선언된 변수는 함수 내에서만 유효하며 함수 외부에서는 참조할 수 없다. 즉, 함수 내부에서 선언한 변수는 지역 변수이며 함수 외부에서 선언한 변수는 모두 전역 변수이다. <br>
**블록 레벨 스코프(Block-level scope)**
모든 코드 블록(함수, if 문, for 문, while 문, try/catch 문 등) 내에서 선언된 변수는 코드 블록 내에서만 유효하며 코드 블록 외부에서는 참조할 수 없다. 즉, 코드 블록 내부에서 선언한 변수는 지역 변수이다.

### 1.2. 스코프의 특징

```javascript
var foo = 123; // 전역 변수

console.log(foo); // 123
{
  var foo = 456; // 전역 변수
}
console.log(foo); // 456
```

위 코드 내에서 보면 var 키워드를 사용하여 선언한 변수는 중복 선언이 허용되므로 위의 코드는 문법적으로 아무런 문제가 없다. 단, 코트 블록 내에 변수 foo는 전역 변수이기 때문에 전역에서 선언된 전역 변수 foo의 값 123을 새로운 값 456으로 재할당하여 덮어씌운다.

ES6는 블록 레벨 스코프를 따르는 변수를 선언하기 위해 `let` 키워드를 제공한다.

```javascript
function run() {
  var foo = "Foo";
  let bar = "Bar";

  console.log(foo, bar);

  {
    let baz = "Bazz";
    console.log(baz);
  }

  console.log(baz); // ReferenceError
}

run();
```

따라서, 위 코드를 실행했을 때 블록 안에 있는 baz를 출력하게 되면 `참조 에러(ReferenceError)`가 발생하는 것이다.

<br>

## 2. 호이스팅(Hoisting)

<span style="color: red">**호이스팅(Hoisting)**</span> 은 var 선언문이나 function 선언문 등을 해당 스코프의 선두로 옮긴 것처럼 동작하는 특성을 말한다.

- `var` 는 함수 스코프의 최상단으로 호이스팅 되고 선언과 동시에 `undefined` 로 초기화 된다.
- `let` 과 `const` 는 블록 스코프의 최상단으로 호이스팅 되고 선언만 되고 값이 할당되기 전까지 어떤 값으로도 초기화되지 않는다.

변수는 `선언 단계` > `초기화 단계` > `할당 단계` 3단계에 걸쳐 생성된다.

### 2.1. var 키워드 동작

`var` **키워드로 선언된 변수는 선언 단계와 초기화 단계가 한번에 이루어진다.**
즉, 스코프에 변수를 등록(선언 단계)하고 메모리에 변수를 위한 공간을 확보한 후, undefined로 초기화(초기화 단계)한다.

따라서 변수 선언문 이전에 변수에 접근하여도 스코프에 변수가 존재하기 때문에 에러가 발생하지 않는다. 다만 `undefined` 를 반환한다. 이후 변수 할당문에 도달하면 비로소 값이 할당된다. 이러한 현상을 <span style="background-color: #F2E2A2">&nbsp;변수 호이스팅(Variable Hoisting)&nbsp;</span>이라 한다.

```javascript
function run() {
  console.log(foo); // undefined
  var foo = "Foo";
  console.log(foo); // Foo
}

run();
```

따라서, `var` 의 경우 위와 같이 선언 전에 출력하면 `undefined` 가 출력된다.

### 2.2. let 키워드 동작

`let` **키워드로 선언된 변수는 선언 단계와 초기화 단계가 분리되어 진행된다.**
즉, 스코프에 변수를 등록(선언단계)하지만 초기화 단계는 변수 선언문에 도달했을 때 이루어진다.

초기화 이전에 변수에 접근하려고 하면 `참조 에러(ReferenceError)` 가 발생한다. 이는 변수가 아직 초기화되지 않았기 때문이다. 따라서 스코프의 시작 지점부터 초기화 시작 지점까지는 변수를 참조할 수 없다

이런 스코프의 시작 지점부터 초기화 시작 지점까지의 구간을 <span style="background-color: #F2E2A2">&nbsp;일시적 사각지대(Temporal Dead Zone; TDZ)&nbsp;</span> 라고 부른다.

```javascript
function checkHoisting() {
  console.log(foo); // ReferenceError
  let foo = "Foo";
  console.log(foo); // Foo
}

checkHoisting();
```

`let` 의 경우는 선언 전에 호이스팅 되긴 하지만 어떤 값도 가지지 않기 때문에 `ReferenceError` 가 발생한다. 이런 현상이 `TDZ` 이다. 즉, 선언은 되었지만 참조는 할 수 없는 사각지대를 갖는 것이다.
<br>

## 3. 글로벌 객체로의 바인딩

**strict mode가 아니라는 가정 하에,**

- `var` 는 글로벌 스코프에서 선언되었을 경우 글로벌 객체에 바인딩된다.
- `let` 과 `const` 는 글로벌 스코프에서 선언되었을 경우 글로벌 객체에 바인딩되지 않는다.

```javascript
var foo = "Foo"; // globally scoped
let bar = "Bar"; // globally scoped

console.log(window.foo); // Foo
console.log(window.bar); // undefined
```

브라우저 환경에서 글로벌 객체는 **window**인데, `var` 의 경우 바인딩이 되었고 `let` 의 경우는 되지 않았다는 걸 볼 수 있다.
<br>

## 4. 재선언(Redeclaration)

- `var` 는 재선언이 가능하다.
- `let` 과 `const` 는 재선언이 불가능하다.

```javascript
var foo = "foo1";
var foo = "foo2"; // 문제없음

let bar = "bar1";
let bar = "bar2"; // SyntaxError: Identifier 'bar' has already been declared
```

`let` 키워드로 재선언을 할 경우, `SyntaxError` 에러가 발생한다.
<br>

## 5. let vs const

- `var` 와 `let` 은 재할당이 가능하다.
- `const` 는 선언과 초기화가 반드시 동시에 일어나야 하며 재할당이 불가능하다. 즉, 상수와 같은 고정값을 선언할 때 사용하는 키워드이다.
  <br>

## 정리

1. `var` 는 함수 레벨 스코프이며, `let` 과 `const` 는 블록 레벨 스코프이다.

2. `var` 는 스코프의 선두에서 선언 단계와 초기화 단계가 실행되어 변수 선언문 이전에 변수를 참조할 수 있지만, `let` 은 스코프의 선두에서 선언 단계가 실행되어 아직 변수가 초기화되지 않아 변수 선언문 이전에 변수를 참조할 수 없다

3. 글로벌 스코프에서 선언되었을 경우`var` 은 글로벌 객체에 바인딩이 되고, `let` 과 `const` 는 글로벌 객체에 바인딩이 되지 않는다.

4. `var` 는 재선언이 가능하고, `let` 과 `const` 는 불가능하다.

5. `var` 와 `let` 은 재할당이 가능하고, `const`는 불가능하다.
