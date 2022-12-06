## 1. 스택(Stack)
<span style="color: red">`스택(Stack)`</span> 은 데이터를 일시적으로 저장하기 위해 사용하는 자료구조다.
데이터의 입출력은 `후입선출(LIFO: Last In First Out)` 방식을 사용하고, **한 쪽 끝에서만 자료를 넣고 빼는 작업이 이루어진다.**

### 1.1. 스택의 구조
`스택(Stack)` 은 다음과 같은 구조로 나누어서 진행된다.
![Stack의 구조](https://velog.velcdn.com/images/tlsl13/post/c07911f0-615f-42c7-b06c-7c875c05aa7a/image.jpg)

`스택(Stack)` 은 top이 있는 위치에서만 데이터에 접근할 수 있다.
- **top()** — 스택의 맨 위에 있는 데이터 값 (현재 가리키고 있는 데이터)
- **pop()** — 데이터를 스택에서 빼내는 것
- **push()** — 데이터를 스택에 쌓는 것

### 1.2. 스택의 활용 예시
1. 작업 실행 취소와 같은 역추적 작업이 필요할 때
2. 괄호 검사, 후위 연산법, 문자열 역순 출력 등

### 1.3. 스택 코드
```javascript
class Stack {
  constructor() {
    this.arr = [];
    this.index = 0;
  }
  push(item) {
    this.arr[this.index++] = item;
  }
  pop() {
    if (this.index <= 0) return null;
    const result = this.arr[--this.index];
    return result;
  }
}

let stack = new Stack();
stack.push(1); 
stack.push(2); 
stack.push(3); 
console.log(stack.pop()); 
console.log(stack.pop()); 
console.log(stack.pop()); 
console.log(stack.pop()); 

// ----------- 출력결과 ------------
// 3
// 2
// 1
// null
```
<br>

## 2. 큐(Queue)
<span style="color: red">`큐(Queue)`</span> 는 스택과 마찬가지로 데이터를 일시적으로 저장하기 위해 사용하는 자료구조다. 
한 쪽에서만 데이터의 삽입, 삭제가 이루어졌던 stack과 달리 큐는 `선입선출(FIFO: First In First Out)` 방식으로 **양쪽 끝에서 데이터의 삽입과 삭제가 각각 이루어진다.**

### 2.1. 큐의 구조
`큐(Queue)` 는 다음과 같은 구조로 나누어서 진행된다.

![](https://velog.velcdn.com/images/tlsl13/post/904dd31f-29d9-45ed-913c-5a6ca9caab75/image.jpg)


- **Rear** - 데이터의 삽입 연산을 할 위치
- **Front** - 데이터의 삭제 연산을 할 위치
- **Enqueue** - 큐의 `rear` 에서 삽입 연산이 수행되는 기능
- **Dequeue** - 큐의 `front` 에서 삭제 연산이 수행되는 기능

### 2.2. 큐의 활용 예시
1. 은행 업무
2. 콜센터 고객 대기시간
3. 프로세스 관리

### 2.3. 큐의 코드
```javascript
class Queue {
  constructor() {
    this.arr = [];
    this.head = 0;
    this.tail = 0;
  }
  enqueue(item) {
    this.arr[this.tail++] = item;
  }
  dequeue() {
    if (this.head >= this.tail) {
      return null;
    } else {
      const result = this.arr[this.head++];
      return result;
    }
  }
}

let queue = new Queue();
queue.enqueue(1); 
queue.enqueue(2); 
queue.enqueue(3); 
console.log(queue.dequeue());
console.log(queue.dequeue()); 
console.log(queue.dequeue()); 
console.log(queue.dequeue());

// ----------- 출력결과 ------------
// 1
// 2
// 3
// null
```
<br>

## 3. 정리
`Queue`는 순서대로 처리해야 하는 작업을 임시로 저장해두는 버퍼(buffer)로서 많이 사용되고, `Stack`은 서로 관계가 있는 여러 작업을 연달아 수행하면서 이전의 작업 내용을 저장해 둘 필요가 있을 때 널리 사용된다.
- 스택과 대기열은 데이터 구조의 이전 구현을 기반으로 한다.
- 스택은 `	LIFO`이고 대기열은 `FIFO`이다.
- 둘 다 컴퓨터 과학 전반에 걸쳐 널리 사용된다.

<br>

