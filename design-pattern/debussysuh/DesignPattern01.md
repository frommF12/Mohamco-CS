# 1. 디자인 패턴

## 디자인 패턴 (Design Pattern)

: 프로그램을 설계할 때 발생했던 문제점들을 객체 간의 상호 관계 등을 이용하여 해결할 수 있도록
하나의 **규약** 형태로 만들어 놓은 것

## 1. 싱글톤 패턴 (Singleton Pattern)

---

하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴으로, 보통 **데이터베이스 연결 모듈에 많이 사용**한다.

- 장점: 인스턴스 생성할 때 드는 비용이 줄어든다.
    - 하나의 인스턴스를 만들어 놓고 해당 인스턴스를 다른 모듈들이 공유하며 사용하기 때문이다.
- 단점: 의존성이 높아진다.

### 자바스크립트의 싱글톤 패턴

```jsx
const obj = {
	a: 27
}
const obj2 = {
	a: 27
}
console.log(obj === obj2)

//false
```

- 리터럴 {} 또는 new Object로 객체 생성하게 되면, 다른 어떤 객체와도 같지 않기 때문에 자체만으로도 싱글톤 패턴 구현 가능
- obj와 obj2는 다른 인스턴스를 가진다.

```jsx
class Singleton {
	constructor() {
		if (!Singleton.instance) {
			Singleton.instance = this
		}
		return Singleton.instance
	}
	getInstance() {
		return this.instance
	}
}
const a = new Singleton()
const b = new Singleton()
console.log(a==b)

//true
```

- Singleton.instance 라는 하나의 인스턴스를 가지는 Singleton 클래스 구현
- 이를 통해 a와 b는 하나의 인스턴스를 가진다.

### 데이터베이스 연결 모듈

```jsx
const URL = "mongodb://localhost:27017/kundolapp"
const createConnection = (url) => ({ url: url })
class DB {
  constructor(url) {
    if (!DB.instance) {
      DB.instance = createConnection(url)
    }
    return DB.instance
  }
  connect() {
    return this.instance
  }
}
const a = new DB(URL)
const b = new DB(URL)
console.log(a === b)

// true
```

- DB.instance 라는 하나의 인스턴스를 기반으로 a, b 생성
- 데이터베이스 연결에 관한 인스턴스 생성 비용을 아낄 수 있음

### 싱글톤 패턴의 단점

> 주로 **TDD (Test Driven Development)** 시 걸림돌이 된다.
> 
- TDD를 할 때 **단위 테스트**를 주로 하기 때문이다.
    - 단위 테스트 : 테스트가 **독립적**이어야 하고 **어떤 순서로든 실행**할 수 있어야 한다.
- 각 테스트마다 ‘독립적인’ 인스턴스를 만들기 어렵다.
    - 싱글톤 패턴은 미리 생성된 하나의 인스턴스를 기반으로 구현하는 패턴이기 때문이다.
- 사용하기 쉽고 실용적이나 모듈 간의 **결합**을 **강하게** 만들 수 있다.

### 의존성 주입 (DI, Dependency Injection)

→ 싱글톤 패턴의 단점인 모듈 간의 **강한 결합**을 **느슨하게** 만들어 해결

의존성은 ‘종속성’이라고도 하며, A가 B에 의존성이 있다는 것은 B의 변경 사항에 대해 A 또한 변해야 된다는 것을 의미한다.

의존성 주입 방식

- 메인 모듈이 아닌 의존성 주입자가 하위 모듈에 간접적으로 의존성을 주입하는 방식
    - 메인 모듈이 하위 모듈에 대한 의존성이 떨어지게 된다.
    - == ‘디커플링이 된다’

장점

1. 모듈 쉽게 교체 가능한 구조가 되어, 쉬운 테스팅과 수월한 마이그레이션
2. 애플리케이션 의존성 방향 일관됨, 쉽게 추론 가능, 모듈 간의 관계들이 더 명확해짐
    - 구현할 때 추상화 레이어를 넣고 이를 기반으로 구현체를 넣어 주기 때문이다.

단점 : 모듈이 더욱더 분리됨 → 클래스 수 증가 → 복잡성 증가 → 약간의 런타임 페널티가 생기기도 함

의존성 주입 원칙

