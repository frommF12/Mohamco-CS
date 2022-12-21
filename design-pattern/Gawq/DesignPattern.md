
## 디자인 패턴이란?
>프로그램을 설계할 때 발생했던 문제점들을 객체 간의 상호 관계등을 이용하여 해결할 수 있도록 하나의 '규약' 형태로 만들어 놓은 것을 의미한다.

## 1. 싱글톤 패턴
싱글톤 패턴(singleton pattern)은 **하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴**이다. 하나의 클래스를 기반으로 여러 개의 개별적인 인스턴스를 만들 수 있지만, 그렇게 하지 않고 하나의 클래스를 기반으로 단 하나의 인스턴스를 만들어 이를 기반으로 로직을 만드는 데 쓰이며, 보통 데이터베이스 연결 모듈에 많이 사용한다.

하나의 인스턴스를 만들어 놓고 해당 인스턴스를 다른 모듈들이 공유하며 사용하기 때문에 **인스턴스를 생성할 때 드는 비용이 줄어드는 장점**이 있다. 하지만 **의존성이 높아진다는 단점**이 있다.

```java
class Singleton {
	private static class singleInstanceHolder {
    	private static final Singleton INSTANCE = new Singletion();
    }
    public static Singleton getInstance() {
    	return singleInstanceHolder.INSTANCE;
    }
}

public class HelloWorld {
	public static void main(String[] args) {
    	Singleton a = Singleton.getInstance();
      	Singleton b = Singleton.getInstance();
        System.out.println(a.hashcode());
        System.out.println(b.hashcode());
        if(a == b) {
        	System.out.println(true):
        }
    }
}
/*
705927765
705927765
true
*/
```

### 1.1. 싱글톤 패턴의 단점
싱글톤 패턴은 TDD(Test Driven Development)를 할 때 걸림돌이 된다. TDD를 할 때 단위 테스트를 주로 하는데, 단위 테스트는 테스트가 서로 독립적이어야 하며 테스트를 어떤 순서로든 실행할 수 있어야 한다.

하지만 싱글톤 패턴은 미리 생성된 하나의 인스턴스를 기반으로 구현하는 패턴이므로 각 테스트마다 '독립적인' 인스턴스를 만들기가 어렵다.

### 1.2. 의존성 주입
또한, 싱글톤 패턴은 사용하기가 쉽고 굉장히 실용적이지만 모듈 간의 결합을 강하게 만들 수 있다는 단점이 있다. 이때 의존성 주입(DI, Denpendency Injection)을 통해 모듈 간의 결합을 조금 더 느슨하게 만들어 해결할 수 있다.

참고로 의존성이란 종속성이라고도 하며 A가 B에 의존성이 있다는 것은 B의 변경 사항에 대해 A 또한 변해야 된다는 것을 의미한다.

#### 1.2.1. 의존성 주입의 장점
모듈들을 쉽게 교체할 수 있는 구조가 되어 테스팅하기 쉽고 마이그레이션하기도 수월하다. 또한, 구현할 때 추상화 레이어를 넣고 이를 기반으로 구현체를 넣어 주기 때문에 애플리케이션 의존성 방향이 일관되고, 애플리케이션을 쉽게 추론할 수 있으며, 모듈 간의 관계들이 조금 더 명확해진다.

#### 1.2.2. 의존성 주입의 단점
모듈들이 더욱더 분리되므로 클래스 수가 늘어나 복잡성이 증가될 수 있으며 약간의 런타임 페널티가 생기기도 한다.

#### 1.2.3. 의존성 주입 원칙
의존성 주입은 "상위 모듈은 하위 모듈에서 어떠한 것도 가져오지 않아야 한다. 또한, 둘 다 추상화에 의존해야 하며, 이때 추상화는 세부 사항에 의존하지 말아야 한다." 라는 의존성 주입 원칙을 지켜주면서 만들어야 한다.

## 2. 팩토리 패턴
팩토리 패턴(factory pattern)은 객체를 사용하는 코드에서 객체 생성 부분을 떼어내 추상화한 패턴이자 상속 관계에 있는 두 클래스에서 상위 클래스가 중요한 뼈대를 결정하고, 하위 클래스에서 객체 생성에 관한 구체적인 내용을 결정하는 패턴이다.

