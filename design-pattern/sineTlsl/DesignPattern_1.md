# 디자인 패턴이란?

<span style="background-color: #f0baaa">&nbsp;**디자인 패턴 (Design Pattern)**&nbsp;</span> 이란 프로그램을 설계할 때 발생했던 문제점들을 객체 간의 상호 관계 등을 이용하여 해결할 수 있도록 하나의 '규약' 형태로 만들어 놓은 것을 의미한다.

## 1. 싱글톤 패턴

> `싱글톤 패턴 (Singleton Pattern)` 은 하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴이다.

하나의 클래스를 기반으로 단 하나의 인스턴스를 만들어 이를 기반으로 로직을 만드는 데 쓰이며, 보통 데이터베이스 연결 모듈에 많이 사용한다.

하나의 인스턴스를 다른 모듈과 공유하며 사용하기 때문에 인스턴스를 생성할 때 드는 비용이 줄어드는 <span style="color: #18222D">**장점**</span>이 있지만, 의존성이 높아진다는 <span style="color: #18222D">**단점**</span>이 있다.

### 1.1. 자바스크립트의 싱글톤 패턴

자바스크립트에서는 리터럴 `{}` 또는 `new Obejct` 로 객체를 생성하게 되면 다른 어떤 객체와도 같지 않기 때문에 이 자체만으로 싱글톤 패턴을 구현할 수 있다.

```javascript
const obj = {
  a: 27,
};
const obj2 = {
  a: 27,
};
console.log(obj === obj2); // false
```

obj와 obj2는 다른 인스턴스를 가지며, 이 예제도 싱글톤 패턴이라 볼 수 있지만 실제 싱글톤 패턴은 아래와 같은 코드로 구성된다.

```javascript
class Singleton {
  constructor() {
    if (!Signleton.instance) {
      Signleton.instance = this;
    }
    return Singleton.instance;
  }
  getInstance() {
    return this.instance;
  }
}
const a = new Singleton();
const b = new Singleton();
console.log(a === b); // true
```

`Singleton.instance` 라는 하나의 인스턴스를 가지는 `Singleton` 클래스를 구현한 모습이며, 이를 통해 a와 b는 하나의 인스턴스를 가진다.

**싱글톤 패턴은 데이터베이스 연결 모듈에 많이 사용된다. **

```javascript
// DB 연결을 하는 것이기 때문에 비용이 더 높은 작업
const URL = "mongodb://localhost:27017/kundolapp";
const createConnection = (url) => ({ url: url });
class DB {
  constructor(url) {
    if (!DB.instance) {
      DB.instance = createConnection(url);
    }
    return DB.instance;
  }
  connect() {
    return this.instance;
  }
}
const a = new DB(URL);
const b = new DB(URL);
console.log(a === b); // true
```

`DB.instance` 라는 하나의 인스턴스를 기반으로 a, b를 생성할 수 있다. 이롤 통해 데이터베이스 연결에 관한 인스턴스 생성 비용을 아낄 수 있다.

### 1.2. 싱글톤 패턴의 단점

싱글톤 패턴은 `TDD(Test Driven Development)` 를 할 때 걸림돌이 된다. `TDD` 를 할 때 단위 테스트를 주로 하는데, 단위 테스트는 테스트가 서로 독립적이어야 하며 테스트를 어떤 순서로든 실행할 수 있어야 한다.

미리 생성된 하나의 인스턴스를 기반으로 구현하는 싱글톤 패턴은 **각 테스트마다 독립적인 인스턴스를 만들기가 어렵다.**

### 1.3. 의존성 주입

<span style="background-color: #f0baaa">&nbsp;**의존성**&nbsp;</span> 이란 종속성이라고도 하며 A가 B에 의존성이 있다는 것은 B의 변경 사항에 대해 A 또한 변해야 된다는 것을 의미한다.

싱글톤 패턴은 사용하기가 쉽고 굉장히 실용적이지만 **모듈 간의 결합을 강하게 만들 수 있다는 단점**이 있다. `의존성 주입(DI, Dependency Injection)` 을 통해 모듈간의 결합을 조금 더 느슨하게 만들어 해결할 수 있다.