- 상위 모듈은 하위 모듈에서 어떠한 것도 가져오지 않아야 한다.
- 둘 다 추상화에 의존해야 하며, 이때 추상화는 세부 사항에 의존하지 말아야 한다.

## 2. 팩토리 패턴 (Factory Pattern)

---

1. 객체를 사용하는 코드에서 객체 생성 부분을 떼어내 추상화한 패턴
2. 상속 관계에 있는 두 클래스에서 상위 클래스가 중요한 뼈대를 결정하고, 하위 클래스에서 객체 생성에 관한 구체적인 내용을 결정하는 패턴

특징

- 느슨한 결합을 가짐
    - 상위 클래스와 하위 클래스가 분리되기 때문이다.
- 더 많은 유연성을 가짐
    - 상위 클래스에서는 인스턴스 생성 방식에 대해 알 필요가 없기 때문이다.
- 유지 보수성 증가
    - 객체 생성 로직이 분리되어 있으므로 코드를 리팩터링하더라도 한 곳만 고칠 수 있게 된다.
- 예시
    
    구체적인 내용(라떼 레시피, 아메리카노 레시피, 우유 레시피)
    
    구체적인 내용이 담긴 하위 클래스가 컨베이스 벨트를 통해 전달됨 → 상위 클래스(바리스타 공장)에서 레시피를 토대로 물질(라떼, 우유 등) 생산
    

### 자바스크립트의 팩토리 패턴

```jsx
const num = new Object(42)
const str = new Object("abc");
num.constructor.name; // Number
str.constructor.name; // String
```

- 전달받은 값에 따라 **다른 타입의 객체 타입 생성**하며 **인스턴스 타입 등을 결정**

```jsx
// 커피 팩토리를 기반으로 라떼 등을 생성하는 코드
class Latte {
  constructor() {
    this.name = "latte"
  }
}
class Espresso {
  constructor() {
    this.name = "Espresso"
  }
}
class LatteFactory {
  static createCoffee() {
    return new Latte()
  }
}
class EspressoFactory {
  static createCoffee() {
    return new Espresso()
  }
}
const factoryList = { LatteFactory, EspressoFactory }
class CoffeeFactory {
  static createCoffee(type) {
    const factory = factoryList[type]
    return factory.createCoffee()
  }
}
const main = () => {
  // 라떼 커피를 주문한다.
  const coffee = CoffeeFactory.createCoffee("LatteFactory")
  // 커피 이름을 부른다.
  console.log(coffee.name); // latte
}
main()
```

의존성 주입

- 상위 클래스 `CoffeeFactory`  - 중요한 뼈대 결정
- 하위 클래스 `LatteFactory`  - 구체적인 내용 결정
    - `LatteFactory` 에서 생성한 인스턴스를 `CoffeeFactory` 에 주입하고 있기 때문이다.

- `createCoffee()` 정적 메서드 정의
    - **정적 메서드 사용 시 장점**
    1. 클래스의 인스턴스 없이 호출이 가능하여 메모리를 절약 가능
    2. 개별 인스턴스에 묶이지 않으며 클래스 내의 함수 정의 가능

## 3. 전략 패턴 (Strategy Pattern)

---

: 객체 행위를 바꾸고 싶은 경우 ‘직접’ 수정하지 않고 전략이라고 부르는 ‘캡슐화한 알고리즘’을 컨텍스트 안에서 바꿔주면서 상호 교체가 가능하게 만드는 패턴

정책 패턴 (Policy Pattern)이라고도 한다.

- 컨텍스트
상황, 맥락, 문맥을 의미
개발자가 어떠한 작업을 완료하는 데 필요한 모든 관련 정보를 말한다.

### passport의 전략 패턴

: Node.js 인증 모듈 구현 미들웨어 라이브러리로, LocalStrategy 전략과 OAuth 전략 등을 지원하고 여러 가지 ‘전략’을 기반으로 인증할 수 있게 한다.

- LocalStrategy 전략
    - 서비스 내의 회원가입된 ID, PW 기반 인증
- OAuth 전략
    - 페이스북, 네이버 등 다른 서비스 기반 인증

