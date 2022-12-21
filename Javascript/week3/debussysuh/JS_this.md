자바스크립트는 스크립트 언어로, 인터프리터에 의해 줄 단위로 읽혀 해석되어 실행된다.

인터프리터에 의해 현재 실행되는 자바스크립트의 환경을 실행 컨텍스트라고 한다. 

자바스크립트 내부에서 이러한 실행 컨택스트를 스택으로 관리하며, 실행되는 시점에 자주 변경되는 실행 컨택스트를 this가 가리킨다.

즉, this는 현재 실행되는 코드의 실행 컨텍스트를 가리킨다.

# this 키워드

---

자신이 속한 객체 또는 자신이 생성할 인스턴스(생성자함수 또는 class 이용)를 가리키는 **자기 참조 변수**

### 자기 참조 변수 (self-reference variable) ?

직역하자면 스스로를 참조하는 변수

객체 내부에서의 this는, 자신이 속한 객체를 가리킨다.

생성자함수 또는 class 내부의 this는, 인스턴스 객체를 가리킨다.

여기서 가리킨다는 다른 말로 표현하면 **바인딩(binding)**이라고 한다.

- this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.
- this는 자바스크립트 엔진에 의해 암묵적으로 생성되며, 코드 어디서든 참조할 수 있다.

[예시] - 원의 지름 구하기 ( 지름(2r) = 반지름(r)* 2)

```javascript
const circle = {
	radius: 5, // 프로퍼티: 객체 고유의 상태 데이터

	getDiameter() { // 메소드: 상태 데이터를 참조하고 조작하는 동작
		return 2 * this.radius; // thiss는 메소드를 호출한 객체를 가리킴
	}
};

console.log(circle.getDiameter()); // 10
```

- 자바스크립트의 thiss는 함수가 호출되는 방식에 따라 this에 바인딩될 값,
즉 this 바인딩이 동적으로 결정된다.

# this 바인딩

---

this 바인딩은 **this에 바인딩 될 값**으로 함수 호출 방식이다.

함수가 어떻게 호출되었는지, 호출 되는 방식에 따라 this가 어디에 바인딩 될지 동적으로 결정된다.

## 함수 호출 방식

1. 일반 함수 호출
2. 메소드 호출
3. 생성자 함수 호출 (객체 생성 함수)
4. Call, Apply, Bind 메소드 호출

### 1 일반 함수 호출

```javascript
const obj = function(){
	console.log(this); // window
}
obj();
```

- 기본적으로 this에는 전역 객체가 바인딩된다.
- 이때 this는 전역 객체를 가리킨다
- 내부 함수는 일반 함수, 메소드, 콜백 함수 어디에서 선언되었든 관계없이 this는 모두 전역 객체를 바인딩한다

### 2 메소드 호출

```javascript
var person = {
	name: 'kim',
	printName: function() {
		console.log(this.name);
	}
}

person.printName()
```

- 메소드 호출: 객체의 값이 함수로 이루어진 경우를 말함
- this: 해당 메소드를 소유한 객체에 바인딩된다.

### 3 생성자 함수 호출

```javascript
var instatnce = new obj();
```

- 생성자 함수: 객체를 생성하는 역할을 한다
- 생성자 함수의 동작 과정
    - 빈 객체 생성 및 this 바인딩
        - 생성자 함수가 실행되기 전, 빈 객체가 생성되고 이는 this로 바인딩된다
    - this를 통한 프로퍼티 생성
    - 생성된 객체 반환
- this: 함수의 인자로 전달받은 값을 객체의 속성에 할당하기 위해 this 키워드를 사용

### 4 Call, Apply, Bind 메소드 호출 - 명시적 this 바인딩

이 메소드를 통해 this를 특정 객체에 명시적으로 바인딩이 가능하다

- Call, Apply : 함수를 호출
    - 대표적으로 유사 배열 객체에 배열 메소드를 사용하는 경우에 활용
- bind : this로 사용할 객체만 전달
    - 메소드의 this와 메소드 내부의 중첩 함수 또는 콜백 함수의 this가 불일치하는 문제를 해결하기 위해 사용

```javascript
const Person = function (name) {
  this.name = name;
};
const obj = {};

Person.apply(obj, ['name']);
console.log(obj); // {name: 'name'}
```

코드에서 apply를 통해 생성자 함수 person 내부의 this에 객체 obj를 바인딩시켰다.

```javascript
const person = {
	name: 'kim',
	obj(callback){
		setTimeout(callback.bind(this),100);
  }
};
person.obj(function(){
  console.log(`my name is ${this.name}`);
});
```

여기서 bind를 사용하지 않으면 this는 person의 객체가 아닌 일반함수에서 호출되어 전역 객체를 가리키게 된다.

<aside>
💡 함수 호출에서의 this: 전역 객체에 바인딩됨
메소드 호출에서의 this: 해당 메소드를 소유한 객체에 바인딩
생성자 함수에서의 this: this는 객체 속성 할당을 위해 사용
명시적 this 바인딩: 특정 객체에 명시적으로 바인딩됨

</aside>

# 정리

1. this는 함수의 호출 방식에 따라 특정 객체를 바인딩 하게된다.
2. 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스와 바인딩된다.
3. Call, Apply, Bind 메소드 사용 시, 함수의 첫 번째 인수로 전달하는 객체에 바인딩된다.
4. Object.method 형태와 같이 객체 내에서 호출할 경우, this는 해당 객체와 바인딩된다.
5. 2,3,4를 제외한 일반적인 함수 호출의 경우, this는 전역 객체와 바인딩된다.
6. ES6의 화살표 함수 내에서 this가 사용될 경우, this는 상위 스코프의 this와 바인딩된다.

> this는 실행 문맥에 따라 결정된다.
이를 응용해 이벤트 함수가 호출되면 this는 해당 이벤트 객체에 할당되겠구나 를 쉽게 파악할 수 있다.
> 

참고문헌

[https://velog.io/@edie_ko/js-this](https://velog.io/@edie_ko/js-this)

[https://velog.io/@danmin20/자바스크립트-this-바인딩이란](https://velog.io/@danmin20/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-this-%EB%B0%94%EC%9D%B8%EB%94%A9%EC%9D%B4%EB%9E%80)

[https://velog.io/@syoon624/JS-Deep-Dive-this](https://velog.io/@syoon624/JS-Deep-Dive-this)