# GoF 디자인 패턴 - 2 

![1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FpMqWb%2FbtrT6uMHywb%2FyvuzG19T6lqk4qLzh3xcX0%2Fimg.jpg)

## 이터레이터 패턴

- 행위 패턴에 속하며 이터레이터를 사용하여 컬렉션 프레임워크에 접근하는 디자인 패턴

> **이터레이터** : 모든 컬렉션 프레임워크에 공통으로 사용가능하며 값의 수정 및 삭제 추가연산 등을 이용할 수 있다.
>

> **컬렉션 프레임워크** : Java에서 정의하는 List,Set,Map,Queue와 같은 자료구조
>

### 이터레이터 패턴 정의

- 컬렉션 구현 방법을 노출시키지 않으면서 모든 항목에 관하여 접근할 수 있게해주는 방법을 제공해주는 패턴
- 객체들을 배열, 스택, 해시테이블 등의 컬렉션에 넣어서 보관할 수 있다. 그런데 클라이언트가 해당 객체들에게 일일이 접근하는 작업을 원할 수 있을 경우 사용한다.
- 이터레이터 패턴을 사용하면 동작원리는 모른채 사용한다.
- 객체를 저장하는 방식은 보여주지 않으면서도 클라이언트가 객체들에게 일일이 접근할 수 있게 해주는 방법이다.
- 반복작업을 Iterator 인터페이스를 이용하여 캡슐화

### 이터레이터 패턴 클래스 다이어그램

