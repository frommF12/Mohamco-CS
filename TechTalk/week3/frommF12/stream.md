## Stream?

스트림은 원하는 결과를 생성하기 위해 파이프라인을 만들 수 있는 객체의 시퀀스이다

스트림에 대한 다른 설명으로는 아래와 같다 

“equivalent of sequence from functional programming langunages” 

스트림은 함수형 프로그래밍의 sequence와 동일하다는 것이다

여기서 sequence는 task(작업)의 순서를 말한다

즉, 객체의 컬렉션 프로세스를 순차적으로 처리하여 결과를 만들어내는 것인데, 이 때 메서드를 파이프라인으로 연결할 수 있다는 것이다 

이러한 스트림은 객체지향 관점에서 Internal Iterator Pattern (내부 반복자 패턴)으로 본다

컬렉션의 순회를 클라이언트(사용자)가 아닌 스트림 내부에서 하는 것이다

→ 로직의 순회를 어디서 할 것이냐의 차이

**외부 반복자 패턴**

for, while문 처럼 요소를 사용하는 클라이언트가 직접 컬렉션 요소를 하나씩 꺼내서 반복처리

```
for (int i=0; i<array.length; i++) {
	system.out.println(array[i]);
}
```

**내부 반복자 패턴**

사용자는 action만 제공하고 액션을 컬렉션이 받아 내부적으로 처리하는 방식

```
Arrays.each( element -> System.out.println(element));
```

스트림의 특징은 다음과 같다

- 스트림은 자료구조가 아닌 컬렉션, 배열, I/O로 입력 받는다
- 스트림은 원래의 자료구조를 변경하지 않으며 각 파이프라인의 결과만 제공한다
- 스트림의 중간 연산의 실행은 lazy하며 결과로 스트림을 반환한다
- 스트림의 최종 연산은 스트림의 마지막을 표시하며 결과를 반환한다

## 생성 방법

스트림을 얻는 방법은 두 가지이다

**컬렉션 → 스트림**

```
testCollectoin.stream()
```

**배열 → 스트림**

```
Arrays.stream(testArray)
```

## 연산

스트림 메서드는 중간 연산(intermediate opreations)과 최종 연산(terminal operations)로 나뉜다

위의 특징에서 볼 수 있듯이 중간 연산은 스트림을 반환하고 최종 연산이 결과를 반환한다 

![이미지](stream.png)

위의 코드 동작은 filter → mapToInt → reduce 순이다

여기서 각 요소들이 메서드를 타고 가서 실행되는 것처럼 보이지만 실제로는 각 메서드가 모두 동작된 후에 다음 메서드를 실행한다

마지막 reduce 메서드를 보면 현재의 결과값과 이전 요소의 결과값을 더한 값을 반환하는 것을 볼 수 있다

중간 연산의 실행을 lazy하게 한다는 것은 각 요소마다 파이프라인을 거치는 것이 아닌, 스트림 내의모든 요소를 실행하여 스트림을 반환한 뒤 다음 메서드를 체이닝한다는 말이다

### 중간 연산

**map**

각 요소 (element)에 주어진 함수를 적용한 결과의 스트림을 반환한다

**filter**

주어진 Predicate에 따라 element를 선택한다 (필터링)

**sorted**

스트림을 정렬하는데 사용된다

### 최종 연산

**collect**

중간 연산을 모두 마치고 결과로 만들어 반환할 때 사용된다

**forEach**

스트림의 모든 요소를 순회 (iterate)할 때 사용된다

**reduce**

스트림의 요소들을 하나의 값으로 줄일 때 사용된다

## 예시

축구 선수 중 골을 넣은 선수의 이름을 출력하는 예시이다

**스트림 사용**

```java
Arrays.stream(soccerPlayers)
      .filter(soccerPlayer -> soccerPlayer.getGoal() > 0)
      .map(SoccerPlayer::getName)
      .forEach(System.out::println);
```

**바닐라 자바**

```java
for (int i = 0; i < soccerPlayers.length; i++) {
    if (soccerPlayers[i].getGoal() > 0) {
        System.out.println(soccerPlayers[i].getName());
    }
}
```

## 스트림이 빠를까 for-loop가 빠를까?

다음 기회에 … 

참고자료

[http://www.angelikalanger.com/Conferences/Videos/Conference-Video-GeeCon-2015-Performance-Model-of-Streams-in-Java-8-Angelika-Langer.html](http://www.angelikalanger.com/Conferences/Videos/Conference-Video-GeeCon-2015-Performance-Model-of-Streams-in-Java-8-Angelika-Langer.html)

[https://jaxlondon.com/wp-content/uploads/2015/10/The-Performance-Model-of-Streams-in-Java-8-Angelika-Langer-1.pdf](https://jaxlondon.com/wp-content/uploads/2015/10/The-Performance-Model-of-Streams-in-Java-8-Angelika-Langer-1.pdf)

[https://jypthemiracle.medium.com/java-stream-api는-왜-for-loop보다-느릴까-50dec4b9974b](https://jypthemiracle.medium.com/java-stream-api%EB%8A%94-%EC%99%9C-for-loop%EB%B3%B4%EB%8B%A4-%EB%8A%90%EB%A6%B4%EA%B9%8C-50dec4b9974b)

[http://people.eecs.berkeley.edu/~bh/ssch20/part6.html](http://people.eecs.berkeley.edu/~bh/ssch20/part6.html)

[https://blog.javarouka.me/2012/01/01/internal-external-iterator/](https://blog.javarouka.me/2012/01/01/internal-external-iterator/)

[https://www.geeksforgeeks.org/stream-in-java/](https://www.geeksforgeeks.org/stream-in-java/)
