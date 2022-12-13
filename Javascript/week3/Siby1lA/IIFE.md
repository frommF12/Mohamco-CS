# ![](https://velog.velcdn.com/images/psb7391/post/4cff3168-b3ab-407b-ae0c-2ad56ddd2de9/image.png)

(Immediately-Invoked Function Expression)

IIFE는 직역하면 즉시-실행 함수 표현식이다.

즉 함수를 선언함과 동시에 즉시 실행되는 함수를 나타낸다 즉시 실행 함수는 코드가 실행될 때 딱 한 번만 실행되며 그 후에는 다시 호출할 수 없고 그렇기에 초기화 기능으로 사용한다.

## 사용법

함수 표현식을 즉시 실행하는 IIFE에 사용법에는 **익명함수** **기명함수** 모두 사용 가능하다.

1. 익명함수

```
(function(){
  console.log('IIFE');
})();

// 화살표 함수로도 사용 가능하다
(() => {
    console.log("IIFE");
})();
```

2. 기명함수

```
(function init(){
  console.log('IIFE');
})();
```

실행하면 바로 "IIFE"가 출력된다. 여기서 함수는 익명함수를 사용했고 괄호로 감쌌는데, 이것은 함수 표현식으로 만들겠다는 것으로 괄호로 감싸지 않으면 함수 선언문이 되기 때문이다.

> 즉시실행함수에 기명함수를 사용해야 할까?

즉시실행함수에도 이름을 붙여 기명 즉시실행함수로 사용 할 수 있다. 하지만 즉시실행함수는 선언과 동시에 호출되어 반환되어 재사용 할 수 없기 때문에 이름을 지어주는 것이 의미가 없다.

하지만 즉시실행함수에 기명,익명 함수를 사용하는 것은 개발자들 사이에서도 의견이 갈린다.

## 사용 이유

1. 필요없는 전역 변수의 생성을 줄일 수 있다.
   함수를 생성하면 그 함수는 전역 변수로써 남아있게 되고, 많은 변수의 생성은 전역 스코프를 오염시킬 수 있다.
   즉시실행함수를 선언하면 내부 변수가 전역으로 저장되지 않기 때문에 전역 스코프의 오염을 줄일 수 있다.

2. private한 변수를 만들 수 있다.
   즉시실행함수는 외부에서 접근 할 수 없는 자체적인 스코프를 가지게된다. 이는 클로저의 사용 목적과도 비슷하며 내부 변수를 외부로부터 private하게 보호 할 수 있다는 장점이 있다.

> 클로저와 private 데이터

IIFE안에서 클로저를 생성하면 private 데이터를 만들 수 있고 외부에서 접근할 수 없다.

```
const getCount = (function(){
  let count = 1;
  return function() {
    ++count;
    return count;
  }
})();

console.log(getCount()); // 2
console.log(getCount()); // 3
```

IIFE안의 익명함수는 클로저가 되고 변수 count 는 private 데이터가 되므로 밖에 보여지지 않는다. 흔히 말하는 모듈 패턴이 바로 이 방식에 의존한다.

```
(function write() {
  var txt = "Test";
  document.write(txt);
})();

write(); // ReferenceError: write is not defined.
console.log(txt); // ReferenceError: txt is not defined.
```

자바스크립트의 클로저 때문에 IIFE안에 사용된 변수, 함수들은 모두 블럭 바깥에 영향을 줄 수 없는 것을 확인할 수 있다.