상위 클래스와 하위 클래스가 분리되기 때문에 느슨한 결합을 가지며 상위 클래스에서는 인스턴스 생성 방식에 대해 전혀 알 필요가 없기 때문에 더 많은 유연성을 갖게 된다. 그리고 객체 생성 로직이 따로 떼어져 있기 때문에 코드를 리팩터링하더라도 한 곳만 고칠 수 있게 되니 유지 보수성이 증가된다.

```java
abstract class Coffee {
	public abstract int getPrice();

    @Override
    public String toString() {
    	return "Hi this coffee is " + this.getPrice();
    }
}
class CoffeeFactory {
	public static Coffee getCoffee(String type, int price) {
    	if ("Latte".equalsIgnoreCase(type)) return new Latte(price);
        else if ("Americano".equalsIgnoreCase(type)) return new Americano(price);
        else {
        	return new DefaultCoffee();
        }
    }
}
class DefaultCoffee extends Coffee {
	private int price;

    public DefaultCoffee() {
   		this.price = -1;
   	}

    @Override
    public int getPrice() {
    	return this.price;
    }
}
class Latte extends Coffee {
	private int price;

    public Latte(int price) {
   		this.price = price;
   	}

    @Override
    public int getPrice() {
    	return this.price;
    }
}
class Americano extends Coffee {
	private int price;

    public Americano(int price) {
   		this.price = price;
   	}

    @Override
    public int getPrice() {
    	return this.price;
    }
}
public class HelloWorld {
	public static void main(String[] args) {
    	Coffee latte = CoffeeFactory.getCoffee("Latte", 4000);
        Coffee ame = CoffeeFactory.getCoffee("Americano", 3000);
        System.out.println("Factory latte ::" + latte);
        System.out.println("Factory ame ::" + ame);
    }
}
/*
Factory latte ::Hi this coffee is 4000
Factory ame ::Hi this coffei is 3000
*/
```
지금 보면 if ("Latte.equalsIgnoreCase(type))을 통해 문자열 비교 기반으로 로직이 구성됨을 볼 수 있는데, 이는 Enum 또는 Map을 이용하여 if문을 쓰지 않고 매핑해서 할 수도 있다.

>**Enum :**
상수의 집합을 정의할 때 사용되는 타입이다. 상수나 메서드 등을 집어넣어서 관리하며 코드를 리팩터링할 때 해당 집합에 관한 로직 수정 시 이 부분만 수정하면 되므로 코드 리팩터링 시 강점이 생긴다.

## 3. 전략 패턴
전략 패턴(strategy pattern)은 정책 패턴(policy pattern)이라고도 하며, 객체의 행위를 바꾸고 싶은 경우 '직접' 수정하지 않고 전략이라고 부르는 '캡슐화한 알고리즘'을 컨텍스트 안에서 바꿔주면서 상호 교체가 가능하게 만드는 패턴이다.

```java
import java.text.DecimalFormat;
import java.util.ArrayList;
import java.util.List;
interface PaymentStategy {
	public void pay(int amount);
}

class KAKAOCardStrategy implements PaymentStrategy {
	private String name;
    private String cardNumber;
    private String cvv;
    private String dateOfExpiry;

    public KAKAOCardStrategy(String nm, String ccNum, String cvv, String expiryDate) {
    	this.name = nm;
        this.cardNumber = ccNum;
        this.cvv = cvv;
        this.dateOfExpriy = expiryDate;
    }

    @Override
    public void pay(int amount) {
    	System.out.println(amount + " paid using KAKAOCard.");
    }
}

class LUNACardStrategy implements PaymentStrategy {
	private String emailId;
    private String password;

    public LUNACardStrategy(String email, String pwd) {
    	this.emailId = email;
        this.password = pwd;
    }

    @Override
    public void pay(int amount) {
    	System.out.println(amount + " paid using LUNACard.");
    }
}

class Item {
	private String name;
    private int price;
    public Item(String name, int cost) {
    	this.name = name;
        this.price = cost;
    }

    public String getName() {
    	return name;
    }

    public int getPrice() {
    	return price;
    }
}

class ShoppingCart {
	List<Item> items;

    public ShoppingCart() {
    	this.items = new ArrayList<Item>();
    }

    public void addItem(Item item) {
    	this.items.add(item);
    }

    public void removeItem(Item item) {
    	this.items.remove(item);
    }

    public int claculateTotal() {
    	int sum = 0;
        for (Item item : items) {
        	sum += item.getPrice();
        }
        return sum;
    }

    public void pay(PaymentStrategy paymentMethod) {
    	int amount = calculateTotal();
        paymentMethod.pay(amount);
    }
}

public class HelloWorld {
	public static void main(String[] args) {
    	ShoppingCart cart = new ShoppingCart();

        Item A = new Item("kundolA", 100);
        Item B = new Item("kundolB", 300);

        cart.addItem(A);
        cart.addItem(B);

        // pay by LUNACard
        cart.pay(new LUNACardStrategy("kundol@example.com", "pukubababo"));

        // pay by KAKAOCard
        cart.pay(new KAKAOCardStrategy("Ju hongchul", "123456789", "123", "12/01"));
    }
}
/*
400 paid using LUNACard
400 paid using KAKAOCard
*/
```
앞의 코드는 쇼핑 카트에 아이템을 담아 LUNACard 또는 KAKAOCard라는 두 개의 전략으로 결제하는 코드이다.
>**컨텍스트 :**
프로그래밍에서의 컨텍스트는 상황, 맥락, 문맥을 의미하며 개발자가 어떠한 작업을 완료하는 데 필요한 모든 관련 정보를 말한다.

## 4. 옵저버 패턴
옵저버 패턴(observer pattern)은 주체가 어떤 객체(subject)의 상태 변화를 관찰하다가 상태 변화가 있을 때마다 메서드 등을 통해 옵저버 목록에 있는 옵저버들에게 변화를 알려주는 디자인 패턴이다.

여기서 주체란 객체의 상태 변화를 보고 있는 관찰자이며, 옵저버들이란 이 객체의 상태 변화에 따라 전달되는 메서드 등을 기반으로 '추가 변화 사항'이 생기는 객체들을 의미한다.

또한 주체와 객체를 따로 두지 않고 상태가 변경되는 객체를 기반으로 구축하기도 한다.

옵저버 패턴을 활용한 서비스로는 **트위터**가 있다.

또한, 옵저버 패턴은 주로 이벤트 기반 시스템에 사용하며 MVC(Model-View-Controller)패턴에도 사용된다.

예를 들어 주체라고 볼 수 있는 모델(model)에서 변경 사항이 생겨 update() 메서드로 옵저버인 뷰에 알려주고 이를 기반으로 컨트롤러(controller) 등이 작동하는 것.
```java
import java.util.ArrayList;
import java.util.List;

interface Subject {
	public void register(Observer obj);
    public void unregister(Observer obj);
    public void notifyObservers();
    public Object getUpdate(Observer obj);
}

interface Observer {
	public void update();
}

class Topic implements Subject {
	private List<Observer> observers;
    private String message;

    public Topic() {
    	this.observers = new ArrayList<>();
        this.message = "";
    }

    @Override
    public void register(Observer obj) {
    	if (!observers.contains(obj)) observers.add(obj);
    }

    @Override
    public void unregister(Observer obj) {
    	observers.remove(obj);
    }

    @Override
    public void notifyObservers() {
    	this. observers.forEach(Observer::update);
    }

    @Overried
    public Object getUpdate(Observer obj) {
    	return this.message;
    }

    public void postMessage(String msg) {
    	System.out.println("Message sended to Topic : " + msg);
        this.message = msg;
        notifyObservers();
    }
}

class TopicSubscriber implements Observer {
	private String name;
    private Subject topic;

    public TopicSubscriber(String name, Subject topic) {
    	this.name = name;
        this.topic = topic;
    }

    @Override
    public void update() {
    	String msg = (String) topic.getUpdate(this);
        System.out.println(name + ":: got message >> " + msg);
    }
}

public class HelloWorld {
	public static void main(String[] args) {
    	Topic topic = new Topic();
        Observer a = new TopicSubscriber("a", topic);
        Observer b = new TopicSubscriber("b", topic);
        Observer c = new TopicSubscriber("c", topic);
        topic.register(a);
        topic.register(b);
        topic.register(c);

        topic.postMessage("amumu is op champion!!");
    }
}
/*
Message sended to Topic: amumu is op champion!!
a:: got message >> amumu is op champion!!
b:: got message >> amumu is op champion!!
c:: got message >> amumu is op champion!!
*/
```
topic을 기반으로 옵저버 패턴을 구현했다. 여기서 topic은 주체이자 객체가 된다. `class Topic implements Subject`를 통해 Subject interface를 구현했고, `Observer a = new TopicSubscriber("a", topic);`으로 옵저버를 선언할 때 해당 이름과 어떠한 토픽의 옵저버가 될 것인지를 정했다.

### 5.1. 자바: 상속과 구현
<p style="color:pink">상속</p>
상속(extends)은 자식 클래스가 부모 클래스의 메서드 등을 상속받아 사용하며 자식 클래스에서 추가 및 확장을 할 수 있는 것을 말한다. 이로 인해 재사용성, 중복성의 최소화가 이루어진다.
<p style="color:pink">구현</p>
구현(implements)은 부모 인터페이스(interface)를 자식 클래스에서 재정의하여 구현하는 것을 말하며, 상속과는 달리 반드시 부모 클래스의 메서드를 재정의하여 구현해야 한다.
<p style="color:pink">상속과 구현의 차이</p>
상속은 일반 클래스, abstract 클래스를 기반으로 구현하며, 구현은 인터페이스를 기반으로 구현한다.

## 6. 프록시 패턴과 프록시 서버
### 6.1. 프록시 패턴
프록시 패턴(proxy pattern)은 대상 객체(subject)에 접근하기 전 그 접근에 대한 흐름을 가로채 대상 객체 앞단의 인터페이스 역할을 하는 디자인 패턴입니다.

이를 통해 객체의 속성, 변환 등을 보완하며 보안, 데이터 검증, 캐싱, 로깅에 사용한다.
### 6.2. 프록시 서버
프록시 서버(proxy server)는 서버와 클라이언트 사이에서 클라이언트가 자신을 통해 다른 네트워크 서비스에 간접적으로 접속할 수 있게 해주는 컴퓨터 시스템이나 응용 프로그램을 가리킨다.

#### 6.2.1. 프록시 서버로 쓰는 CloudFlare
CloudFlare는 전 세계적으로 분산된 서버가 있고 이를 통해 어떠한 시스템의 콘텐츠 전달을 빠르게 할 수 있는 CDN 서비스이다.

CDN 말고도 CloudFlare를 통해 누릴 수 있는 이점은 많다. 대표적으로 DDOS 공격 방어, HTTPS 구축이 있다. 이 모든 것은 웹 서버 앞단에 두어 '프록시 서버'로 쓰기 때문에 가능한 것이다.

##### 6.2.1.1. DDOS 공격 방어
DDOS는 짧은 기간 동안 네트워크에 많은 요청을 보내 네트워크를 마비시켜 웹 사이트의 가용성을 방해하는 사이버 공격 유형이다. CloudFlare는 의심스러운 트래픽, 특히 사용자가 접속하는 것이 아닌 시스템을 통해 오는 트래픽을 자동으로 차단해서 DDOS 공격으로부터 보호한다. CloudFlare의 거대한 용량과 캐싱 전략으로 소규모 DDOS 공격은 쉽게 막아낼 수 있으며 이러한 공격에 대한 방화벽 대시보드도 제공한다.

##### 6.2.1.2. HTTPS 구축
서버에서 HTTPS를 구축할 때 인증서를 기반으로 구축할 수도 있다. 하지만 CloudFlare를 사용하면 별도의 인증서 설치 없이 좀 더 손쉽게 HTTPS를 구축할 수 있다.
