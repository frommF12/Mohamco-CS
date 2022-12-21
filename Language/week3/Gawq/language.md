# 추상 클래스와 인터페이스의 차이

> 추상 클래스(abstract class)
>

```java
public abstract class 클래스 이름 {
  ...
	public abstract void 메소드 이름();
}
```

추상 클래스는 클래스 구현부 내부에 **추상 메소드가 하나 이상 포함되거나 abstract로 정의**된 경우를 말한다.
abstract class를 **상속받은 클래스(하위 클래스)는 자기의 성질에 맞게 Overriding하여 사용**한다.

- 상속을 위한 클래스이기 때문에 new 연산자를 사용하여 객체를 생성할 수 없다.
- java에서는 다중 상속을 지원하지 않기 때문에 여러 개의 추상 클래스를 상속할 수 없다.
- 일반 메소드나 변수의 사용이 가능하다.

**추상 메소드란,**

선언부만 작성하고 구현부는 작성하지 않은 채로 남겨두는 것이 추상 메소드이며, 상속받는 클래스에 따라서 구현되는 내용이 달라질 수 도 있다.

***

인터페이스와는 다르게 static이나 final이 아닌 필드를 가질 수 있고, public, protected, private 접근 제어자를 모두 사용 가능하다.

```java
public abstract class AbstractClass {
	// Field
	private String name;

	// Constructor
	public AbstractClass(String name) {
		this.name = name;
	}

	// Method
	public void method() {};
}

public class RealClass extends AbstractClass {
	private String address;

	public RealClass(String name, String address) {
		super(name);
		this.address = address;
	}
}
```

***

추상 클래스는 상속을 통해서만 사용 가능하며, 하위 클래스의 생성자에서 `super()`를 사용해서 추상 클래스의 생성자를 부르고 초기화시킨다.

> 인터페이스(interface)
>

```java
interface 인터페이스 이름 {
		...
	public abstract void 메소드 이름();
	public default void 메소드 이름() {};
}
```

인터페이스는 모든 메소드가 추상 메소드인 경우를 말한다.

- 인터페이스는 추상 클래스보다 추상화 정도가 높으며 추상 클래스와는 다르게 구현부가 있는 일반 메서드, 일반 멤버 변수를 가질 수 없다.
- 인터페이스의 모든 메소드는 `‘public abstract’`로 선언해야 하며, 이를 생략할 수 있다.
- 모든 멤버 변수는 `‘public static final’`으로 선언해야 하며, 마찬가지로 생략 가능하다.
  (생략할 수 있는 이유는 컴파일 시 자동으로 생성해주기 때문)
    - public static final을 사용하는 이유
        - 인터페이스는 인스턴스가 존재할 수 없기 때문에 상속받은 구현 객체에서 같은 상태를 보장하기 위해서 public static final을 사용한다.
- 추상 클래스와 마찬가지로 new 연산자를 사용해 객체 생성 불가능
- 추상 클래스와 반대로 다중 상속 가능

> 클래스와 인터페이스 사이 관계 이해하기
>

아래 사진에서 보이는 것처럼, **클래스끼리, 인터페이스끼리** **상속**할 때는 `extends`,
**클래스가 인터페이스를 상속**받을 때는 `implements` 키워드를 사용한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f076b2a2-cf05-453b-b21f-2b1c715f7098/Untitled.png)

> 추상 클래스와 인터페이스의 공통점
>
- 추상 클래스와 인터페이스는 선언부만 있고 구현은 없는 클래스이다.

  (자바 8부터 인터페이스에서 default method로 구현이 가능하지만 일반적으로 구현은 없다.)

- 인스턴스화(객체 생성 X)를 할 수 없다.
- 상속받은 클래스가 반드시 선언된 추상 메소드를 구현하도록 한다.

> 추상 클래스와 인터페이스의 차이점
>
- 추상 클래스는 extends 키워드를 사용하여 상속하며 다중 상속은 불가능 하다. (단일 상속)
  반면 인터페이스는 implements 키워드를 사용하여 상속하며 다중 상속이 가능하다.
- 추상 클래스는 일반 변수, 생성자, 일반 메소드, 추상 메소드를 모두 가질 수 있는 반면 인터페이스는 상수와 추상 메소드만 가질 수 있고, 생성자와 일반 변수는 가질 수 없다.
- 추상 클래스의 목적은 상속을 받아서 기능을 확장시키는 것
- 인터페이스의 목적은 구현하는 모든 클래스에서 특정한 메소드가 반드시 존재하도록 강제하는 역할
  즉, 구현 객체가 같은 동착을 한다는 것을 보장하기 위함

***

정리하자면 자바의 특성상 하나의 클래스만 상속이 가능하기 때문에 해당 클래스의 구분을 추상 클래스 상속을 통해 해결하고, 할 수 있는 공통된 기능들을 인터페이스의 다중 상속을 통해 구현한다.

***

추상 클래스는 다중 상속이 불가능하기 때문에 하나의 클래스에서 하위 클래스에 물려줄 특성이 풍부할수록 좋고, 인터페이스는 다중 상속이 가능하기 때문에 각각의 인터페이스는 목적에 맞는 최소한의 메소드(구현을 강제할)를 선언하는 것이 좋다.

참고사이트

---

[https://wildeveloperetrain.tistory.com/112](https://wildeveloperetrain.tistory.com/112)

[https://dev-coco.tistory.com/12](https://dev-coco.tistory.com/12)

[https://brunch.co.kr/@kd4/6](https://brunch.co.kr/@kd4/6)