# Getter와 Setter를 지양해야하는 이유

![Lombok](https://avatars.githubusercontent.com/u/45949248?s=280&v=4)

스프링부트를 자바 Language로 사용해오던 나에게 Lombok 라이브러리의 `@Getter` `@Setter` 어노테이션은 혁신과도 같은 것이었다.

그 귀찮은 Getter와 Setter를 직접구현하지 않고도 어노테이션 하나만으로 구현이 가능하기 때문이다.

하지만 프로젝트의 크기가 커져갈 수록 어노테이션이 남발되게 되고 필요없는 상황에서도 습관적으로 사용하기 때문에 이렇게 무차별적 사용을 해도 괜찮은가? 에 대한 의문이 들게되어 해당 글을 작성하게 되었다.

## Setter는 왜 지양되어야 하는가?

1. Entity에서의 경우에는 DB와 직접적으로 연결되는 클래스로 DB에 값이 직접적으로 수정이 되는 로직은 안정적이지 못하다.
2. 유지보수적 측면에서 값을 변경한 의도를 파악하기 어려워져 코드를 역추적하여 해당 값의 변경의의를 자신이 짠 코드임에도 알 수 없게된다.
3. 필드가 많아질 경우 가독성이 떨어지게 된다.
4. 객체의 일관성이 떨어지게 된다. 이 말은 예를 들어 회원정보 수정 API가 존재한다고 했을 때 Setter를 통해 무분별하게 값이 변경될 수 있다면 API 그 자체의 의미가 사라지게된다.
5. 의도치 않은 값의 변경이 일어날 수 있다. 이를 데이터의 무결성을 해친다고 표현한다.
6. 자바는 객체지향이지만 절차지향적인 프로그래밍이 되어진다.

## Setter의 대처방법?

1. 빌더 패턴을 사용하여 DTO 클래스를 활용하여 값을 변경하는 것이 확실한 방법이다.
   메소드의 Naming을 해당 수정과 어떤 관계가 있는지 유의미하게 짓고 선택적으로 필요한 변수는 초기화 하는 방법을 통해 변경감지를 통해 객체단위의 수정을 사용하는 방법이 좋다고 본다.
   객체단위의 수정을 이용할 경우 모든 필드에 관하여 Setter가 들어가지 않기에 가독성에서 이득적인 측면을 볼 수도 있다.
2. 엔티티의 경우 변경감지 (Dirty Checking)와 빌더패턴을 이용하여 값을 변경한다.

```java
Member findMember=memberRepository.findById(user_Seq);

findMember.updatePwOrNickname(userUpdateInfoDTO);
```

```java
public class UserDTO{
		public static void UserUpdateInfoDTO{
					private String password;
					private String Nickname;

					public 
		}
}
```

```java
public class Member{
	....
	

		public void updatePworNickname(UserDTO.UserUpdateInfoDTO userUpdateInfoDTO){
					this.password=userUpdateInfoDTO.getPassword();
					this.nickname=userUpdateInfoDTO.getNickName();
		}
}
```

변경감지 기법은 현재 상당히 많이 사용되는 방법이긴 하지만 여기서 벌써 두가지 문제가 발생하게 된다.
1. DTO의 값을 초기화하기 위해선 setter를 통해 DTO에 값을 넣어주어야한다.
2. 우리는 Getter 또한 지양하는 이유도 알아보기로 하였다.하지만 변경감지를 위해선 get 메소드를 여전히 활용중이다.

이를 통해 우리는 Entity에서의 무분별한 Setter를 지양하되 DTO의 경우 선택적인 사용은 불가피하다는 점이있다. Getter는 다음을 살펴보도록하자

## Getter는 왜 지양되어야 하는가?

기본적으로 우리가 Entity를 설계함에 있어 자바의 기본 원칙 중 하나인 캡슐화에 위배되기 때문이다.

대부분 Entity의 인스턴스 변수는 `private`으로 선언된다. 그런데 이때 Getter로 값을 아무대서나 가져올 수 있다면? 이경우에는 private의 의미는 사라지게되고 코드의 효율성을 위해선 오히려 `public` 으로 선언되는 것이 좋다고 생각한다.




## Getter의 대처방법?

이는 캡슐화의 기본원칙인 객체는 자신의 인스턴스의 상태값을 모르는 채로 객체에 질문이 던져져야된다는 점을 지켜야한다. 이는 OOP의 디미터의 법칙이 좋은 예시가 될 수 있을것 같다.

디미터의 법칙은 하나의 객체가 타객체를 지나치게 많이 알게되면 그에 맞는 아키텍쳐를 설계를하게되어 결합도가 높아지고 종속적인 프로그래밍이 되는것을 개선하고자 나온 법칙이다.

이를 해결하기위해 객체는 타객체의 인스턴스를 몰라야하며 이를 통해 궁극적으로는 객체의 자율성과 응집성을 높이기 위한 법칙이다.

이 말만 들으면 이해가 쉽게 되지는 않을것이다. 코드로 살펴보자면 다음과 같다.

### 디미터의 법칙을 예제

우리는 일반회원등급의 사용자에게만 알림을 날리는 코드를 작성하고 싶을때 회원 Entity가 다음과 같이 작성되어 있다고 가정하자.

```java
@Getter
public class Member{

	private String userid;
	private Role role:

}

@Getter
public enum Role{
	GUEST,MEMBER,ADMIN

	private String authority;

	public Role(String authorithy){
			this.authorithy=authorithy;
	}
}
```

**디미터의 법칙 위반코드**

나는 평소 아래와 같은 방법으로 서비스레이어에 비즈니스 로직을 작성을 하였다.

```java
@Service
public class NotificationService{
		
		public void sendNotificationForMember(final Member member){
				if(Role.MEMBER.equals(member.getRole().getAuthority())){
						sendNotification(member);
				}
		}
}
```

이는 개발자가 클래스에 무슨 인스턴스가 존재하는지 알기 때문에 편의성을 위해 get으로 변수를 꺼내와 객체지향스럽지 못한 코드를 작성하고 있다.

이는 캡슐화의 원칙인 객체에게 메세지를 보내는 것이 아닌 객체의 자료를 명확히 알고있기에 디미터의 법칙을 위배하는 코드이다.

그렇다면 디미터의  법칙을 준수하는 코드는 무엇일까?
아까 말한 것과 같이 객체에 질문을 던져야한다. 이말은 상당히 이해가 가지않을 수 있으니 코드로 살펴보자면 다음과 같다.

```java

@Getter
public class Member{

	private String userid;
	private Role role;
	
	public boolean isMemberRole(){
			return role.isRoleMember();
	}
}

@Getter
public enum Role{
	GUEST,MEMBER,ADMIN;

	private String authority;

	public Role(String authorithy){
			this.authorithy=authorithy;
	}

	public boolean isRoleMember(){
			return Role.MEMBER.equals(authority);
	}

```

```java
@Service
public class NotificationService{
		public void sendMessageForMember(final Member member){
				if(member.isSeoulUser()) {
						sendNotification();
				}
		}
}
```

이럴 경우 도트가 눈에 띄게 줄어들어 가독성 적인 측면과 디미터의 법칙도 위배하지 않는다.
하지만 도트가 많다고 디미터의 법칙에 위배되는 것이 아니다 .예시로 Stream을 들 수 있을 것이다.

디미터의 법칙은 객체간의 결합도와 관련된 것이며 객체 내부의 구조가 외부에서 알 수 있는지가 가장 중요한 이슈로 작용된다.