![2](https://user-images.githubusercontent.com/39437170/209640022-48ff88bc-8573-433e-ac79-1cc0933c9acc.png)

- iterator: 컬렉션의 요소들을 순서대로 검색하기 위한 인터페이스
- ConcreteInterface: iterator 인터페이스 구현체
- Aggregate: 여러 요소들로 구성된 컬렉션 인터페이스
- ConcreteAggregate: Aggregate 인터페이스 구현체

### 이터레이터 패턴 예시

아침 메뉴를 파는 팬케이크 하우스와 저녁 메뉴를 판매하는 식당이 합병하려고한다.

이 때, 서로 각자의 스타일로 메뉴를 구현해놨는데, 두 메뉴를 합쳐서 사용해야 한다. 두 메뉴를 합치는 방법에 대해 이야기 해보자.

참고로 팬케이크 하우스는 ArrayList를 이용하여 구현해놓았고, 식당은 정적 배열을 이용하여 구현해 놓았다.

두 메뉴를 합치는 것은 새로 뽑은 웨이트리스가 담당할 것이다.

**웨이트리스 클래스의 업무는 아래와 같다.**

**웨이트리스**

| Method Name | 역할 |
| --- | --- |
| printMenu() | 메뉴에 있는 모든 항목을 출력 |
| printBreakfastMenu() | 아침 식사 항목만 출력 |
| printDinnerMenu() | 저녁 식사 항목만 출력 |
| printVegetarianMenu() | 채식주의자용 메뉴 항목만 출력 |
| isItemVegetarian(name) | 해당 항목이 채식주의자용이면 true를 리턴하고 그렇지 않으면 false를 리턴 |

```java
package Iterator_Pattern.test;

import java.awt.*;

public classDinnerMenu {
static final intMAX_ITEMS = 6;
intnumberOfItems = 0;
   MenuItem[] menuItems;

publicDinnerMenu(){
      menuItems =newMenuItems[MAX_ITEMS];

      addItem("BLT",
            "통밀 위에 베이컨, 상추, 토마토를 얹은 메뉴",true, 2.99);
      addItem("오늘의 스프",
            "감자 샐러드를 곁들인 오늘의 스프",false, 3.29);
        ...
   }
public voidaddItem(String name, String description,
booleanvegetarian,doubleprice)
   {
      MenuItem menuItem =newMenuItem(name, description, vegetarian, price);
if(numberOfItems >= MAX_ITEMS){
         System.err.println("죄송합니다. 메뉴가 꽉 찼습니다. 더 이상 추가할 수 없습니다.");
      }else{
         menuItems[numberOfItems] = menuItem;
         numberOfItems = numberOfItems + 1;
      }
   }
publicMenuItem[] getMenuItems() {
returnmenuItems;
   }
}

```

```java
package Iterator_Pattern.test;

import java.awt.*;
import java.util.ArrayList;

public classPancakeHouseMenu {
   ArrayList menuItems;

public PancakeHouseMenu() {
      menuItems =newArrayList();

      addItem("레큘러 팬케이크 세트",
            "달걀 후라이와 소시지가 곁들여진 팬케이크",
						true,
            2.99);

      addItem("블루베리 팬케이크",
            "신선한 블루베리와 블루베리 시럽으로 만든 팬케이크",
						true,
            3.49);
   }
	
	public voidaddItem(String name, String description,boolean vegetarian,double price){
      MenuItem menuItem =newMenuItem(name, description, vegetarian, price);
      menuItems.add(menuItem);
	}

	public ArrayList getMenuItems(){
			return menuItems;
  }
}

```

웨이트리스 클래스에서 printMenu() 메소드를 구현하는 것을 상상해보자.

이런 경우, 웨이트리스 클래스에서는 두 음식점으로부터 각각getMenuItems를 호출하여 Items 에 접근한다.

두 음식점은 서로 다른 집합체를 사용하기 때문에 각 아이템을 접근하는 방식이 다르다. (arrayList.get(index) / array[index] )

따라서 통합된 인터페이스를 제공하여 웨이트리스 클래스(클라이언트)가 동일한 방식으로 각 아이템에 접근할 수 있게 해주기 위해 이터레이터 패턴을 적용하여 보자.

![3](https://user-images.githubusercontent.com/39437170/209640116-c1039875-db22-4b2c-a96d-66d8bb4d3bf4.png)

컬렉션에 다음 아이템이 있는지 여부 hasNext()를 확인하고, 다음 아이템을 next() 받아온다.

```java
public interface Iterator {
	boolean hasNext();
    Object next();
}
```

저녁 메뉴를 판매하는 식당에 Iterator 적용하면 코드는 아래와 같아진다.

```java
package Iterator_Pattern.test;

import java.awt.*;

public classDinnerMenu {
static final intMAX_ITEMS = 6;
intnumberOfItems = 0;
   MenuItem[] menuItems;

publicDinnerMenu(){
      menuItems =newMenuItems[MAX_ITEMS];

      addItem("BLT",
            "통밀 위에 베이컨, 상추, 토마토를 얹은 메뉴",true, 2.99);
      addItem("오늘의 스프",
            "감자 샐러드를 곁들인 오늘의 스프",false, 3.29);
        ...
   }
public voidaddItem(String name, String description,
booleanvegetarian,doubleprice)
   {
      MenuItem menuItem =newMenuItem(name, description, vegetarian, price);
if(numberOfItems >= MAX_ITEMS){
         System.err.println("죄송합니다. 메뉴가 꽉 찼습니다. 더 이상 추가할 수 없습니다.");
      }else{
         menuItems[numberOfItems] = menuItem;
         numberOfItems = numberOfItems + 1;
      }
   }
publicMenuItem[] getMenuItems() {
returnmenuItems;
   }
}

```

### 이러레이터 패턴 장점과 단점

- 장점
    - 집합체 클래스의 응집도를 높여준다.
    - 집합체 내에서 어떤 식으로 일이 처리되는지 알 필요 없이, 집합체 안에 들어있는 모든 항목에 접근 할 수 있게 해준다.
    - 모든 항목에 일일이 접근하는 작업을 컬렉션 객체가 아닌 이터레이터 객체에서 맡게 된다. 이렇게 하면, 집합체의 인터페이스 및 구현이 간단해질 뿐만 아니라, 집합체에서는 반복 작업에서 손을 떼고 원래 자신이 할 일에만 전념할 수 있다.

- 단점
    - 단순한 순회를 구현하는 경우 클래스만 많아져 복잡도가 증가할 수 있다.


## 노출모듈 패턴

### 노출모듈 패턴의 정의

노출 모듈 패턴은 즉시 실행함수를 통해 private,public 같은 접근 제어자를 만드는 패턴을 말한다.

자바의 경우 이미 접근 제어자가 명명되어 있어 사용이 가능하며 private, public, default, protected의 종류가 있으나 자바 스크립트의 경우 이를 직접 구현해야한다.

### 노출 모듈 패턴의 장단점

- 장점
    - private한 데이터를 제공할 수 있다
    - 전역 변수의 사용을 줄일 수 있다.
    - 함수와 변수를 지역화시켜 그 의미가 명확해진다.
    - 코드의 일관성 증가
    - 가독성 증가

- 단점
    - private 메소드 접근 방법 구현 및 구현이 어렵다.

## MVC 패턴

![4](https://user-images.githubusercontent.com/39437170/209640167-50fb96b9-f5a2-4568-b0bf-0e17f8e2bda0.png)

![5](https://user-images.githubusercontent.com/39437170/209640210-d27a7455-2f61-44bb-b493-6476102622bb.png)

### MVC 패턴 정의

MVC는 **Movel-View-Controller**의 약자로 소프트웨어 공학에서 고안된 개발방법론이며, 부담을 줄인다는 점에서 SOLID의 단일 책임 원칙과도 비슷한 면을 보이며, 예시로 **Servlet/Jsp**에서 비즈니스 로직과 뷰가 하나의 프로그램 파일에 묶여있는 것을 탈피하고자 Model과 비즈니스로직을 분리한 것이 있다.

1. **Model** : 모든 상태의 데이터를 뜻하며, 어플리케이션 로직 또한 이에 포함된다. 뷰와 컨트롤러에서 상태를 조작하거나 가져오기 위한 인터페이스를 제공한다.
2. **View** : 유저에게 보여지는 시각적 요소이며 모델을 직접적으로 구성하여 인터페이스로 제공한다.
3. **Controller** : View에게 Model을 라우팅해주는 역할을 수행한다. 뷰를 통해 입력된 값이나 모델을 뷰로 전달할때 Controller를 사용하여 전달한다.

### ****📲**** Model

데이터를 가진 객체를 모델이라고 지칭한다. 데이터는 내부의 상태에 대한 정보를 가질 수도 있고, 모델을 표현하는 이름 속성으로 가질 수 있다. 모델의 상태에 변화가 있을 때 컨트롤러와 뷰에 이를 전달하며,이를 통해 뷰는 최신의 결과를 보여줄 수 있고, 컨트롤러는 모델의 변화에 따른 적용 가능한 명령을 추가, 제거, 수정이 가능하다.

**모델의 규칙**

- 사용자가 편집하길 원하는 모든 데이터를 가지고 있어야만 함
- 뷰나 컨트롤러에 대해서 어떠한 정보도 알지 말아야 함
- 변경이 일어나면, 변경 통지에 대한 처리방법을 구현해야 함

### ****🖥️ View****

View는 클라이언트 측 기술은 HTML/CSS/Javascript들을 모와둔 컨테이너이너의 역할을 수행하며,사용자가 볼 결과물을 생성하기 위해 모델로부터 정보를 얻어온다.

**뷰의 규칙**

- 모델이 가지고 있는 정보를 따로 저장해서는 안됨
- 모델이나 컨트롤러와 같이 다른 구성 요소를 몰라야 함
- 변경이 일어나면, 변경 통지에 대한 처리방법을 구현해야 함

### ****🕹️ Controller****

사용자가 접근한 URL에 따라 사용자의 요청사항을 파악한 후에 그 요청에 맞는 데이터를 Model을 의뢰하고, 데이터를 View에 반영해서 사용자에게 알려준다.

모델에 명령을 보냄으로써 뷰의 상태를 변경할 수 있음 => (워드에서 문서 편집)컨트롤러가 관련된 모델에 명령을 보냄으로써 뷰의 표시 방법을 바꿀 수 있음 => (문서를 스크롤하는 것)

**컨트롤러의 규칙**

- 모델이냐 뷰에 대해서 알고 있어야 함
- 모델이나 뷰의 변경을 모니터링해야 함

### MVC 패턴 방식

![6](https://user-images.githubusercontent.com/39437170/209640265-9a0efc5d-3b91-4b5e-9df8-d8aef5962932.png)

- **모델 1 방식 :** 뷰와 로직을 모두 하나의 JSP페이지에서 처리하는 방식
    - **모델 1 방식의 장점**
        - 구조가 단순하여 사용이 간편
        - 숙련된 개발자와의 차이가 거의 없음
    - **모델 1 방식의 단점**
        - JSP 코드가 복잡해짐에 따라 유지보수가 어려움
        - 분업에 용이하지 못함

![7](https://user-images.githubusercontent.com/39437170/209640328-5bd247e1-b625-4d4f-a51e-ca6542db5f5d.png)

- **모델 2 방식** : 웹 브라우저 사용자의 요청을 서블릿(Controller)가 받아들여 해당 요청으로 View로 보여줄 것인지 Model로 보낼 것인지를 판단하여 전송하며 이는 Controller에 작성된 로직에 따라 결정
    - **모델 2 방식의 장점**
        - 분업이 모델 1에 비해 간편함
        - 유지보수성이 모델 1에 비해 높음
    - **모델 2 방식의 단점**
        - 설계가 어려움

### MVC 패턴 구동원리

![8](https://user-images.githubusercontent.com/39437170/209640360-17f70ba1-b121-4d7e-896f-e5698a3243bc.png)

MVC패턴은 Spring프레임워크와 JSP를 사용하는 웹 애플리케이션 개발에 많이 사용되고 있다. 이 때의 웹 애플리케이션에서의 MVC의 구동 원리는 위의 사진과 같다.

1. 웹 브라우저가 웹 서버에 웹 애플리케이션 실행을 요청한다. (MVC 구조가 WAS라고 보면 된다.)
2. 웹 서버는 들어온 요청을 처리할 수 있는 서블릿을 찾아서 요청을 전달한다. (Matching)
3. 서블릿은 모델 자바 객체의 메서드를 호출한다.
4. 데이터를 가공하여 값 객체를 생성하거나, JDBC를 사용하여 데이터베이스와의 인터랙션을 통해 값 객체를 생성한다.
5. 업무 수행을 마친 결과값을 컨트롤러에게 반환한다.
6. 컨트롤러는 모델로부터 받은 결과값을 View에게 전달한다.
7. JSP는 전달받은 값을 참조하여 출력할 결과 화면을 만들고 컨트롤러에게 전달한다.
8. 뷰로부터 받은 화면을 웹 서버에게 전달한다.
9. 웹 브라우저는 웹 서버로부터 요청한 결과값을 응답받으면 그 값을 화면에 출력한다.

### MVC 패턴 사용이점

- 비즈니스 로직과 UI로직을 분리하여 유지보수를 독립적으로 수행가능
- Model과 View가 다른 컴포넌트들에 종속되지 않아 애플리케이션의 확장성, 유연성에 유리함
- 중복 코딩의 문제점 제거

### ****MVC 패턴의 한계****

![9](https://user-images.githubusercontent.com/39437170/209641786-ace51a19-30fb-40c5-8abf-73998e47dfb8.png)

Model과 View는 서로의 정보를 갖고 있지 않는 독립적인 상태라고 하지만 Model과 View사이에는 Controller를 통해 소통을 이루기에 의존성이 완전히 분리될 수 없습니다.

그래서 복잡한 대규모 프로그램의 경우 다수의 View와 Model이 Controller를 통해 연결되기 때문에 컨트롤러가 불필요하게 커지는 현상이 발생하기도 합니다.

이러한 현상을 **Massive-View-Controller** 현상이라고 하며 이를 보완하기 위해 MVP, MVVM, Flux, Redux등의 다양한 패턴들이 생겨났습니다.

### MVC에서 View가 업데이트 되는 방법

- View가 Model을 이용하여 직접 업데이트 하는 방법
- Model에서 View에게 Notify 하여 업데이트 하는 방법
- View가 Polling으로 주기적으로 Model의 변경을 감지하여 업데이트 하는 방법.

### MVC 사용처

백엔드 개발자인 내가 가장 가까이서 접할 수 있는 MVC 패턴 적용 모델로는 Spring MVC가 존재하며, View로는 Thymeleaf를 보편적으로 이용한다.

## MVP 패턴

![10](https://user-images.githubusercontent.com/39437170/209641860-a1901773-4a47-4bad-a1c5-b30c2c0084f4.png)

### MVP 패턴의 정의

MVP 패턴은 Model + View + Presenter를 합친 용어이며 Model과 View는 MVC 패턴과 동일하고, Controller 대신 Presenter가 존재한다.

- Model : 어플리케이션에서 사용되는 데이터와 그 데이터를 처리하는 부분
- View : 사용자에서 보여지는 UI 부분
- Presenter : View에서 요청한 정보로 Model을 가공하여 View에 전달해 주는 부분입니다. View와 Model의 라우터 역할을 수행

MVP 패턴은 Presenter와 View가 1:1 관계를 맺는다.

### MVC 대신 MVP를 사용하는 이유

MVC는 Model과 View의 의도치 않은 의존성이 생길 수밖에 없는 구조이다.

하지만 MVP는 Model과 View 분리되어 있고 오직 Presenter를 통해서 상태나 변화를 알려줄 수 있다.

![11](https://user-images.githubusercontent.com/39437170/209641905-7d10caaa-a0a3-48d9-a31c-3dfc8246a0ca.png)

위의 그림과 같이 비즈니스 로직이 완전히 분리되어 TDD 작성에 용이하다는 장점이 존재

하지만 MVP구조는 View와 Presenter의 의존관계가 강해진다는 단점이 존재하게된다.

### View 업데이트 동작원리

1. View에 사용자의 인터랙션이 들어온다.
2. View는 Presenter에 액션이 들어왔다고 전달한다.
3. Presenter는 View 액션대로 Model을 구성한다.
4. Update된 Presenter의 데이터를 View에 업데이트 합니다.

### MVP 장단점

- MVP 장점
    - MVP 패턴의 장점으로는 View와 Model의 의존성이 존재하지 않는다. 이는 이전 MVC패턴의 단점을 상쇄하여 의존성을 해결하였다.
- MVP 단점
    - 하지만 단점으로 View와 Presenter사이의 의존성이 심화되게된다.

## MVVM 패턴

![12](https://user-images.githubusercontent.com/39437170/209641957-3d5c3bc8-732e-481f-b0e4-9b40825e6738.png)

### MVVM 패턴 정의

![13](https://user-images.githubusercontent.com/39437170/209641983-96a80b9a-dfba-451e-b9d0-4d75ff92b703.png)

MVVM 패턴은 MVC 패턴에서 Controller를 빼고 ViewModel을 추가한 패턴으로 아키텍처 패턴에 속한다.

- View
    - iOS는 ViewController까지 View가 된다.
    - 사용자가 보여지는 View를 생각하면 된다. 유저 인터랙션을 받는 역할, 인터랙션을 받을 시 ViewModel에게 명령을 내린다.
- ViewModel
    - View를 표현하기 위해 만들어진 View를 위한 Model
    - View와는 Binding을 하여 연결후 View에게서 액션을 받고 또한 View를 업데이트를 수행
    - ex) textView에 보여줄 내용을 담당하는 함수 등, View에서 변화가 일어나는 ViewController의 역할을 담당
- Model
    - 데이터, 비즈니스 로직, 서비스 클라이언트 등으로 구성
    - 실제적 데이터

### MVVM 패턴 동작원리

1. View에 입력이 들어오면 ViewModel에게 명령
2. ViewModel은 필요한 데이터를 Model에게 요청
3. Model은 ViewModel에게 요청된 데이터를 응답
4. ViewModel은 응답 받은 데이터를 가공해서 저장
5. View는 ViewModel과의 Data Binding으로 인해 자동으로 갱신

### **Data Binding**

- 데이터 바인딩의 개념은 쉽게 말해 Model과 UI 요소 간의 싱크를 맞춰주는 것
- 이 패턴을 통해 View와 로직이 분리되어 있어도 한 쪽이 바뀌면 다른 쪽도 업데이트가 이루어져 데이터의 일관성 유지가 가능

### **MVVM 패턴의 장단점**

- 장점
    - View와 Model이 서로 전혀 알지 못하기에 독립성을 유지할 수 있다
    - 독립성을 유지하기 때문에 효율적인 유닛테스트가 가능 (TDD)
    - View와 ViewModel을 바인딩하기 때문에 코드의 양이 줄어든다
    - View와 ViewModel의 관계는 N:1 관계
    - 유닛테스트를 하기가 좋으며, 그 이유는 ViewModel을 사용함으로 Controller와의 의존성도 없기 때문
- 단점
    - 간단한 UI에서 오히려 ViewModel을 설계하는 어려움이 있을 수 있다
    - 데이터 바인딩이 필수적으로 요구
    - 복잡해질수록 Controller처럼 ViewModel이 빠르게 비대해진다
    - 표준화된 틀이 존재하지 않아 사람마다 이해가 다르다