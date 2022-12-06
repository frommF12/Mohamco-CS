# Scanner & BufferedReader

얼마 전에 코드리뷰를 하면서 Scanner 말고 BufferedReader로도 **사용자(키보드) 입력**을 받을 수 있다는 것을 알게 되어서 Scanner와 BufferedReader의 차이가 무엇인지 궁금해서 알아보았다.

### **Scanner와 BufferedReader의 사용법**

---

```java
import java.util.Scanner;

public class Input {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);

		String input = sc.nextLine();
	}
}
```

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Input {
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	
		String input = br.readLine();
	}
}
```

Scanner와 BufferedReader 클래스 둘 다 사용자(키보드) 입력을 받을 수 있는 기능을 제공한다.

### **Scanner**

---

Scanner는 입력 받을 때 정수 값, 소수 값, 문자 데이터도 구분지어 읽어들일 수 있다.

즉 직관적이고 사용하기에 편리하다는 장점이 있다.

그렇지만 Scanner는 입력을 읽는 과정에서 내부에서 정규 표현식 적용, 입력값 분할, 파싱 과정 등을 거치고, 키보드의 입력이 키를 누르는 즉시 바로 전달되기 때문에 버퍼를 사용하는 BufferReader 보다 **속도 면에서 불리하다는 큰 단점**이 존재한다.

### **BufferedReader**

---

버퍼리더는 입력 받은 값을 기본적으로 8192 char (16394 byte) 크기의 버퍼에 담아두었다가 한번에 프로그램에 전송한다.

어쩌면 입력받은 즉시 전송하는 Scanner가 더 효율적이라고 생각될 수 있겠지만 버퍼링 없이 전송하는 것은 CPU와의 성능 차이 때문에 버퍼를 사용하는 방식보다 속도에서 비효율적이라고 할 수 있다.

과일을 수확할 때, 하나의 과일을 딸 때마다 창고로 옮겨주는 것과 수확한 과일을 한 수레에 모아 한번에 창고로 옮겨주는 것의 효율 차이를 예로 들 수 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/240bc99d-0832-4f95-8bf8-28688b9176d4/Untitled.png)

백준님이 작성하신 입력 속도 비교 글을 보면 평균적으로 **BufferedReader** 사용 시에 **0.6585초**, **Scanner** 사용 시에 **4.8448초**가 걸리는 차이가 있다. 적은 양의 데이터인 경우 문제가 없겠지만, 데이터 양이 많아질수록 성능 차이는 크다.

[https://www.acmicpc.net/blog/search/입력+속도](https://www.acmicpc.net/blog/search/%EC%9E%85%EB%A0%A5+%EC%86%8D%EB%8F%84)

하지만 BufferedReader는 문자열 String값 밖에 읽지 못하기 때문에 활용적인 측면에서는 여러가지 타입을 입력 받을 수 있는 Scanner가 더 좋다고 볼 수 있다.

---

두 클래스가 공통적으로 생성할 때에 `System.in`을 매개변수로 받는 것을 볼 수 있다.

System.in은 java.lang 패키지의 System 클래스이며, System 클래스의 in은 정적(static) 변수이다

```java
public final class System {
	public static final InputStream in = null;
	...
}
```

in은 InputStream 타입의 변수임을 볼 수 있다.
InputStream은 [java.io](http://java.io) 패키지의 바이트 단위 입력을 위한 최상위 입력 스트림 클래스이다.

[System.in](http://System.in)을 매개변수로 사용하기 때문에 사용자(키보드) 입력을 받을 수 있다고 생각하면 된다. 다음과 같이 **System.in만을 사용하여 입력을 받는 것이 가**능하다.

```java
import java.io.IOException;

public class Input {
	public static void main(String[] args) throws IOException {
		int input = System.in.read();
	}
}
```

System.in과 BufferedReader 클래스를 사용할 때는 옆에 throws IOException이 붙는다. 사용자의 입력은 여러 다양한 타입으로 들어올 수 있는데, 잘못된 값이 들어올 경우 에러가 나지 않도록 예외처리를 해줘야한다.

Scanner는 System.in을 생성시에 내부에서 try-catch를 사용하여 예외처리를 하기 때문에 예외처리를 따로 하지 않아도 된다.

위의 코드를 보면 Scanner와 달리 BufferedReader는 생성 시에 매개변수로 InputStreamReader를 사용한다.
Scanner 또한 클래스 생성자에서 InputStreamReader를 생성하여 사용한다.

```java
public final class System {
	public Scanner(InputStream source) {
		this(new InputStreamReader(source), WHITESPACE_PATTERN);
	}
	...
}
```

InputStreamReader란 문자 입력 스트림의 한 종류로 입력 장치(키보등 등)으로부터 받은 입력 값을 자바 응용 프로그램으로 전달하는 객체이다. 자바 응용 프로그램은 입력 장치로부터 직접 데이터를 읽지 않고 입력 스트립을 통해 데이터를 읽는다.

**결론**

---

많은 양을 받아들일 때는 버퍼를 사용해 속도가 빠른 BufferedReader를 사용하는 것이 좋지만, 그렇지 않을 경우 Scanner를 사용하는 것이 활용적이라고 볼 수 있다.