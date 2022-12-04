# DFS(깊이 우선 탐색)

![Depth-first-tree](https://user-images.githubusercontent.com/77488652/205468569-baf8378c-15fe-4241-9861-b3adce30c57c.png)

먼저 `DFS` 는 그래프 탐색의 한 종류이며 깊이 우선 탐색이라고 부른다.

루트 노드 혹은 다른 임의의 노드에서 시작해 다음 분기로 넘어가기 전에 해당 분기를 완벽하게 탐색하고 넘어가는 방법이다. 넓게 탐색하기 전에 깊게 탐색한다.
모든 노드를 방문하고자 하는 경우 이 방법을 사용한다.

**결론**

> DFS란 특정 노드에서 시작해 다음 분기로 넘어가기 전에 해당 분기를 완벽하게 탐색하는 방법이다.

## 장점

1. 현 경로상의 노드들만 기억하면 되기에 저장공간 수요가 비교적 적다.
2. 목표 노드가 깊은 단계에 있을 경우 해를 빨리 구할 수 있다.

## 단점

1. 해가 없는 경로가 깊을 경우 탐색시간이 오래 걸릴 수 있다.
2. 얻어진 해가 최단 경로가 된다는 보장이 없다.

## DFS의 시간 복잡도

- DFS는 그래프(정점의 수: N, 간선의 수: E)의 모든 간선을 조회한다.
  - 인접 리스트로 표현된 그래프: O(N+E)
  - 인접 행렬로 표현된 그래프: O(N^2)

## 자바스크립트 코드 구현

<img width="250" alt="Screen Shot 2022-12-04 at 10 41 54 AM" src="https://user-images.githubusercontent.com/77488652/205469911-291024ad-a79e-4495-862f-393c35767911.png">

이 트리를 DFS 코드로 구현 해보자 (재귀)

### 코드

![code](https://user-images.githubusercontent.com/77488652/205469917-a9e10cf6-b98b-4aeb-a6c8-a4b23da66523.png)

### 결과

<img width="119" alt="Screen Shot 2022-12-04 at 10 44 09 AM" src="https://user-images.githubusercontent.com/77488652/205469946-f2210062-dcfe-4c8a-bcc4-5af61557b0eb.png">

### 탐색 과정

이해하기 쉽게 그래프의 진행 과정을 자세히 봐보자.
**코드를 봐가면서 그림을 보면 이해하기 쉬울 것이다.**

#### 1번

<img width="594" alt="1" src="https://user-images.githubusercontent.com/77488652/205473239-619410c9-f631-459b-99f8-e7a81a5d1609.png">

트리의 루트 노드인 1를 기준으로 탐색을 시작한다. (DFS(1))

코드의 5번 라인에서 1를 answer에 대입한다.

#### 2번

<img width="550" alt="2" src="https://user-images.githubusercontent.com/77488652/205473242-3571b8a9-da6b-427c-8935-61f3c445d644.png">

DFS(1)의 6번 라인에서 `DFS(2)`를 호출한다.

DFS(2)의 5번 라인에서 2를 answer에 대입한다.

#### 3번

<img width="620" alt="3" src="https://user-images.githubusercontent.com/77488652/205473245-c48eb3fa-879b-48ee-9fcd-5520fe5c901a.png">

DFS(2)의 6번 라인에서 `DFS(4)`를 호출한다.

DFS(4)의 5번 라인에서 4를 answer에 대입한다.

#### 4번

<img width="680" alt="4" src="https://user-images.githubusercontent.com/77488652/205473606-af049487-8f9c-496a-98b6-02081ae0b18a.png">

DFS(4)의 6번 라인에서 `DFS(8)`를 호출한다.

DFS(8)의 4번 라인에서 조건이 안맞기 때문에 트리를 끊어 `DFS(4)`의 6번 라인으로 다시 돌아간다.

#### 5번

<img width="657" alt="5" src="https://user-images.githubusercontent.com/77488652/205473728-f32e9deb-b645-4059-8c9b-f359ef0b4918.png">

DFS(4)의 7번 라인에서 `DFS(9)`를 호출한다.

DFS(9)의 4번 라인에서 조건이 안맞기 때문에 트리를 끊어 `DFS(4)`의 7번 라인으로 다시 돌아간다.

#### 6번

<img width="620" alt="6" src="https://user-images.githubusercontent.com/77488652/205473611-4521669c-6bd9-47d8-9eae-928b4f591cc1.png">

DFS(4)의 함수는 7번 라인이 끝이므로 트리를 끊어 `DFS(2)`의 6번 라인으로 다시 돌아간다.

#### 7번

<img width="611" alt="7" src="https://user-images.githubusercontent.com/77488652/205473612-fb10f890-d361-4d57-81ed-997d80c979ba.png">

DFS(2)의 7번 라인에서 `DFS(5)`를 호출한다.

DFS(5)의 5번 라인에서 5를 answer에 대입한다.

#### 8번

<img width="631" alt="8" src="https://user-images.githubusercontent.com/77488652/205473615-a23092d5-47a9-45fe-a4ae-c11f37507cce.png">

DFS(5)의 6번 라인에서 `DFS(10)`를 호출한다.

DFS(10)의 4번 라인에서 조건이 안맞기 때문에 트리를 끊어 `DFS(5)`의 6번 라인으로 다시 돌아간다.

#### 9번

<img width="621" alt="9" src="https://user-images.githubusercontent.com/77488652/205473968-533ca214-7430-4b17-bcae-1adce09a5971.png">

DFS(5)의 7번 라인에서 `DFS(11)`를 호출한다.

DFS(11)의 4번 라인에서 조건이 안맞기 때문에 트리를 끊어 `DFS(5)`의 7번 라인으로 다시 돌아간다.

#### 10번

<img width="600" alt="10" src="https://user-images.githubusercontent.com/77488652/205473970-cc93ae5e-e38f-4d2b-9885-48b8b85b401e.png">

DFS(5)의 함수는 7번 라인이 끝이므로 트리를 끊어 `DFS(2)`의 7번 라인으로 다시 돌아간다.

DFS(2)의 함수는 7번 라인이 끝이므로 트리를 끊어 `DFS(1)`의 6번 라인으로 다시 돌아간다.

#### 11번

<img width="580" alt="11" src="https://user-images.githubusercontent.com/77488652/205473972-0f9cb444-c665-4a2c-91a9-aa104c347ee6.png">

DFS(1)의 7번 라인에서 `DFS(3)`를 호출한다.

DFS(3)의 5번 라인에서 3를 answer에 대입한다.

#### 12번

<img width="589" alt="12" src="https://user-images.githubusercontent.com/77488652/205473973-06d1855e-a1d9-481e-a06d-27c5ebbfcfc4.png">

DFS(3)의 6번 라인에서 `DFS(6)`를 호출한다.

DFS(6)의 5번 라인에서 6를 answer에 대입한다.

#### 13번

<img width="607" alt="13" src="https://user-images.githubusercontent.com/77488652/205473974-e85b428b-b37f-4c1e-997c-8010bd8cca94.png">

DFS(6)의 6번 라인에서 `DFS(12)`를 호출한다.

DFS(12)의 4번 라인에서 조건이 안맞기 때문에 트리를 끊어 `DFS(6)`의 6번 라인으로 다시 돌아간다.

#### 14번

<img width="600" alt="14" src="https://user-images.githubusercontent.com/77488652/205474247-97d6a598-c9f8-4e18-81b7-17efd70e035c.png">

DFS(6)의 7번 라인에서 `DFS(13)`를 호출한다.

DFS(13)의 4번 라인에서 조건이 안맞기 때문에 트리를 끊어 `DFS(6)`의 7번 라인으로 다시 돌아간다.

#### 15번

<img width="593" alt="15" src="https://user-images.githubusercontent.com/77488652/205474248-f38c28ea-a18c-4c49-a6f0-94964f9080f3.png">

DFS(6)의 함수는 7번 라인이 끝이므로 트리를 끊어 `DFS(3)`의 6번 라인으로 다시 돌아간다.

#### 16번

<img width="613" alt="16" src="https://user-images.githubusercontent.com/77488652/205474249-ee548739-1229-4b6c-9f80-9d8457277e83.png">

DFS(3)의 7번 라인에서 `DFS(7)`를 호출한다.

DFS(7)의 5번 라인에서 7를 answer에 대입한다.

#### 17번

<img width="615" alt="17" src="https://user-images.githubusercontent.com/77488652/205474256-07d909bc-050a-4d29-b05d-fdf4bc8d141a.png">

DFS(7)의 6번 라인에서 `DFS(14)`를 호출한다.

DFS(14)의 4번 라인에서 조건이 안맞기 때문에 트리를 끊어 `DFS(6)`의 6번 라인으로 다시 돌아간다.

#### 18번

<img width="608" alt="18" src="https://user-images.githubusercontent.com/77488652/205474258-d9b7ea02-76af-4d09-9dff-d016ccef9f23.png">

DFS(6)의 7번 라인에서 `DFS(15)`를 호출한다.

DFS(15)의 4번 라인에서 조건이 안맞기 때문에 트리를 끊어 `DFS(6)`의 7번 라인으로 다시 돌아간다.

#### 19번

<img width="599" alt="19" src="https://user-images.githubusercontent.com/77488652/205474260-c64ec9f2-fb0a-4dd0-8549-933018cab744.png">

DFS(1), DFS(3), DFS(7) 전부 다 7번째가 끝 라인이므로 순서대로 트리를 끊는다.

#### 정리

DFS(1)이 먼저 호출되고 내부 함수를 실행하다가 DFS(2)를 실행하면 해당 코드의 위치와 DFS(1)을 함수의 스택에 저장하고 다음 스택에 DFS(2)를 넣고 다시 함수를 실행한다. 이렇게 계속 스택이 쌓이게 되고 내부 if 문에 의해서 함수가 return되어 종료되면 다시 스택의 최상위에 있던 함수가 pop() 되어 사라지고 바로 하단의 함수의 실행 코드부터 해당 함수가 실행된다. 그리고 DFS(n\*2 + 1)가 실행되어 또 스택을 다시 쌓고 함수의 스택이 전부 사라질 때까지 반복하여 실행된다.

이런 실행 순서를 이해하면 전위 순회, 중위 순회, 후위 순회를 실행해 볼 수 있다.

위의 예제 코드는 전위 순회이다.
