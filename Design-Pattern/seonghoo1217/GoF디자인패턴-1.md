# 디자인 패턴

디자인 패턴은 프로그램을 설계할 때 발생했던 문제점들을 객체 간의 상호 관계등을 이용하여 해결할 수 있도록 하나의 규약 형태로 만들어 놓은것

표준은 아니지만 표준적인 해법과 작명법을 제시한 도서가 GoF의 <디자인 패턴>이라는 책이며 현재 수 많은 디자인 패턴의 학습의 기준이 되는 책이다.

GoF에서는 크게 패턴을 **생성, 구조, 동작** 패턴으로 정의하며 이외에도 **동시성, 아키텍처,기타 패턴**등으로 분리되며 동시성은 크게 **동기화**에 대해 다루며 아키텍처의 경우 대표적으로 **MVC 모델 패턴**이 존재한다.

## 싱글톤 패턴

![1](https://user-images.githubusercontent.com/39437170/208501220-da24513b-87de-45d7-bd3a-50f66168e496.png)

### 싱글톤 패턴의 정의

싱글톤 패턴은 `생성패턴`에 속하며 하나의 클래스에 하나의 인스턴스만을 가지는 패턴이다.

하나의 클래스를 기반으로 여러개의 개별적인 인스턴스 생성이 가능하지만 하나만의 인스터를 만들어 이를 기반으로 로직을 만드는데 사용된다. 대부분 DB연결에서 사용된다.

보통 객체를 새로 생성하기 위해선 자바의 `new` 예약어를 통해 인스턴스를 생성하여 사용한다.

예를 들어 new를 통해 A라는 클래스를 10번호출 할경우 A라는 인스턴스는 총 10개가 생성되는 것이다.

하지만 하나의 인스턴스만을 보장받아 사용해야하는 경우가 있다. 예를 들어 공동이 사용하는 자원에 관하여 필요한 시점마다 계속 new를 통해 인스턴스를 생성하게 된다면 해당 자원의 관리는 어려워질뿐아니라 의미 자체가 사라지게 된다.

이런 경우에 우리는 싱글톤 패턴을 사용하여 1개의 인스턴스만을 사용한다.

**혹시 `new`를 한번만 호출하여 객체를 1개만 생성한다면?**

이라는 발상을 다들 한번은 해보았을 수 있지만 이것이 해당 인스턴스가 1개만 존재한다는 것을 `**보증`** 해줄수는 없다.

### 하나의 인스턴스만을 보장해야하는 이유

**한개의 인스턴스만을 보장받아 얻는 이점은 과연 무엇인가?**

우선적으로 메모리 측면에서 이점을 볼 수 있다. 이는 당연한 결과로 최초 한번의 `new` 를 통해 생성된 객체를 고정된 메모리 영역에서 사용하고 추후 해당객체에 대한 작업을 통할때 이미 생성한 객체에 접근하기에 메모리의 손실을 최소화할 수 있다.

부가적으로 이미 생성한 객체에 접근하기에 속도적인 측면에서도 이점을 얻을 수 있다.

또다른 이점으로는 다른 객체간에 데이터 공유가 쉽다는 것이다. 싱글톤 인스턴스가 전역으로 사용되는 인스턴스이기 때문에 다른 클래스의 인스턴스가 접근하여 사용 할 수 있다.

하지만 이의 문제는 동시성문제로 이어지게 되는데 이점을 유의하여 사용하여야한다.

### 싱글톤 패턴 예제 코드

```java
package SingleTon_Pattern;

public class Singleton {
	private static Singleton singleton=new Singleton();

	private Singleton(){
		System.out.println("created Singleton");
	}

	public static Singleton getInstance(){
		return singleton;
	}
}
```

```java
package SingleTon_Pattern;

public class Main {
	public static void main(String[] args) {
		System.out.println("Start");
		Singleton instance1 = Singleton.getInstance();
		Singleton instance2 = Singleton.getInstance();

		System.out.println("instance1과 instance2 동일성 비교::"+(instance1==instance2));
		System.out.println("end");
	}
}
```

### 싱글톤 패턴의 문제점

싱글톤 패턴은 위에서 말한 이점들을 가질 수 있지만 많은 문제점 또한 수반하기에 `trade-off` 를 잘고려하여야한다.

**먼저 싱글톤 패턴을 구현하는 코드 자체가 많이 필요하다**. 객체의 생성을 확인하고 생성자를 호출하는 경우에 **멀티스레딩 환경**에서 발생할 수 있는 동시성 문제를 해결하기 위해 **syncronized** 키워드를 사용해야한다.

**두번째로는** 테스트가 어렵다. TDD를 할 때 걸림돌이된다. 싱글톤 인스턴스는 자원을 공유하기 때문에 테스트가 결정적으로 격리된 환경에서 수행되기 위해선 인스턴스는 매번 초기화되어 새상태를 보장받아야한다. 단위테스트는 서로 독립적이어야한다. 하지만 싱글톤 패턴은 이미 생성된 인스턴스를 기반으로 하기에 독립적이지 못한다.

그렇지않다면 어플리케이션 전역에서 상태를 공유하기 때문에 테스트가 온전하게 수행되지 못한다.

**세번째로는** 의존 관계상 클라이언트가 구현체 클래스에 의존적이게 된다. new 키워드를 직접 사용하여 클래스 안에서 직접 객체를 생성하고 있으므로, 이는 SOLID 원칙 중 DIP를 위배하게되고 OCP 원칙 또한 위반할 가능성이 높다.

### 의존성 주입

싱글톤 패턴은 결합도가 상당히 높은 패턴으로 의존성 주입을 활용시에 이를 대처할 수 있음

의존성 주입자가 직접 하위 모듈에게 줌으로서 메인모듈이 간접적으로 의존성을 주입함. 이를 **`디커플링`**이라고 함

### 의존성 주입의 장점

- 모듈의 교체가 간단해져 테스팅이 쉬어짐
- 마이그레이션 또한 쉬워짐
- 추상화를 기반으로 구현체를 넣어주기에 의존성 방향이 일관됨
- 모듈의 관계 명확

### 의존성 주입의 단점

- 모듈들이 분리되므로 클래스 수가 늘어나 복잡성이 증가
- 때문에 약간의 런타임 패널티

### 의존성 주입 원칙

- 상위 모듈은 하위 모듈에서 어떤 것도 가져오지 않는다
- 팩토리 패턴과 유사

### 결론

오직 한 개의 인스턴스 생성을 보증하여 효율을 찾을 수 있지만 그에 못지많게 수반되는 문제점도 많다. 싱글톤 패턴은 안티패턴으로 불릴 만큼 단독으로 사용한다면 객체 지향에 위반되는 사례가 많다. 스프링 컨테이너 같은 프레임워크의 도움을 받으면 싱글톤 패턴의 문제점들을 보완하면서 장점의 혜택을 누릴 수 있다. 실제로 스프링 빈은 컨테이너의 도움을 받아 싱글톤 스콥으로 관리되고 있다.

프레임워크 도움없이 싱글톤 패턴을 적용하고 싶다면, 위에서 살펴본 장단점의 trade-off를 잘 고려하여 사용하는 것이 좋을 것이다.

## 팩토리 패턴

### 팩토리 패턴의 정의

객체의 생성을 추상화시켜 상위 클래스에서는 뼈대만 결정하고 하위 클래스에서 상세 내용을 결정한다.

상위 클래스와 하위 클래스가 분리되어 있고 분리로 인한 리팩터링에 대한 부담이 적고 유연성이 높다

### 팩토리 패턴의 종류

- 팩토리 메소드 패턴
- 추상 팩토리 패턴

### 팩토리 메소드 패턴의 정의

![2](https://user-images.githubusercontent.com/39437170/208501328-74d2a271-2e86-4233-bf5b-d182ccc3d7a3.png)

객체를 만들어내는 부분을 `서브 클래스(Sub-Class)` 에 위임하는 패턴

즉 `new` 키워드 호출로 객체를 생성하는 역할을 서브 클래스(하위 클래스) 에 위임하는 것이다. 결국 팩토리 메소드 패턴은 객체를 만들어내는 공장을 만드는 패턴이다.

팩토리 메소드 패턴에서 인스턴스를 만드는 방법을 상위 클래스 측에서 결정하지만 구체적인 클래스명까지 지정하진 않습니다.

구체적인 내용은 모두 하위 클래스 측에서 수행한다.

따라서 인스턴스 생성을 위한 `골격(framework)`과 실제의 인스턴스 생성의 클래스를 분리해서 생각할 수 있다.

### 추상 팩토리 패턴의 정의

![3](https://user-images.githubusercontent.com/39437170/208501393-f245e402-03c5-4bd6-9ddd-3eea08f6cd17.png)

- 인터페이스를 사용하여 구현 클래스없이 관련 객체들의 팩토리를 생성할 수 있음
- '**구현부에 신경쓰지 않고 인터페이스(API)만 생각**'하는 상태

**AbstractProduct**

⇒ AbstractFactory 역할에 의해 만들어지는 추상적인 부품이나 제품의 인터페이스(API)입니다.

**AbstractFactory**

⇒ AbstractProduct 역할의 인스턴스를 만들어 내기 위한 인터페이스(API)를 결정합니다.

**ConcreteProduct**

⇒ AbstractProduct 역할에서 명세되어있는 인터페이스(API)를 구현합니다.

**ConcreteFactory**

⇒ AbstractFactory 역할에서 명세된 인터페이스(API)를 구현합니다.

### 팩토리 메소드 패턴 vs 메소드 추상 패턴

팩토리 메소드 패턴은 한 종류의 객체를 생성하기 위해 사용되지만, 추상 팩토리 패턴은 연관되거나 의존적인 객체로 이루어진 여러 종류의 객체를 생성하기 위해 사용된다.

또한 팩토리 메소드 패턴은 팩토리 인터페이스를 구현하여 그 자체가 하나의 객체를 생성하는데 사용되지만, 추상 팩토리 패턴은 팩토리 객체가 아닌 다른 객체 내부에 구현되어 해당 객체에서 여러 타입을 생성하기 위함

### 팩토리 메소드 패턴 예시

```java
// 상위 팩토리
package Factory_Method_Pattern.test;

public interface Factory {
	void createItem();
}
```

```java
package Factory_Method_Pattern.test;

public class NoteBookFactory implements Factory{

	@Override
	public void createItem() {
		System.out.println("노트북 만듬");
	}
}
```

```java
package Factory_Method_Pattern.test;

public class PhoneFactory implements Factory {

	@Override
	public void createItem() {
		System.out.println("핸드폰 생성");
	}
}
```

### 추상 팩토리 패턴 예시

```java
package Abstract_Factory_Pattern.test;

public interface FoodAbstractFactory {
	void createSauce();
	void createSalad();
	void createStake();
}
```

```java
package Abstract_Factory_Pattern.test;

public class ChefFoodFactory implements FoodAbstractFactory {
	@Override
	public void createSauce() {
		System.out.println("소스 만들기");
	}

	@Override
	public void createSalad() {
		System.out.println("샐러드 만들기");
	}

	@Override
	public void createStake() {
		System.out.println("스테이크 만들기");
	}
}
```

```java
package Abstract_Factory_Pattern.test;

public class Restaurant {
	private String food;
	public void getOrder(){
		FoodAbstractFactory chefA = new ChefFoodFactory();
		switch (food){
			case "salad":
				chefA.createSalad();
				break;
			case "sauce":
				chefA.createSauce();
				break;
			case "stake":
				chefA.createStake();
				break;
		}
	}

	public Restaurant(String food) {
		this.food=food;
	}
}
```

이때 chef가 바뀔경우 FooadAbstractFactory의 구현체만 바꾸면되므로 유지보수성이 높다.

## 전략패턴

![4](https://user-images.githubusercontent.com/39437170/208501456-74d536e7-7264-4136-9095-7513f012f908.png)

### 전략패턴 정의

사용자(Client)가 자신에게 맞는 전략(Strategy)을 선택하여 로직을 선택할 수 있다.

전략패턴이란 동작패턴에 속하며 쉽게 말해 `전략`을 쉽게 변경가능하게 설계하는 패턴이다.

여기서 말하는 `전략`의 정의는 객체의 행위에 대하여 그 행위가 목적을 달성하기 위해 일을 수행하는 방식, 비즈니스 규칙, 문제를 해결하는 알고리즘을 의미한다.

여기서 행위는 메소드이고 행위가 목적을 달성한다는 것은 메소드가 에러를 만나지않고 무사히 실행 후 안전히 종료되는것을 뜻한다.

좀더 사전적인 정의로는 다음과 같이 정의할 수 있다.

> 객체들이 할 수 있는 행위 각각에 대한 전략 클래스를 구현한 후 유사 행위들을 캡슐화한 인터페이스를 정의한다.
그 후 객체의 행위를 동적으로 변경하고 싶은 경우 직접 행위를 수정하지 않고 전략을 바꿔주기만 함으로서 행위를 유연하게 확장하는 방법
>

1. 전략 패턴 사용의 이유
- Solid의 원칙 중 하나인 OCP를 위배하지 않을 수 있다.
- 확장성있는 설계를 위해서

1. 전략 패턴의 구현 방법 및 순서
- 전략을 우선적으로 생성한다. 이때  인터페이스를 생성하여 상속받는 구조로 진행할 경우 캡슐화에 위배되지 않는다는 점과 확장성을 고려한 설계가 가능해진다는 이점이 존재한다.
- 다음으로 전략을 설정할 수 있는 메소드를 초기화하여 클래스 내부에 구현한다.
  해당 방법의 경우 기존 메소드를 수정하지 않고 (OCP를 위배하지않고) 값의 유연한 변경이 가능해진다.
- 이제 전략 클래스를 사용하는 Client 객체를 만들어 이용해본다.

### 전략패턴 예제

```java
package Strategy_Pattern.apply;

public interface WeaponStrategy {
	void attack();
}
```

```java
package Strategy_Pattern.apply;

public class SwordStrategy implements WeaponStrategy {
	@Override
	public void attack() {
		System.out.println("검으로 공격");
	}
}
```

```java
package Strategy_Pattern.apply;

public class KnifeStrategy implements WeaponStrategy {
	@Override
	public void attack() {
		System.out.println("칼로 공격");
	}
}
```

```java
package Strategy_Pattern.apply;

public class CharacterStrategy {
	private WeaponStrategy weaponStrategy;

	//교환성 보장
	public void setWeaponStrategy(WeaponStrategy weaponStrategy){
		this.weaponStrategy=weaponStrategy;
	}

	public void characterAct() {
		weapon_type(weaponStrategy);
	}

	private void weapon_type(WeaponStrategy weaponStrategy){
		if (weaponStrategy==null){
			System.out.println("맨손 공격");
			return;
		}
		weaponStrategy.attack();
	}
}
```

## 옵저버 패턴

![5](https://user-images.githubusercontent.com/39437170/208501483-ca618a5d-7be4-4503-8ce0-c36a2fdb0ad7.png)

### 옵저버 패턴의 정의

옵저버 패턴은 동작패턴에 속하며 주체가 대상 객체의 상태 변화를 관찰하다 상태 변화가 생길 경우 메서드 등을 통해 객체가 직접 목록의 각 옵저버에게 통지하도록 하는 디자인 패턴이다.

발행/구독 모델로 알려져 있기도 하다.

비슷한 예시로 웹 소켓의 STOMP구조 또한 해당 패턴에 속한다.

### 옵저버 패턴 예시

![6](https://user-images.githubusercontent.com/39437170/208501557-fb21418e-e711-4e4c-9215-35470ce6db44.png)

> 옵저버 패턴(Observer Pattern)에서는 한 객체의 상태가 바뀌면 그 객체에 의존하는 다른 객체들한테 연락이 가고, 자동으로 내용이 갱신되는 방식으로 일대다(one-to-many) 의존성을 정의한다.
>

![7](https://user-images.githubusercontent.com/39437170/208501639-2a6bf1fd-dd23-4680-9f20-a4c006ae47d2.png)

코치인 베디는 모든 크루들에게 정보를 알려야 한다.

크루(크루 인터페이스)의 기능은 자신의 상태를 업데이트하는 기능을 가진다. 아주 간단하게 하나의 기능을 가지고 있다고 하자.

위의 도식대로 인터페이스를 정의해보자.

```java
package observer_pattern.test;

public interface Coach {
	void subscribe(Crew crew);
	void unsubscrib(Crew crew);
	void notifyCrew(String msg);

}
```

```java
package observer_pattern.test;

public interface Crew {
	void update(String msg);
}
```

크루원들은 모두 스스로 업데이트하는 기능을 가지고 있고 주체는 이를 옵저버들에게 연락한다.

```java
package observer_pattern.test;

import java.util.ArrayList;
import java.util.List;

public class Bedi implements Coach{
	private List<Crew> crews=new ArrayList<>();

	public void eatFood(){
		System.out.println("베디 코치가 밥을 먹는다");
		notifyCrew("나 밥먹었어");
	}

	public void runAway(){
		System.out.println("베디 코치가 농땡이 친다");
		notifyCrew("나 농땡이쳤어");
	}

	public void updateCombat(){
		System.out.println("베디가 나노강화제를 주입했다");
		notifyCrew("너흰 강해졌다 돌격해");
	}

	@Override
	public void subscribe(Crew crew) {
		crews.add(crew)
	}

	@Override
	public void unsubscrib(Crew crew) {
		crews.remove(crew);
	}

	@Override
	public void notifyCrew(String msg) {
		crews.forEach(crew -> crew.update(msg));
	}
}
```

베디(코치)는 **Crew들의 리스트를 가지고 있고**, 세 가지 기능을 가진다. **밥 먹기, 농땡이 치기, 강력해 지기**

그리고 인터페이스에 정의된 대로 3개의 함수를 구현한다. **주목할 것은 notifyCrew 메서드를 각 기능에서 호출한다는 것**이다. 그리고 크루들에게 한 명씩 업데이트 메서드를 호출하게 한다. (이 부분이 알림을 보내는 부분이다)

이때 티버(크루)는 베디(코치)의 알림을 받고 싶어서 구독을 하고 싶어 한다.

```java
package observer_pattern.test;

public class Tiber implements Crew{
	@Override
	public void update(String msg) {
		System.out.println("Tiber 구독:"+msg);
	}
}
```

이를 본 루인과 제이도 베디의 연락을 받고싶어진다.

```java
package observer_pattern.test;

public class Lewin implements Crew{

	@Override
	public void update(String msg) {
		System.out.println("Lewin 구독:"+msg);
	}
}
```

```java
package observer_pattern.test;

public class Jay implements Crew{
	@Override
	public void update(String msg) {
		System.out.println("Jay 구독:"+msg);
	}
}
```

이제 베디가 훈련을 시작한다.

```java
package observer_pattern.test;

public class Main {
	public static void main(String[] args) {
		Bedi bedi = new Bedi();
		Crew jay = new Jay();
		Crew lewin = new Lewin();
		Crew tiber = new Tiber();

		bedi.subscribe(jay);
		bedi.subscribe(lewin);
		bedi.subscribe(tiber);

		bedi.updateCombat();
	}
}
```

![8](https://user-images.githubusercontent.com/39437170/208501714-1138c8de-2636-42c7-9336-27cd8467df04.png)


하지만 Tiber는 이에 불만이 존재하여 베디에게 그만 가르침을 받으려한다.

```java
bedi.unsubscrib(tiber);
bedi.runAway();
```

![9](https://user-images.githubusercontent.com/39437170/208501748-0ba0592f-de85-4f04-b6b5-14e360e2206d.png)

결과적으로 TIber는 구독자에서 빠지게되어 알림이 가지않게된다.

### 옵저버 패턴의 장단점

### 장점

**1.** 실시간으로 한 객체의 변경사항을 다른 객체에 전파할 수 있습니다.

**2.** 느슨한 결합으로 시스템이 유연하고 객체간의 의존성을 제거할 수 있다.

### **단점**

**1.** 너무 많이 사용하게 되면, 상태 관리가 힘들 수 있습니다

**2.** 데이터 배분에 문제가 생기면 자칫 큰 문제로 이어질 수 있습니다.

### 느슨한 결합

두 객체가 느슨하게 결합되어 있다는 것은 상호작용을 하긴 하지만 서로의 정보를 알지못한다는 것 즉, 캡슐화를 잘 지킨상태라는 것이다. 옵저버 패턴은 이를 주체와 옵저버에 적용시켜 사용한다.

### 느슨한 결합의 장점

1. 옵저버를 언제든 새로 추가, 제거할 수 있다.
2. 새로운 형식의 옵저버라 할 지라도 주제를 전혀 변경할 필요가 없다.
3. 주제와 옵저버는 서로 독립적이다.
4. 주제나 옵저버가 바뀌더라도 서로에게 영향을 미치지 않는다.

> 디자인원칙
>
>
> 서로 상호작용을 하는 객체 사이에서는 가능하면 느슨하게 결합하는 디자인을 사용해야 한다.
>

## 프록시 패턴

객체를 사용하고자 할때 객체를 직접적으로 참조하는 것이 아닌 대항객체를 통해 접근하여 메모리의 이점과, 실체 객체의 생성을 미루어 기본적인 정보를 객체의 생성없이 이용할 수 있다는 장점이 존재한다.

스프링에서는 이를 JPA에서 사용하고있다.

### 프록시 패턴의 장단점

**프록시패턴의 장점**

- 사이즈가 큰 객체가 로딩되기 전에도 프록시를 통해 참조를 할 수 있다.
- 실제 객체의 public, protected 메소드를 숨기고 인터페이스를 통해 노출시킬 수 있다.
- 로컬에 있지 않고 떨어져있는 객체를 사용할 수 있다.
- 원래 객체에 접근에 대해 사전처리를 할 수 있다.

**프록시패턴 단점**

- 객체를 생성할 때 한 단계를 거치게 되므로, 빈번한 객체 생성이 필요한 경우 성능이 저하될 수 있다.
- 프록시 내부에서 객체 생성을 위해 스레드가 생성, 동기화가 구현되어야 하는 경우 성능이 저하될 수 있다.
- 로직이 난해해져 가독성이 떨어질 수 있다.

### 프록시 패턴의 종류

**가상프록시**

꼭 필요로 하는 시점까지 객체의 생성을 연기하고, 해당 객체가 생성된 것 처럼 동작하도록 만들고 싶을 때 사용하는 패턴이다. 프록시 클래스에서 작은 단위의 작업을 처리하고 리소스가 많이 요구되는 작업들이 필요할 경우만 주체 클래스를 사용하도록 구현한다.

**원격프록시**

원격 객체에 대한 접근을 제어 로컬 환경에 존재하며, 원격 객체에 대한 대변자 역할을 하는 객체 서로 다른 주소 공간에 있는 객체에 대해 마치 같은 주소 공간에 있는 것 처럼 동작하게 하는 패턴이다.(예: Google Docs)

**보호프록시**

주체 클래스에 대한 접근을 제어하기 위한 경우에 객체에 대한 접근 권한을 제어하거나 객체마다 접근 권한을 달리하고 싶을 경우 사용하는 패턴으로 프록시 클래스에서 클라이언트가 주체 클래스에 대한 접근을 허용할지 말지 결정하도록 할 수 있다.

### 프록시 서버

![10](https://user-images.githubusercontent.com/39437170/208501809-930ea4f0-cdb8-499a-9b4a-df3a44e361bf.png)

클라이언트와 서버 사이에서 클라이언트가 자신을 통해 다른 네트워크 서비스에 간접적으로 접속할 수 있게해주는 컴퓨터 시스템 또는 응용프로그램.

서버와 클라이언트 사이에 중계기로서 대리로 통신을 수행하는 것을 가리켜 '프록시', 그 중계 기능을 하는 것을 프록시 서버라고 부른다.

### 스프링에서의 프록시 서버

대표적인 예시로 AOP가  존재한다.

AOP는 `Aspect Oriented Programming`이다. `관점 지향 프로그래밍`으로 번역되며 중점적인 역할은 핵심 기능들 사이에 공유하는 관심사항을 삽입하여 코드의 중복을 줄이는 방식의 패러다임.

간단히 예를 들면, 우리가 만든 프로그램에 모든 메소드 호출 시마다 로그를 찍고 싶을 때 로그를 찍는 것은 핵심 모듈이 아니기에 개발한 코드에는 로그를 찍는 기능은 개발이 되어 있지 않을 것이다. 그럼 무수한 메소드에 직접 로그를 입력하는 함수를 짜야 하는데, 비효율적이다.

또한 객체지향의 관점에서 핵심 로직이 이런 부수적인 기능을 책임져야 한다는 것은 관심사의 분리에 어긋난다. 그래서 스프링은 AOP라는 이름으로 부수적인 기능을 책임져줄 기능을 만들어짐. 이는 의존관계 설정을 도맡아 해주는 IoC 기법인 DI와 유사

AOP는 핵심 모듈 사이에 필요한 기능을 삽입하여 적절한 타이밍에 호출되도록 해주는 기능.이를 구현할 때 스프링은 대상 빈을 프록시로 감싸는 방법을 활용.