**1) 의존성 주입의 장점**
모듈들을 쉽게 교체할 수 있는 구조가 되어 테스팅하기 쉽고 마이그레이션하기도 수월하다.구현할 때 추상화 레이어를 넣고 이를 기반으로 구현체를 넣어 주기 때문에 애플리케이션 의존성 방향이 일관되고, 애플리케이션을 쉽게 추론할 수 있으며 모듈 간의 관계들이 더 명확해진다.

**2) 의존성 주입의 단점**
모듈둘아 더욱 더 분리되므로 클래스 수가 늘어나 복잡성이 증가될 수 있으며 약간의 런타임 페널티가 생기기도 한다.

**3) 의존성 주입 원칙**
의존성 주입은 "상의 모듈은 하위 모듈에서 어떠한 것도 가져오지 않아야 한다. 또한 둘 다 추상화에 의존해야 하며, 이때 추상화는 세부 사항에 의존하지 말아야 한다." 라는 의존성 주입 원칙을 지켜주면서 만들어야 한다.

<br>

## 2. 팩토리 패턴

> `팩토리 패턴 (Factory Pattern)` 은 객체를 사용하는 코드에서 객체 생성 부분을 떼어내 추상화한 패턴이자 상속 관계에 있는 두 클래스에서 상위 클래스가 중요한 뼈대를 결정하고, 하위 클래스에서 객체 생성에 관한 구체적인 내용을 결정하는 패턴이다.

### 2.1. 팩토리 패턴 장점

상위 클래스와 하위 클래스가 분리되어 있어 **느슨한 결합**을 가지며, 상위 클래스에서는 **더 많은 유연성**을 갖게 된다. 객체 로직이 따로 떼어져 있기 때문에 코드를 리팩터링하더라도 한 곳만 고칠 수 있게 되니 **유지 보수성이 증가**된다.

### 2.2. 자바스크립트의 팩토리 패턴

자바스크립트에서 팩토리 패턴를 간단하게 `new Object()` 로 구현할 수 있다.

```javascript
const num = new Object(42);
const str = new Object("abc");
num.constructor.name; // Number
str.constructor.name; // String
```

숫자를 전달하거나 문자열을 전달함에 따라 다른 타입의 객체를 생성하는 것을 볼 수 있다. 즉, **전달받은 값에 따라 다른 객체를 생성하면서 인스턴스의 타입을 구한다.**

**[ 커피 팩토리를 기반으로 라떼 등을 생산하는 코드 ]**

```javascript
class Latte {
  constructor() {
    this.name = "latte";
  }
}
class Espresso {
  constructor() {
    this.name = "Espresso";
  }
}

class LatteFactory {
  static createCoffee() {
    return new Latte();
  }
}
class EspressoFactory {
  static createCoffee() {
    return new Espresso();
  }
}
const factoryList = { LatteFactory, EspressoFactory };

class CoffeeFactory {
  static createCoffee(type) {
    const factory = factoryList[type];
    return factory.createCoffee();
  }
}
const main = () => {
  // 라떼 커피를 주문한다.
  const coffee = CoffeeFactory.createCoffee("LatteFactory");
  // 커피 이름을 부른다.
  console.log(coffee.name); // latte
};
main();
```

`CoffeeFactory` 라는 상위 클래스가 중요한 뼈대를 결정하고 하위 클래스인 `LatteFactory` 가 구체적인 내용을 결정하고 있다. 이는 **의존성 주입**이라고 볼 수 있다.

`LatteFactory` 에서 생성한 인스턴스를 `CoffeeFactory` 에 주입하고 있기 때문이다.

또한 `createCoffee()` 정적 메서드를 정의하였는데, **정적 메서드**를 사용하면 클래스의 인스턴스 없이 호출이 가능하여 메모리를 절약할 수 있고 개별 인스턴스에 묶이지 않으며 클래스 내의 함수를 정의할 수 있는 장점이 있다.

<br>

## 3. 전략 패턴

> `전략 패턴 (Sttrategy Pattern)` 은 **정책 패턴(Policy Pattern)**이라고도 하며, 객체의 행위를 바꾸고 싶은 경우 직접 수정하지 않고 전략으로 부르는 '캡슐화한 알고리즘'을 컨텍스트 안에서 바꿔주면서 상호 교체가 가능하게 만드는 패턴이다.

### 3.1. passport의 전략 패턴

