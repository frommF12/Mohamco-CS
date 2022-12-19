# Tech talk

### @Autowired를 사용하는 의존성 주입보다 생성자 주입을 권장하는 이유

> **의존성 주입**
>

Spring은 @Autowired 어노테이션을 이용한 다양한 **의존성 주입(DI; Dependency Injection)**방법을 제공한다.

의존성 주입은 필요한 객체를 직접 생성하는 것이 아닌 외부로부터 객체를 받아 사용하는 것이다.

이를 통해 객체 간의 결합도를 줄이고 코드의 재활용성을 높일 수 있다.

@Autowired는 Spring에게 의존성을 주입하는 지시자 역할로 쓰인다.

> **의존성 주입을 해야하는 이유**
>
- Test가 용이해진다.
- 코드의 재사용성을 높여준다.
- 객체 간의 의존성(종속성)을 줄이거나 없앨 수 있다.
- 객체 간의 결합도를 낮추면서 유연한 코드를 작성할 수 있다.

> 의존성 주입의 3가지 방법
>
1. 생성자 주입(Constructor Injection)
2. 필드 주입(Field Injection)
3. 수정자 주입(Setter Injection)

### 생성자 주입(Constructor Injection)

```java
@Component
public class Example {

		// final로 선언 가능
		private final HelloService helloService;

		// 단일 생성자인 경우 추가적인 어노테이션 생략 가능
		public Example(HelloService helloService) {
				this.helloService = helloService;
		}
}
```

클래스의 생성자가 하나이고, 그 생성자로 주입 받을 객체가 빈으로 등록되어 있다면 @Autowired를 생략할 수 있지만, 생성자가 2개 이상인 경우에는 생성자에 어노테이션을 붙여주어야 함

### 필드 주입(Field Injection)

```java
@Component
public class Example {
	
		@Autowired
		private HelloSerivce helloService;
}
```

필드에 @Autowired 어노테이션만 붙여주면 자동으로 의존성 주입이 된다.
사용법이 매우 간단하기에 가장 많이 접할 수 있는 방법이다.

단점

- 코드가 간결하지만, 외부에서 변경하기 힘들다.
- 프레임워크에 의존적이고 객체지향적으로 좋지 않다.

### 수정자 주입(Setter Injection)

```java
@Component
public class Example {
		
		private HelloService helloService;

		@Autowired
		public void setHelloService(HelloService helloService) {
				this.helloService = helloService;
		}
}
```

Setter 메소드에 @Autowired 어노테이션을 붙이는 방법이다. (꼭 Setter 메소드일 필요는 X)

단점

- 수정자 주입을 사용하면 setXXX 메소드를 public으로 열어두어야 하기 때문에 어디서든 변경이 가능하다.

### 그렇다면 왜 생성자 주입을 권장할까?

> **순환 참조를 방지할 수 있다.**
>

개발을 하다 보면 여러 컴포넌트 간에 의존성이 생긴다.

예를 들어, A가 B를 참조하고, 다시 B가 A를 참조하는 순환 참조되는 코드가 있다고 가정해보면

```java
@Service
public class AService {
		
		// 순환 참조
		@Autowired
		private BService bService;
	
		public void HelloA() {
				bService.HelloB();
		}
}
```

```java
@Service
public class BService {
		
		// 순환 참조
		private AService aService;

		public void HelloB() {
				aService.HelloA();
		}
}
```

필드 주입과 수정자 주입은 빈이 생성된 후에 참조를 하기 때문에 애플리케이션이 아무런 오류, 그리고 경고 없이 구동된다. 그리고 그것은 실제 코드가 호출될 때까지 문제를 알 수 없다는 것이다.

반면, 생성자를 통해 주입하고 실행하면 `BeanCurrentlyInCreatioinException`이 발생하게 되어 애플리케이션이 구동조차 되지 않게 된다.

그렇기 때문에 순환 참조는 생성자 주입에서만 문제가 된다.

그렇다면 순환 참조 오류를 피하기 위해 수정자 또는 필드 주입을 사용해야 할까? 그렇지 않다. 순환 참조가 있는 객체 설계는 잘못된 설계이기 때문에 **오히려 생성자 주입을 사용하여 순환 참조되는 설계를 사전에 막아야한다.**

> **불변성**
>

생성자로 의존성을 주입할 때 final로 선언할 수 있고, 이로인해 런타임에서 의존성을 주입받는 객체가 변할 일이 없어지게 된다.

하지만 수정자 주입이나 일반 메소드 주입을 이용하게 되면 풀필요하게 수정의 가능성을 열어두게 되고,
이는 OOP의 5가지 원칙중 OCP(Open-Closed Principal, 개방-폐쇄의 원칙)을 위반하게 된다.

그러므로 생성자 주입을 통해 변경의 가능성에 대해 배제하고 불변성을 보장하는 것이 좋다.

```java
@Service
public class AService {
		private final ARepository aRepository;

		public AService(ARepository aRepository) {
				this.aRepository = aRepository;
		}
}
```

> **테스트에 용이하다.**
>

생성자 주입을 사용하게 되면 테스트 코드를 조금 더 편리하게 작성할 수 있다.

DI의 핵심은 관리되는 클래스가 DI 컨테이너에 의존성이 없어야 한다는 것이다.

즉, 독립적으로 인스턴스화가 가능한 POJO(Plain Old Java Object)여야 한다는 것이다.

위와 같은 이유로 필드 주입이나 수정자 주입보다는 생성자 주입의 사용을 권장하는 바이다.

참고 사이트

---

[https://madplay.github.io/post/why-constructor-injection-is-better-than-field-injection](https://madplay.github.io/post/why-constructor-injection-is-better-than-field-injection)

[https://dev-coco.tistory.com/70](https://dev-coco.tistory.com/70)