```jsx
// ‘전략’만 바꿔서 인증하는 코드
var passport = require('passport'),
  LocalStrategy = require('passport-local').Strategy;

passport.use(new LocalStrategy(
	function (username, password, done) {
    User.findOne({ username: username }, function (err, user) {
      if (err) {
        return done(err);
      }
      if (!user) {
        return done(null, false, { message: 'Incorrect username.' });
      }
      if (!user.validPassword(password)) {
        return done(null, false, { message: 'Incorrect password.' });
      }
      return done(null, user);
    });
  })
);
```

- passport.use(new LocalStrategy(..
    - passport.use() 메서드에 ‘전략’을 매개변수로 넣어 로직 수행

## 4. 옵저버 패턴 (Obsever Pattern)

---

: **주체**가 어떤 객체(subject)의 **상태 변화**를 **관찰하다가** 상태 변화가 있을 때마다 메서드 등을 통해 옵저버 목록에 있는 **옵저버**들에게 **변화를 알려주는** 디자인 패턴

- 주체: 객체의 상태 변화를 보고 있는 관찰자
- 옵저버들: ‘추가 변화 사항’이 생기는 객체들 (객체의 상태 변화에 따라 전달되는 메서드 등을 기반
- 주체와 객체를 따로 두지 않고 상태가 변경되는 객체를 기반으로 구축하기도 함 (ex. 트위터)

### 옵저버 패턴 구조

주로 이벤트 기반 시스템과 MVC 패턴에 사용된다.

Model(주체)에 변경 사항  발생 → update() 메서드로 View(옵저버)에 알림 → 이를 기반으로 Controller 등이 작동

### 프록시(proxy) 객체

: 어떠한 대상의 기본적인 동작(속성 접근, 할당, 순회, 열거, 함수 호출 등)의 **작업을 가로챌 수 있는** 객체

- 자바스크립트에서 프록시 객체는 두 개의 매개변수를 가짐
    - **target** : 프록시할 대상
    - **handler** : 프록시 객체의 target 동작을 가로채서 정의할 동작들이 정해져 있는 함수
- 디자인 패턴 중 하나인 프록시 패턴이 녹아들어 있는 객체

### 자바스크립트에서의 옵저버 패턴

프록시 객체를 통해 구현 가능

```jsx
// 프록시 객체 구현 코드
const handler = {
	get: function(target, name) {
    	return name === 'name' ? `${target.a} ${target.b}` : target[name]
    }
}
const p = new Proxy({a: 'KUNDOL', b: 'IS AUMUMU ZANGIN'}, handler)
console.log(p.name). // KUNDOL IS AUMUMU ZANGIN
```

- `new Proxy` 로 선언한 객체와 a와 b라는 속성에 특정 문자열을 담아 -> handler에 "**name이라는 속성에 접근할 때는 a와 b라는 것을 합쳐서 문자열을 만들어라.**" 구현
- p라는 변수에 name이라는 속성을 선언하지 않았는데도 p.name으로 name 속성에 접근하려고 할 때, 그 부분을 가로채 문자열을 만들어 반환하는 것을 볼 수 있다.

### → 프록시 객체를 이용한 옵저버 패턴

```jsx
function createReactiveObject(target, callback) {
  const proxy = new Proxy(target, {
    set(obj, prop, value) {
      if (value !== obj[prop]) {
        const prev = obj[prop]
        obj[prop] = value
        callback(`${prop}가 [${prev}] >> [${value}] 로 변경되었습니다`)
      }
      return true
    },
  })
  return proxy
}
const a = {
  형규: "솔로",
}
const b = createReactiveObject(a, console.log)
b.형규 = "솔로"
b.형규 = "커플"
// 형규가 [솔로] >> [커플] 로 변경되었습니다
```

- 프록시 객체의 `get()` 함수는 속성과 함수에 대한 접근을 가로챈다.
- `has()` 함수는 in 연산자의 사용을 가로챈다.
- `set()` 함수는 속성에 대한 접근을 가로챈다.

→ `set()` 함수가 형규의 속성을 가로채서 솔로에서 커플로 되는 것을 확인

## 5. 프록시 패턴 (Proxy Pattern)

---

: 대상 객체(subject)에 접근하기 전 그 접근에 대한 흐름을 가로채 대상 객체 앞단의 인터페이스 역할을 하는 디자인 패턴

- 객체의 속성, 변환 등을 보완
- 보안, 데이터 검증, 캐싱, 로깅에 사용
- 프록시 객체, 프록시 서버로도 활용

프록시 서버에서의 캐싱

- 캐시 안에 정보를 담아두고, 캐시 안에 있는 정보를 요구하는 요청에 대해 다시 저 멀리 있는 원격 서버에 요청하지 않고 캐시 안에 있는 데이터를 활용하는 것을 말한다.
- 이를 통해 불필요하게 외부와 연결하지 않기 때문에 트래픽을 줄일 수 있다는 장점이 있다.

## 5.1. 프록시 서버 (Proxy Server)

: 서버와 클라이언트 사이에서 클라이언트가 자신을 통해 다시 네트워크 서비스에 간접적으로 접속할 수 있게 해주는 컴퓨터 시스템이나 응용 프로그램

### A. nginx

---

비동기 이벤트 기반의 구조와 다수의 연결을 효과적으로 처리 가능한 웹 서버

- 주로 Node.js 서버 앞단의 프록시 서버로 활용
- Node.js 서버 구축 시, 앞단에 nginx를 둔다.
    - Node.js의 버퍼 오버 플로우 취약점 예방
    - 익명 사용자의 직접적인 서버로의 접근 차단 → 보안성 강화
        - 버퍼 오버플로우 : 버퍼는 데이터가 저장되는 메모리 공간으로, 메모리 공간을 벗어나는 경우를 말한다. 이때 사용되지 않아야 할 영역에 데이터가 덮어씌워져 주소, 값을 바꾸는 공격이 발생하기도 한다.
- 실제 포트 숨기기 가능
- 정적 자원을 gzip 압축 가능
    - gzip 압축 : DEFLATE 알고리즘(LZ77 + Huffman 코딩의 조합)을 기반으로 한 압축 기술.
    - 데이터 전송량을 줄일 수 있으나, 압축 해제 시 서버에서의 CPU 오버헤드도 생각해서 gzip 압축 사용 유무 결정해야 한다.
- 메인 서버 앞단에서의 로깅 가능

### B. CloudFlare

---

전 세계적으로 분산된 서버를 통해 어떤 시스템의 콘텐츠 전달을 빠르게 하는 CDN 서비스

- 사용자, 크롤러, 공격자가 자신의 웹 사이트에 접속 시 CloudFlare를 통해 공격자로부터 보호 가능
- 사용 시 이점
    - DDOS 공격 방어
    - HTTPS 구축
    
    → 웹 서버 앞단에 두어 ‘프록시 서버’로 쓰기 때문에 이 모든 것이 가능한 것이다.
    

- CDN (Content Delivery Network)
    - 각 사용자가 인터넷에 접속하는 곳과 가까운 곳에서 콘텐츠를 캐싱 또는 배포하는 서버 네트워크
    - 이를 통해 사용자가 웹 서버로부터 콘텐츠를 다운로드하는 시간 감소

### DDOS 공격 방어

DDOS : 짧은 기간 동안 네트워크에 많은 요청을 보내 네트워크를 마비시켜 웹 사이트의 가용성을 방해하는 사이버 공격 유형

CloudFlare는 의심스러운 트래픽, 특히 사용자가 접속하는 것이 아닌 **시스템을 통해 오는 트래픽을 자동으로 차단** → DDOS 공격으로부터 보호

CloudFlare의 거대한 용량과 캐싱 전략으로 소규모 DDOS 공격은 쉽게 막아낼 수 있으며 이러한 공격에 대한 **방화벽 대시보드 제공**

### HTTPS 구축

서버에서 HTTPS 구축 시 CloudFlare를 사용으로 **별도의 인증서 설치 없이** 좀 더 손쉽게 HTTPS를 구축 가능

### C. CORS와 프론트엔드의 프록시 서버

---

CORS (Cross-Origin Resource Sharing): 서버가 웹 브라우저에서 리소스를 로드할 때, 다른 오리진을 통해 로드하지 못하게 하는 HTTP 헤더 기반 메커니즘

- 오리진 : 프로토콜과 호스트 이름, 포트의 조합
- FE 서버 앞단에 프록시 서버를 놓아서 만든다.
    - FE 서버를 만들어서 BE 서버와 통신 시에 마주치는 CORS 에러 해결 가능
    - FE 서버와 BE 서버 간의 포트 번호 불일치 오류 해결
    - 다양한 API 서버와의 통신 가능