# Data Structure

## 배열(Array)과 연결 리스트(Linked List)

### **배열(Array)이란?**

---

- 배열은 메모리 상에 데이터(원소)를 연속하게 배치한 구조

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cef032c2-25d6-4d10-a8b8-b35f531a4a60/Untitled.png)

- 배열은 같은 타입의 데이터를 여러개 나열한 **선형 자료구조**
- 연속적인 메모리 공간에 **순차적**으로 데이터를 저장
- 배열은 선언할 때 크기를 정하면, 그 크기로 **고정**이 된다.
- 선언된 값은 다시 배열을 선언하지 않으면 **변경할 수 없다.**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e6a0beae-df75-42a3-91f0-fbbc595b2b33/Untitled.png)

- 배열의 주소를 살펴보면, 한 칸마다 배열의 자료형의 크기를 가지고 있다.
- 배열의 자료형이 int라면, 배열 한 칸의 크기는 int(4byte)가 되는 것이다.
- 각 요소는 인덱스를 통해 엑세스 할 수 있다.

**배열의 시간 복잡도(Time complexity)**

---

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2da50df5-9f98-4413-90ef-7cacc755433a/Untitled.png)

**특징**

---

- 동일한 데이터 유형을 가진다.
    - 주로 동일한 데이터 유형을 가지지만 이질형 데이터도 지원 가능한 프로그래밍 언어도 있음
    - 이질형 데이터들이 모인 집합체는 주로 레코드라 한다.
- 배열의 각 요소에 접근하는 시간은 O(1)로 모두 동일하다.
    - 기본위치 + 오프셋(요소크기 * 인덱스) 연산으로 모든 요소에 접근 가능
- 연속된 메모리에 단일 블록화하여 데이터를 저장한다.
    - 낭비되는 공간이 거의 없음
    - 다만, 큰 배열일 경우, 필요 메모리 할당이 불가능할 수도 있음
- 실제 메모리상에서 물리적으로 데이터가 순차적으로 저장되기 때문에 데이터에 순서가 있으며, index가 존재하여 indexing 및 slicing이 가능하다.
    - indexing : index를 사용해 특정 요소를 리스트로부터 읽어들이는 것
    - slicing : 요소에 특정 부분을 따로 분리해 조작하는 것

**장점**

---

- 구현이 쉽다.
- 참조를 위한 추가적인 메모리가 필요하지 않는다.
- 연속적이므로 메모리 관리가 편하다.
- 인덱스를 통해 접근하므로 검색이 빠르다.

**단점**

---

- 배열의 크기를 변경할 수 없다.
    - 크기를 변경하려면 새로운 배열을 만든 후, 기존의 데이터를 옮겨야 한다.
- 메모리 낭비가 발생하게 된다.
    - 배열을 선언한다 하더라도 사용하지 않는 공간은 배열을 선언할 때, 공간이 할당되어 있어서 낭비가 된다.


**배열을 사용해야하는 상황**

---

- 데이터 개수가 확실하게 정해져 있을 때 데이터 저장을 위한 자료구조로 선택하면 좋음
- 삽입 / 삭제 작업이 적을 때 사용하면 좋음
- 배열에 저장된 데이터를 검색하는 작업이 많을 때(인덱스로 빠르게 검색 가능)

### **연결** **리스트(Linked List)란?**

---

- 데이터와 포인트로 구성된 노드 간의 `연결(link)`을 이용해서 리스트를 구현한 자료구조
- 배열의 고정크기의 단점을 보완하기 위해 만들어졌으며 배열과 달리 연속적인 메모리 공간에 저장되어 있지 않기 때문에 연결이 필요함

**연결리스트 표현**

---

- 아래 그림과 같이 포인터를 사용하여 각 노드를 연결한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d4bbdcc1-f1a2-457e-a416-4188131c9fbb/Untitled.png)

