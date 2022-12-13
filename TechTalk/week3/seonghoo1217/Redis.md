## Redis

> 빠른 오픈 소스 인 메모리 키 값 데이터 구조 스토어

보통 데이터베이스는 하드 디스크나 SSD에 저장한다. 하지만 Redis는 메모리(RAM)에 저장해서 디스크 스캐닝이 필요없어 매우 빠른 장점이 존재함

캐싱도 가능해 실시간 채팅에 적합하며 세션 공유를 위해 세션 클러스터링에도 활용된다.


***RAM은 휘발성 아닌가요? 껐다키면 다 날아가는데..***

이를 막기위한 백업 과정이 존재한다.

- snapshot : 특정 지점을 설정하고 디스크에 백업
- AOF(Append Only File) : 명령(쿼리)들을 저장해두고, 서버가 셧다운되면 재실행해서 다시 만들어 놓는 것

데이터 구조는 key/value 값으로 이루어져 있다. (따라서 Redis는 비정형 데이터를 저장하는 비관계형 데이터베이스 관리 시스템이다)

##### value 5가지

1. String (text, binary data) - 512MB까지 저장이 가능함
2. set (String 집합)
3. sorted set (set을 정렬해둔 상태)
4. Hash
5. List (양방향 연결리스트도 가능)

### 인메모리 DB의 필요성

인메모리 DB는 대부분 속도측면에서의 성능개선을 위해 사용된다. 인메모리 DB는 메모리 RAM에 데이터를 올려서 사용하는 방식이다.
왜 메모리에 데이터를 올릴까? 당연히 **속도** 때문이다. SSD,HDD 같은 저장공간에서 데이터를 가져오는 것보다 RAM에 올려진 데이터를 가져오는데 걸리는 속도가 수백배(HDD 기준) 이상 빠르다

![Ram](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FBcwuU%2FbtqWKYSRi5Q%2FjRblmXQlGtIdD4nURFkHWk%2Fimg.png)

대신 용량이 작다는 **단점**이 존재한다. 그렇기 때문에 메인 데이터베이스로 사용하기엔 무리가 있다.
어? 그럼 RAM의 용량이 큰걸 사서 메인 데이터베이스로 쓰면안되나? **안된다.**
비용이 상당히 큰 걸림돌이 되기 때문이다.

![HDD](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcJNBig%2FbtqWzsOg5he%2FX3tHPGyI7obqDSlc9LDokk%2Fimg.png)

### Redis 사용적합 예시

대용량의 데이터 중 소규모의 데이터만을 추출하고자 할때, 또는 값의 변경이 잘일어나지 않고 소규모의 데이터일 때 사용에 적합하다.

예를 들어 사용자가 백만명인 게임의 상위랭커 100위를 보여주는 기능에 적합하다.

## 캐시

캐시란 자주사용하는 데이터나 값을 미리 복사해 놓는 임시 저장소를 말한다. 그렇기 때문에 캐시에 존재하는 데이터에 한해서는 최소한의 비용으로 반복적 접근을 할 수 있다.

이런 캐시는 두가지 종류로 나뉜다.

### Local Cache VS Global Cache

- Local Cache
  - Local 환경에서만 작동
  - Local 장비의 Resource를 활용한다. (Memory,Disk)
  - Local에서만 작동 되기 때문에 속도가 빠르다.
  - 타 서버와 데이터 공유가 힘들다.

- Global Cache
  - 데이터의 분산저장이 가능
  - 여러 서버에서 접근이 가능
  - Local Cache에 비해 느리다.
  - 별도의 Cache Server를 이용하기 때문에 서버 간 데이터 공유가 쉽다.

Redis는 Global Cache에 적합한다.


### Redis의 동시성 이슈

Redis는 싱글 스레드 기반으로 데이터를 처리한다. 그런데 동시성 이슈라는 어찌보면 황당한 말이다. 하지만 우리가 분명 Redis를 사용함에 있어
여러명들의 클라이언트들에게 동시에 응답을 해주는 것 또한 알고 있다.

이는 Redis의 동작원리가 실제 명령에 대한 작업은 커널 레벨에서 멀티 플렉싱(Multiplexing)을 이용해 동시성을 보장하기 때문이다.
즉 유저레벨에서는 싱글 스레드로 작동하나, 커널 I/O레벨에서는 스레드 풀을 이용한다.

동시성이 존재한다는 것은 언제든 동시성으로 인한 에러가 발생할 수 있다는 것이고 그렇기에 동시성에 대한 처리가 필요하다.

### Redis의 동시성 처리를 위한 Transaction

Spring Data Redis를 기준으로 Transaction 사용 방법이 두가지가 존재한다.
한 가지는 `SessionCallback 인터페이스`를 통해 여러 명령을 하나로 묶어서 처리하는 방법인데, 
이 방법은 인터페이스를 통해 직접적으로 Redis 명령어를 사용하여 트랜잭션 경계를 설정하는 방법이고,
다른 한 가지는 간편하게 `@Transactional` 어노테이션을 사용하는 방법이다.
