## 패킷 통신

- 데이터를 패킷이라고하는 작은 단위로 나누어 전송하는 방식

## IP(Internet Protocol)

- **IP**는 패킷 데이터들을 최대한 빠른 속도로 특정 목적지 주소로 보내는 프로토콜이다.
    - 빨리보내는 것이 해당 프로토콜의 목적
    - 패킷 전달 여부를 보증하지 않으며, **패킷의 순서 또한 보장하지 못한다.**

## TCP(Transmission Control Protocol)

- 패킷 통신은, 데이터를 작은 단위로 나누어 전송하기에 순서가 뒤섞이거나 내용이 유실될 가능성이 존재하며 이러한 문제점을 보완하기 위한 프로토콜
- TCP는 패킷을 정상적으로 받을 수 있도록(유실을 최소화)하는 프로토콜
    - 꼼꼼히 보내는 것이 목적이기에 IP보다는 **느린 패킷 전송속도**를 가진다.
    - 패킷 전달여부를 확실히 **보증**하며, 패킷을 송신 순서대로 받게해준다.
    - 목적지에 도착한 패킷들을 순서대로 정렬하고, 손상되거나 손실된 패킷이 존재한다면 출발지에 재요청하는 방식으로 진행

## TCP/IP

- TCP(전송 조절 프로토콜)와 IP(인터넷 프로토콜)의 묶음
- 사용자가 IP를 사용하여 패킷을 최대한 빨리 전송하면 TCP를 이용하여 패킷을 정상 수신한다.
- 해당 형태는 서로 다른 복수의 프로토콜을 조합한 형태이며, 이를 프로토콜 스택(Protocol Stack), 프로토콜 스위트(Protocol Suite)라고 부르며 , 서로 다른 프로토콜 스택끼리는 통신 할 수 없다.

## OSI 7계층 VS TCP/IP 4계층

![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FYOqHr%2FbtrEumwLV72%2Fv3utDz5FvFpvMy6wqZdk30%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FYOqHr%2FbtrEumwLV72%2Fv3utDz5FvFpvMy6wqZdk30%2Fimg.png)

- OSI 7계층의 경우 컴퓨터 간의 패킷 통신을 위해 거쳐야하는 7개의 계층을 의미한다.
- TCP/IP 4계층은, TCP/IP 프로토콜 통신 과정에 초점을 맞추어, OSI 7계층을 좀 더 단순화 시킨 계층을 의미한다.
- 해당 계층구조는 몇가지 특징이 존재한다.
    - 각 계층별 역할이 다르므로 계층별 간섭을 최소화할 수 있다.
    - 특정 계층에서 문제 발생시, 해당계층만 관리하면되기에 유지보수가 편리
    - 다른 계층끼리는 데이터의 전달과정을 구체적으로 알 필요가 없기에 데이터의 캡슐화 및 은닉이 가능

## TCP/IP 4계층의 캡슐화,역캡슐화

![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FV3PaG%2FbtrEtMCw2da%2FKi1cjiSeJkaHoQUNtrA2G0%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FV3PaG%2FbtrEtMCw2da%2FKi1cjiSeJkaHoQUNtrA2G0%2Fimg.png)

- TCP/IP 4계층은, 위 그림과 같이 애플리케이션 계층,전송 계층,인터넷 계층,네트워크 접근 계층으로 이루어져 있다.
- 데이터 전송 시, 데이터는 상위 계층에서 하위 계층으로 이동하고 계층 이동마다 **정보**(헤더)가 추가된다.
    - 이를 캡슐화라고 지칭한다.
- 계층 별로 추가되는헤더는 아래 사진과 같다.

![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FezkpmB%2FbtrEuQjONvp%2Fmc7GW3SNxmypplWwn0zqLK%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FezkpmB%2FbtrEuQjONvp%2Fmc7GW3SNxmypplWwn0zqLK%2Fimg.png)

### 4계층 : 애플리케이션 계층 (Application Layer)

- 사용자와 가장 가까운 계층으로, **사용자-소프트웨어** 간의 소통을 담당하는 계층이다.
- 애플리케이션을 실행하기 위한 데이터형식이 작성된다.
- 프로토콜으로는 **HTTP,HTTPS,FTP,SSH,Telnet,DNS,SMTP**가 존재한다.

### 3계층 : 전송 계층(Transport Layer)

- 통신 노드간 **신뢰성 있는 데이터** 전송을 보장하는 계층이다.
- 역캡슐화 과정에서, 포트번호를 사용해 데이터를 정확한 애플리케이션에 전달하는 역할도 수행한다.
    - 네트워크 액세스 계층과 인터넷 계층을 통해, 데이터가 목적지 기기까지 정상적으로 도착했다면 **전송계층은 포트번호를 사용해 데이터를 목적지 기기 내 적절한 에플리케이션으로 전달한다.**
