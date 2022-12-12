# HTTP 와 HTTPS

오늘날, 우리는 모두 대부분의 정보를 인터넷으로 확인한다. 모든 웹 브라우저에 있는 정보에 접근할 때는 URL을 통하여 접근한다. 반대로 생각하면 URL을 모르는 정보에는 접근을 할 수 없다라는 것을 뜻한다.

위 설명이 현재 우리가 사용하는 **WWW(World Wide Web)** 의 기본적인 틀이다.

구글을 예시로 보면,

[https://www.google.com/?hl=ko](https://www.google.com/?hl=ko) → 이 URL은 구글의 URL이다.

URL에서 www.google.com는 구글이라는 곳임을 인지하지만, 그 앞에 붙은 https(http+s)는 무엇을 뜻하는 것일까? 그리고, URL을 입력한 후 나타나는 웹페이지에 있는 모든 정보들은 다 어디서 온 것일까?

누군가는 어떤 정보를 생성하거나 가공했을 것이고, 누군가는 그 정보를 보고 싶을 것이다.

예를 들어 나 자신이 URL을 통해 누군가에게 해당 정보를 **요청하면,** 요청한 정보를 누군가가 나에게 다시 **전달해준다.**

이러한 규칙을 HTTP라고 부른다.

## **HTTP**

HTTP는 HYPERTEXT TRANSFER PROTOCOL의 약자이다.

- Hypertext: 컴퓨터 화면이나 전자 기기에서 볼 수 있는 데이터이며, 다른 데이터와 연결될 수 있는 주소를 참조하고 있다.
- Transfer: 사람들이 브라우저를 통해 확인하는 웹 상의 데이터는 HTTP에 의해 전달된다.
- Protocol: 규칙 혹은 규약을 뜻한다.

즉, 웹 상에서 클라이언트와 서버가 서로 정보를 주고받을 수 있도록 하는 규약이다.

- 클라이언트 : 서버에 정보(데이터) 전송을 요청(Request)할 수 있는 클라이언트 소프트웨어(크롬, IE, 사파리 등 웹 브라우저가 대표적이죠)가 설치된 컴퓨터(스마트폰 등을 포괄하는, 연산하는 기계의 개념)를 의미한다.
- 서버 : 응답하는(Response) 소프트웨어(아파치, nginx, IIS 등이 유명하죠)가 설치된 컴퓨터를 의미한다. 서버는 클라이언트의 요청을 해석하고 클라이언트의 요청 및 서버 관리자가 설정한 알고리즘에 준하는 정보를 클라이언트에게 송신한다.
<p align="center"><img width="500" alt="2" src="https://user-images.githubusercontent.com/65716445/204492132-4c57de22-9417-4262-b99c-0b4537656afa.png"></p>

1. 클라이언트가 보고 싶은 정보를 서버에게 HTTP를 통해 요청 → HTTP Request

2. 서버는 알맞은 응답 메시지 및 정보를 클라이언트에게 전달.

3. 응답 메시지 및 정보 중 HTTP body 내용이 클라이언트가 설정한 클라이언트의 용처에 도달한다. → HTTP Response

### **HTTP Message**

<p align="center"><img width="800" alt="2" src="https://user-images.githubusercontent.com/65716445/204492140-7e16b119-0072-4945-882b-4f52e27421af.png"></p>

- 서버 주소(Host), 요청 메서드(Post), 상태 코드(403,404), target path, 헤더 정보, 바디 정보 등이 포함된다.
- 요청 메시지, 응답 메시지의 모양이 다르다.
- HTTP/1.1 메시지는 사람이 읽을 수 있다. → 통신 상태 문제점 파악 가능하다.

### **HTTP Header**

- HTTP 메시지의 헤더에는 콘텐츠 관련 정보(Content-Type),인증 관련 정보(Authorization), 쿠키 정보, 캐시 관련 정보 등 서버와 클라이언트간 통신 시 필요한 정보를 담는다.
- 클라이언트 요청 시,서버 응답 시 모두 헤더에 정보를 담을 수 있다.

### **HTTP Status**

- HTTP 요청 시, 클라이언트는 요청의 **결과**에 대한 상태 정보를 얻는다.
- 200, 400, 500 등 숫자 코드와, OK, NOT FOUND 등의 텍스트로 이루어진다.
- 코드를 이용해 각 결과에 해당하는 행위를 할 수 있다.

### **HTTP 요청 메서드**

- HTTP에서 클라이언트가 서버로 요청을 보낼 때 사용한다.
- 요청 시 요청 메서드로 특정 요청에 대한 동작을 정의한다.
- **GET, POST, PUT, PATCH, DELETE 등**

### **REST API(Representational State Transfer API)**

- HTTP 규약을 어떻게 잘 활용을 해서 서버와 클라이언트 사이의 통신을 구축할 수 있을까?
- API(Application Programming Interface): 사용자가 특정기능을 사용할 수 있도록
  제공하는 함수를 의미한다.
- REST API는 HTTP의 요청 메서드에 응하는 서버 API와 클라이언트 간 통신의 구조가 지켜야 할 좋은 방법을 명시한 것이다.
- 구체적인 내용으로는 요청 메서드의 의미, URI 설계, 클라이언트의 상태에 대한 동작 등을 정의한다.

### **Fetch API**

- 기존 XMLHTTPRequest를 대체하는 HTTP 요청 API로 **Promise를 리턴**하도록 정의되어 있다.
- 네트워크 요청 성공 시, Promise는 Response 객체를 resolve 한다.
- 네트워크 요청 실패 시, Promise는 에러 reject 한다.
- response.ok → **`response.ok`** 는 HTTP Status code가 200-299 사이면 true, 그외는 false이다.

## **HTTP status code**

HTTP status code(응답 상태 코드)는 특정 HTTP 요청이 성공적으로 완료되었는지 알려주는 코드이다. 총 응답은 5개의 그룹으로 나누어진다. ([section 10 of RFC 2616](https://tools.ietf.org/html/rfc2616#section-10)에 정의)

- 응답: 100
- 성공적인 응답: 200
- 리다이렉트: 300
- 클라이언트 에러: 400
- 서버 에러: 500

예를 들어, 우리가 가끔 알 수 없는 URL로 들어갔을 때, **404 Error** 가 나오는 페이지를 많이 봤을 것이다.

- 추가적으로 404에러는 Not Found로 클라이언트가 HTTP를 통해 송신 요청한 정보를 서버가 가지고 있지 않을 때 등장한다. URL 중 서버까진 맞았는데 그 다음이 틀렸을 때 서버가 보유하지 않은 정보 자원임을 의미한다.

<p align="center"><img width="900" alt="2" src="https://user-images.githubusercontent.com/65716445/204492149-c0863a62-6ac3-41d0-bc23-f6c2151e96bb.png"></p>
## **HTTPS**

- HTTPS(HyperText Transfer Protocol over Secure Socket Layer)는 HTTP를 보완하는 수단이다.
- HTTP는 암호화되지 않았기 때문에 도난이나 변조, 도청이 가능하다.
- HTTPS는 HTTP의 일반 텍스트(text)에 SSL(보안 소켓 계층)이나 TLS프로토콜을 씌워 통해 데이터를 암호화하는 기법이며, 로그인이나 결제화면에서 주로 쓰인다.
- SSL은 서버와 브라우저 사이에 안전하게 암호화된 연결을 만들 수 있게 도와주고, 서버 브라우저가 민감한 정보를 주고받을 때 이것이 도난당하는 것을 막아준다.
- HTTPS를 사용하는 경우 URL에서 http://가 아닌 https://를 사용한다.

## **HTTPS의 보안성**

<p align="center"><img width="500" alt="2" src="https://user-images.githubusercontent.com/65716445/204492158-9e790cab-a070-4346-84d4-628abcaed4a7.png"></p>

- ‘HTTP vs HTTPS 차이’는 바로 SSL 인증서이다.
- HTTPS는 쉽게 말해서 HTTP 프로토콜에 보안 기능을 추가한 것이다.
- **SSL 인증서는 사용자가 사이트에 제공하는 정보를 암호화하는데, 쉽게 말해서 데이터를 암호로 바꾼다고 생각하면 쉽다. 이렇게 전송된 데이터는 중간에서 누군가 훔쳐 낸다고 하더라도 데이터가 암호화되어있기 때문에 해독할 수 없다.**
- **그 외에도 HTTPS는 TLS(전송 계층 보안) 프로토콜을 통해서도 보안을 유지한다.**
- TLS은 데이터 무결성을 제공하기 때문에 데이터가 전송 중에 수정되거나 손상되는 것을 방지하고, 사용자가 자신이 의도하는 웹사이트와 통신하고 있음을 입증하는 인증 기능도 제공하고 있다.

<aside>
💡 HTTPS는 SSL을 사용함으로 HTTP보다 보안성이 우수하다.

</aside>

## HTTPS 확인 방법

브라우저에서 URL을 확인하여 웹사이트에 HTTPS 보호 기능이 있는지를 확인할 수 있다. **도메인 이름 앞에 자물쇠 아이콘이 있으면 이 사이트는 HTTPS로 인해 안전한 것이다.**

<p align="center"><img width="600" alt="2" src="https://user-images.githubusercontent.com/65716445/204492170-a624f321-6ca4-4aee-83f7-fa3380bcbc7f.png"></p>

### Google 순위 요소로서의 SSL

- 웹 사이트를 HTTPS에서 실행하려면 SSL(Security Sockets Layer) 인증서가 필요하다.
- Netscape에서 개발한 이 인증서는 웹 사이트의 데이터를 암호화하고 웹 사이트 방문자에게 당신의 웹 사이트가 안전하다는 것을 증명하는 역할을 한다.
- [Google 팀은 HTTPS의 필요성을 거듭하여 밝혔다.](https://developers.google.com/search/blog/2014/08/https-as-ranking-signal) 또한 이를 기반으로 하는 알고리즘 업데이트도 출시했다. HTTPS 보안이 없는 사이트는 SERP에서 높은 순위를 얻기 어렵다.

## HTTPS의 SEO 효과 4가지

### 1. 더 나은 사용자 경험 제공

- 사용자 경험(UX)은 SEO의 큰 부분을 차지한다.
- 사용자들이 검색을 통해 웹 사이트에 방문했을때 지나치게 깜박거리는 텍스트나 수많은 팝업 광고 등이 먼저 등장하게 된다면 그들은 더 이상 사이트에 머물지 않는다.
- 또한 SSL 인증서가 없는 안전하지 않은 사이트는 Google이 사이트에 대해 높은 순위를 달성하기 위해 설정한 “고품질의 신뢰할 수 있는 안전한 웹 사이트” 기준에 맞지 않는다.

<p align="center"><img width="600" alt="2" src="https://user-images.githubusercontent.com/65716445/204492180-188eaca5-0368-4676-ad7f-4f1219692eca.jpeg"></p>

실제로 Google은 안전하지 않은 사이트에 대해 매우 엄격하므로 [Chrome 업데이트](https://blog.chromium.org/2018/02/a-secure-web-is-here-to-stay.html)에서는 사용자가 SSL 인증서가 없는 사이트를 방문할 때 사용자에게 알리고, 암호화되지 않은 웹 사이트에 “Not secure”, 즉 “안전하지 않음”이라는 레이블을 지정한다.

- 안전하지 않은 HTTP 사이트 표시에 대한 예시

<p align="center"><img width="600" alt="2" src="https://user-images.githubusercontent.com/65716445/204492183-edfad238-6048-40dc-924f-0fc2c68934e7.png"></p>

- 예시와 같은 경고 표시는 웹 사이트를 계속 방문하는 것에 대해 사용자가 다시 한 번 생각해보도록 할 것이다. 이는 좋은 사용자 경험이나 높은 검색결과 순위로 이어지지 않게 된다.

### 2. 사이트 체류시간 증가

- 사용자가 검색결과로 돌아가기 전에 웹 페이지를 분석하는 데 걸리는 시간인 [체류시간은 SEO에 중요한 요소](https://blog.hubspot.com/marketing/dwell-time)다. (웹 페이지의 체류시간이 짧다면 페이지가 사용자의 검색의도와 일치하지 않는다는 의미)
- HTTPS가 없는 웹 사이트는 사용자가 “안전하지 않음”이라는 메시지를 직면하게 하여 페이지 체류시간을 줄이는데 영향을 미친다.
- 그 결과 Google은 당신의 사이트를 품질이 낮거나 해당 검색어와 관련이 없는 것으로 판단하여 해당 콘텐츠가 최적화되어 있더라도 웹 사이트 순위를 낮추게 된다.

### 3. 사이트 로딩 속도 개선

- HTTPS는 HTTP보다 [300% 이상 더 빠르게 로딩](http://www.httpvshttps.com/)되기 때문에 SEO적으로 유리하다.

- HTTP vs HTTPS 로딩 속도 비교 테스트

<p align="center"><img width="600" alt="2" src="https://user-images.githubusercontent.com/65716445/204492189-53f5ab48-63f4-4e47-968f-7adaea18b988.png"></p>
### 4. SEO 전략 확인 및 검증

- HTTPS를 사용하는 보안 웹사이트는 분석 대시보드에서 해당 리퍼러(Referral) 정보를 보호하고 표시한다.
- 웹 사이트에 대한 최적의 트래픽 소스를 명확하게 찾아내고 정확하게 확인할 수 있다.

<p align="center"><img width="600" alt="2" src="https://user-images.githubusercontent.com/65716445/204492193-24b8a3b7-28b5-4fa2-895f-00618c7b8479.png"></p>

<aside>
💡 HTTPS는 HTTP에 비해 SEO에도 유리하다.

</aside>

## 마무리 정리

- HTTPS(https://)는 **SSL(Secure Socket Layer) 인증서를 사용**하는 HTTP(http://)이다.
- SSL(또는 TLS) 인증서는 일반 HTTP 요청 및 응답을 암호화한다. 따라서 **HTTPS는 HTTP보다 더 안전한 보안용 프로토콜**이다.
- HTTP와 HTTPS의 유일한 차이점 → HTTPS를 사용한 웹 페이지를 통해 전송되는 모든 데이터는 추가적인 보안 계층이 있다. 이를 TLS(전송 계층 보안) 프로토콜이라고 하며, **모든 유형의 데이터는 변경되거나 손상될 수 없는 HTTPS 사이트를 통해 전달되며 제3자로부터 보호된다.**
- 또한 HTTPS는 SEO(검색 엔진 최적화)에서도 유리하다.