- Head는 리스트의 처음을 나타낸다.
- 노드는 데이터와 다음 노드를 가리키는 Next 포인터로 구성되어 있다.
- 각 노드는 Next 포인터를 사용하여 다음 노드와 연결된다.
- 마지막 노드는 null을 가리켜 리스트의 끝을 나타낸다.

**연결 리스트의** **시간 복잡도(Time complexity)**

---

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b4a0a162-ef1d-48c4-973b-914a67a65424/Untitled.png)

**특징**

---

- 노드의 next 부분에 다음 노드의 위치를 저장함으로써 선형적 데이터 자료구조를 가진다.
- 연결되어있는 구조이기 때문에 특정 위치의 데이터를 탐색하기 위해서는 첫 노드부터 탐색을 시작하여 순차 접근만 가능하다.
- 주소만 연결하면 되기 때문에 삽입 삭제가 배열에 비해 빠르고 쉽다.
- 불연속적 단위로 저장되기 때문에 조회에 불리하며 포인터로 인해 추가 저장공간이 필요하다.

**장점**

---

- 크기가 가변적이다.
    - 메모리가 허용하는 만큼 커질 수 있음
- 삽입 삭제가 쉬움
    - 삽입 삭제 시에 데이터 이동이 필요 없음

**단점**

---

- 랜덤 엑세스가 불가능함, 요소에 접근하려면 첫 번째 노드부터 순차적으로 접근해야함
- 포인터를 위한 추가 메모리 공간이 필요하다.

**이중 연결 리스트 (Doubly Linked List)**

---

- 이중 연결 리스트는 위에서 언급한 연결 리스트와 다르게 전/후로 탐색이 가능한 구조이다.
- 즉, 단순 연결 리스트의 노드는 데이터와 다음 노드의 주소를 저장한다면, 이중 연결 리스트의 노드는 데이터, 이전 노드의 주소와 다음 노드의 주소를 저장하게 된다.
- 장점 : 단순 연결 리스트에서는 최악의 경우 n번의 탐색을 해야하지만, 이중 연결 리스트에서는 얻고자 하는 데이터의 위치가 tail에 가깝다면 tail에서부터 역방향으로 탐색이 가능하기 때문에 탐색 시간을 줄일 수 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/001c8903-bcdf-4fa3-8b81-e629a136297f/Untitled.png)

**원형 연결 리스트 (Circular Linked List)**

---

- 원형 연결 리스트는 단순 연결 리스트의 마지막 노드가 null을 가리키는 게 아닌, 처음 노드를 가리키는 구조이다.
- 즉, head에서부터 순회를 반복적으로 진행하다보면 다시 처음으로 돌아오는 구조이다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d560ca96-1bc9-47c9-a7a1-079068da0d2f/Untitled.png)

- 이중 연결 리스트도 마지막 노드가 처음 노드를 가리키는 구조가 되면, 이를 이중 원형 연결 리스트라고 한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e3df5645-a5e1-4c5e-af9d-ca47ddaad168/Untitled.png)

참고 사이트

---

[https://velog.io/@xxhaileypark/자료-구조-배열-연결-리스트-Array-LinkedList](https://velog.io/@xxhaileypark/%EC%9E%90%EB%A3%8C-%EA%B5%AC%EC%A1%B0-%EB%B0%B0%EC%97%B4-%EC%97%B0%EA%B2%B0-%EB%A6%AC%EC%8A%A4%ED%8A%B8-Array-LinkedList)

[https://yoongrammer.tistory.com/44](https://yoongrammer.tistory.com/44)

[https://velog.io/@hanif/자료구조-배열](https://velog.io/@hanif/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EB%B0%B0%EC%97%B4)

[https://chunggaeguri.tistory.com/entry/자료구조-배열Array이란](https://chunggaeguri.tistory.com/entry/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EB%B0%B0%EC%97%B4Array%EC%9D%B4%EB%9E%80)