- 프로토콜로는 **TCP,UDP,RTP,RTCP**가 존재한다.

### 2계층 : 인터넷 계층(Internet Layer)

- 패킷을 최종목적지까지 라우팅하는 계층
- 프로토콜로는 **IP,ARP,ICMP,RARP,OSPF**가 존재한다.

### 1계층 : 네트워크 액세스 계층(Network Access Layer or Network Interface Layer)

- 데이터를 전기신호로 변환한 후,**물리적 주소인 MAC 주소**를 사용해, 알맞은 기기로 데이터를 전달하는 계층
- 프로토콜로는 Ethernet,Wi-Fi,PPP,Token Ring과 같은 프로토콜이 사용

![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FDVYCW%2FbtrEtaEn6Nh%2FpSlH18jhzLOXZa1cJZKDjk%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FDVYCW%2FbtrEtaEn6Nh%2FpSlH18jhzLOXZa1cJZKDjk%2Fimg.png)

## TCP/IP 예제

1. 웹브라우저에 구글의 도메인을 입력한다.
    1. 이 때 구글 웹서버에 80번포트로 HTTP Request를 보낸다는 의미이다.
2. 구글 웹서버에 HTTP Request를 보내기 위해선 각계층에 필요한 정보들을 담은 패킷을 만들어야한다.

![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb2guU0%2FbtrEuyDK3ZU%2FKHXSRuiMz6FZdQCkCueO7k%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb2guU0%2FbtrEuyDK3ZU%2FKHXSRuiMz6FZdQCkCueO7k%2Fimg.png)

하지만 각계층에서 사용하는 프로토콜은 고정적이지 않으며, 예시일 뿐이다.

- 우선, Application Layer에는 Http Request 헤더가 들어간다.

```java
:authority: www.google.com
  :method: GET
  :path: /
  :scheme: https
  ...
  ...
```

- 그다음 Transport Layer에는 TCP 헤더가 들어간다.
    - TCP헤더에서는 **SP(출발지 포트번호)와 DP(목적지 포트번호)가 중요**하다.
    - 출발지 포트번호는 내 컴퓨터에서 만든 소켓의 포트 번호이므로, 내 컴퓨터는 알고 있으며, 목적지 포트 번호 또한 80으로 인식한다.

- 다음 순서로는 Internet Layer에서 IP헤더가 들어간다.
    - IP헤더에서의 중요개념은 **SA(출발지 IP주소)와 DA(목적지 IP주소)**이다.
    - 현재 DNS(Domain Name Server)밖에 알지 못하기에 목적지의 IP주소를 알지못하고 출발지 IP주소만 알고있게된다.
    - 따라서 IP주소를 얻어내기 위해 도메인 서버에 DNS 쿼리를 보낸다.

- 마지막으로 Network Access Layer에는 Ethernet 헤더가 들어간다.
    - Ethernet 헤더에서 중요개념은 **SA(출발지 MAC 주소)와 DA(목적지 MAC주소)**이다.
    - 목적지 MAC주소는 구글의 MAC 주소가 아닌 패킷이 전달될 라우터 또는 게이트웨이 MAC 주소를 의미

## 전송 계층의 데이터 전송방식

### 가상회선 패킷 교환방식

- 가상회선 패킷 교환 방식은 각 패킷에 가상회선 식별자(VCI)가 포함되며 모든 패킷을 전송하면 가상회선이 해제되고 패킷들은 전송된 **순서대로** 도착하는 방식을 말한다.
- 데이터를 연결하기전에 논리적 연결이 설정되며 이를 가상회선이라고 지칭한다.
- 데이터 그램은 패킷마다 라우터가 경로를 선택하지만,가상회선 방식은 경로를 설정할 때 한 번만 수행한다.