**passport**는 `Node.js` 에서 인증 모듈을 구현할 때 쓰는 미들웨어 라이브러리로, 여러 가지 **전략**을 기반으로 인증할 수 있게 한다.

```javascript
var passport = require("passport"),
  LocalStrategy = require("passport-local").Strategy;

passport.use(
  new LocalStrategy(function (username, password, done) {
    User.findOne({ username: username }, function (err, user) {
      if (err) {
        return done(err);
      }
      if (!user) {
        return done(null, false, { message: "Incorrect username." });
      }
      if (!user.validPassword(password)) {
        return done(null, false, { message: "Incorrect password." });
      }
      return done(null, user);
    });
  })
);
```

`passport.use(new LocalStrategy( ...` 처럼 `passport.use()` 라는 메서드에 **전략**을 매개변수로 넣어서 로직을 수행하는 것을 볼 수 있다.
<br>

## 4. 옵저버 패턴

> `옵저버 패턴 (Observer Pattern)` 은 주체가 어떤 객체의 상태 변화를 관찰하다가 상태 변화가 있을 때마다 메서드 등을 통해 옵저버 목록에 있는 옵저버들에게 변화를 알려주는 디자인 패턴이다.

여기서 말하는 `주체` 란 객체의 상태 변화를 보고 있는 관찰자를 뜻하고, `옵저버들` 이란 이 객체의 상태 변화에 따라 전달되는 메서드 등을 기반으로 **추가 변화 사항**이 생기는 객체들을 의미한다.

**옵저버 패턴 활용**

1. 트위터
2. 주로 이벤트 기반 시스템에 사용
3. MVC(Model-View-Controller) 패턴

### 4.1. 자바스크립트에서의 옵저버 패턴

자바스크립트에서의 옵저버 패턴은 프록시 객체를 통해 구현할 수 있다.

<span style="background-color: #f0baaa">&nbsp;**프록시 객체**&nbsp;</span> 는 어떠한 대상의 기본적인 동작(속성 접근, 할당, 순회, 열거, 함수 호출 등)의 작업을 가로챌 수 있는 객체를 뜻하며, **자바스크립트에서 프록시 객체는 두 개의 매개변수를 가진다.**

- `target` : 프록시할 대상
- `handler` : 프록시 객체의 target 동작을 가로채서 동작들이 정해져 있는 함수

**[ 프록시 객체 코드 ]**

```javascript
const handler = {
	get: function(target, name) {
    	return name === 'name' ? `${target.a} ${target.b}` : target[name]
    }
}
const p = new Proxy({a: 'KUNDOL', b: 'IS AUMUMU ZANGIN'}, handler)
console.log(p.name). // KUNDOL IS AUMUMU ZANGIN
```

`new Proxy` 로 선언한 객체와 a와 b라는 속성에 특정 문자열을 담아서 handler에 "**name이라는 속성에 접근할 때는 a와 b라는 것을 합쳐서 문자열을 만들어라.**"를 구현했다. p라는 변수에 name이라는 속성을 선언하지 않았는데도 p.name으로 name 속성에 접근하려고 할 때, 그 부분을 가로채 문자열을 만들어 반환하는 것을 볼 수 있다.

**[ 프록시 객체를 이용한 옵저버 패턴 코드 ]**

```javascript
function createReactiveObject(target, callback) {
  const proxy = new Proxy(target, {
    set(obj, prop, value) {
      if (value !== obj[prop]) {
        const prev = obj[prop];
        obj[prop] = value;
        callback(`${prop}가 [${prev}] >> [${value}] 로 변경되었습니다`);
      }
      return true;
    },
  });
  return proxy;
}
const a = {
  형규: "솔로",
};
const b = createReactiveObject(a, console.log);
b.형규 = "솔로";
b.형규 = "커플";
// 형규가 [솔로] >> [커플] 로 변경되었습니다
```

프록시 객체의 `get()` 함수는 속성과 함수에 대한 접근을 가로채며, `has()` 함수는 in 연산자의 사용을 가로챈다. `set()` 함수는 속성에 대한 접근을 가로챈다.

`set()` 함수를 통해 속성에 대한 접근을 **가로채**서 형규라는 속성이 솔로에서 커플로 되는 것을 확인할 수 있다.