![https://woovictory.github.io/img/virtual_circut_packet.png](https://woovictory.github.io/img/virtual_circut_packet.png)

### 데이터 그램 패킷 교환방식

데이터를 전송하기 전에 논리적 연결이 설정되지 않으며 패킷이 독립적으로 전송된다.

이를 `데이터그램` 이라고 한다.

패킷을 수신한 라우터는 최적의 경로를 선택하여 패킷을 전송하는데 하나의 메시지에서 분할된 여러 패킷은 서로 다른 경로로 전송될 수 있다.(**비연결 지향형**)

**송신 측에서 전송한 순서와 수신 측에 도착한 순서가 다를 수 있다.**

![https://woovictory.github.io/img/datagram_packet.png](https://woovictory.github.io/img/datagram_packet.png)

### TCP 연결 성립과정

- TCP는 신뢰성을 확보할 때 `3-way-handshake` 작업을 실행한다.

### 3-Way-HandShake

![Untitled](https://github.com/seonghoo1217/TodayILearnd/raw/main/TIL-img/HandShacking.PNG)

1. Synchronization: 임의의 시퀀스 번호를 서버에 보낸다.
2. SYN+ACK : 클라이언트의 SYN을 수신한후 시퀀스번호 +1 을 다시 전달한다.
3. ACK : 이를 다시한번 서버에 +1 값으로 전달

### TCP 연결해제 과정

- TCP를 연결해제 할때는 4-웨이 핸드셰이크를 사용한다.

![Untitled](https://t1.daumcdn.net/cfile/tistory/99229C485C1D90C038)

### 4-웨이-핸드셰이크

1. 클라이언트가 연결을 닫기위해 FIN으로 설정된 세그먼트를 보내며 WAIT 상태가된다.
2. 서버는 클라이언트로 ACK 승인세그먼트를 보낸후 CLOSE_WAIT 상태에 접어들게 되고 클라이언트가 세그먼트를 받고 다시한번 WAIT 상태가 된다.
3. 서버는 ACK를 보낸후 일정 시간 후 클라이언트에게 FIN이라는 세그먼트를 보낸다.
4. 클라이언트는 TIME_WAIT 상태후 서버로 ACK를 보내 CLOSE 상태를 확인한후 임의의 시간대기후 연결이 완전히 해제된다.

### TIME_WAIT

일정시간 후 연결이 닫히는 이유는 무엇일까?

총 두가지 이유가 존재한다.

첫번째로는 지연패킷이 발생하는 것을 대비하기 위함이다. 정확히는 발생을 대처하는 것이 아닌 패킷이 뒤늦게 도달할 경우 데이터 무결성 문제가 발생하기에 잠시대기한다.

두번째로는 두 장치가 연결이 닫혔는지 확인하기 위함이다. 3번상태에서 닫히게 될경우 장치의 상태가 3번으로 유지되기에 계속 접속오류가 나게된다.

### 이더넷 프레임

데이터 링크 계층은 이더넷 프레임을 통해 전달받은 데이터의 에러를 검출하고 캡슐화한다.

![https://t1.daumcdn.net/cfile/tistory/1258B1484F90FC671C](https://t1.daumcdn.net/cfile/tistory/1258B1484F90FC671C)

- Praeamble : 이더넷 프레임이 시작임을 알린다.
- SFD(Start Frame Delimiter) : 다음 바이트부터 MAC주소 필드가 시작됨을 알림
- DMAC,SMAC : 수신,송신 MAC 주소를 뜻함
- EtherType: 데이터 계층위의 계층인 IP프로토콜을 정의한다. IPv4 또는 IPv6를 의미한다.
- Payload: 전달받은 데이터
- CRC: 에러 확인 비트

## 계층 간 데이터 송수신 과정

필자가 컴퓨터를 통해 다른 컴퓨터로 데이터를 요청한다면 우리 눈에는 그냥 데이터가 오는걸로 보이지만 실질적으로는 TCP/IP 4계층을 거치며 여러 요청값들이 **캡슐화되고 다시 비캡슐화**되는 것을 반복한다.

### 캡슐화 과정

캡슐화 과정은 상위 계층의 헤더와 데이터를 하위 계층의 데이터 부분에 포함시키고 해당 계층의 헤더를 삽입하는 과정

- **Application Layer ⇒ Transport Layer**
    - 세그먼트 또는 데이터그램화 되며 TCP(L4)헤더가 붙는다..
- **Transport Layer ⇒ Internet Layer**
    - IP(L3) 헤더가 붙여지며 **패킷화가** 진행됨
- **Internet Layer ⇒  Network Access Layer**
    - 프레임 헤더와 프레임 트레일러가 붙어 **프레임화**가 진행

### 비캡슐화 과정

비캡슐화 과정은 하위계층에서 상위 계층으로 가며 각 계층의 헤더를 제거하는 과정

캡슐화의 역순의 과정을 거친뒤 최종적으로 사용자에게 애플리케이션의 PDU인 메시지로 전달됨

### PDU(Protocol Data Unit)

네트워크의 어떠한 계층에서 타계층으로 데이터 전송시의 단위를 PDU라고 지칭한다.

PDU는 크게 헤더와 페이로드로 구성되어있으며 페이로드는 계층에따라 다르게 불린다.

- 애플리케이션 계층 : 메시지
- 전송 계층 : 세그먼트(TCP),데이터그램(UDP)
- 인터넷 계층 : 패킷
- 링크 계층 : 프레임(데이터링크 계층), 비트(물리계층)

- **헤더:** 제어 관련 정보
- **페이로드:** 데이터

### 네트워크 기기

네트워크 기기는 계층별로 처리 범위를 나눌 수 있다.

물리 계층을 처리할 수 있는 기기와 데이터 링크 계층을 처리할 수 있는 기기등이 존재하며 상위계층에서 처리가능한 기기는 하위계층을 처리할 수 있지만 역순은 불가능하다.

- 애플리케이션 계층 : L7 스위치
- 인터넷 계층: 라우터,L3 스위치
- 데이터 링크 계층 : L2 스위치, 브리지
- 물리계층 : NIC,리피터,AP

## L7 스위치

스위치는 여러 장비를 연결하고 데이터 통신을 중재하며 목적지가 연결된 포트로만 전기신호를 보내 데이터를 전송하는 통신 네트워크 장비

- L7 스위치는 로드밸런서라고도 하며 , 서버의 부하를 분산하는 기기
- 클라이언트로부터 오는 요청들을 뒤쪽의 여러 서버로 나누는 역할을 수행하며 시스템이 처리할 수 있는 트래픽 증가를 목표로 삼는다.
    - URL,서버,캐시,쿠키들을 기반으로 트래픽을 분산
    - 바이러스,불필요한 외부 데이터 등을 걸러내는 필터링 기능 또한 수행하며 응용프로그램 수준의 트래픽 모니터링이 가능하다.
- 장애가 발생한 서버가 존재할 시 트래픽 분산 대상에서 제외해야하기 위해 주기적 헬스체크를 이용

### L7 스위치와 L4 스위치의 차이

L4 스위치 또한 로드밸런서로 사용이 가능하다. 그렇다면 어떤 차이점이 존재할까?

L4 스위치는 우선적으로 사용계층에서 차이가 존재하며 전송계층에서 작동한다. 또한 스트리밍 관련 서비스에서는 사용이 불가하며, 메시지를 기반으로 인식하지 못하며 IP와 포트를 기반으로 트래픽을 분산한다.

반면 L7스위치는 IP와 포트이외에도 URL,Http 헤더 ,쿠키등을 기반으로 트래픽을 분산한다.

참고로 클라우드 서비스에서 L7 스위치를 이용한 로드밸런싱은 ALB컴포넌트로 하며 L4 스위치를 이용한 로드밸런싱은 NLB를 이용한다.

### 헬스 체크

- 전송 주기와 재전송 횟수등을 설정한 후 반복적으로 서버에 요청을 보내는 것
- 서버의 정상여부를 판별하는 기준
- 헬스체크의 요청 수는 서버의 부하를 일으킬 정도가 아니며, 다양한방법으로 요청을한다.
- 만약 TCP 요청시 핸드셰이크가 일어나지 않는다면 정상이 아닌것

### 로드밸런서를 이용한 서버 이중화

서버 이중화는 서버의 에러가 일어날 경우를 가정하여 안정적인 운용을 위해 구축한다.

로드밸런서는 2대이상의 서버를 가상의 IP를 제공하여 이를 기반으로 안정적인 서비스를 제공

## Internet Layer의 기기

대표적으로 라우터와 L3 스위치가 존재

### 라우터

- 여러개의 네트워크를 연결,분할,구분시켜주는 역할을 하며 다른네트워크에 존재하는 장치끼리 데이터를 주고 받을 때 패킷소모를 최소화하고 경로를 최적화하여 최소경로로 패킷을 포워딩하는 장비

### L3 스위치

L2 스위치의 기능과 라우팅기능을 갖춘 장비이다. L3스위치는 라우터와 동일시 된다.

하드웨어 기반의 라우팅을 담당하는 장치

## Data Link Layer 기기

### L2 스위치

- 장치들의 MAC 주소를 MAC 주소 테이블을 통해 관리하여 연결된 장치로 부터 패킷이 왔을 때 패킷전송을 담당
- IP 주소를 이해하지못해 IP주소 기반의 라우팅은 불가능하며 단순히 패킷의 MAC 주소를 읽어 스위칭 하는 역할
- 목적지가 MAC주소 테이블에 없다면 전체포트에 전달하고 일정시간 이후 삭제

### 브리지

- 근거리 통신망을 상호 접속할 수 있도록 하는 통신망 연결 장치
- 포트와 포트사이의 다리역할을 하며 장치에서 받아온 MAC 주소를 MAC 주소테이블로 관리
- 통신망 범위 확장 및 통신망 구축에 사용

## 물리 계층 기기

### NIC

LAN 카드라고 하며 네트워크 구성과 빠른 속도의 송수신에 사용

고유의 식별번호인 MAC 주소가 존재

### 리피터

약해진 신호를 증폭하여 전달하는 역할

### AP

패킷을 복사하는 기능