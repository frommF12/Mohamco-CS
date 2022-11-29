# 🐵 GET, POST Method

## HTTP Method

웹상에서 클라이언트가 프로토콜을 통해 서버에 데이터를 요청(Request)하면 서버는 이를 처리하여 결과를 클라이언트에 응답(Response)해주는데 이때, 클라이언트가 서버에게 요청하는 방법을 <span style="color: red">`HTTP Method`</span>라고 한다. <span style="color: red">`HTTP Method`</span>에는 8가지의 종류 중 가장 많이 쓰이는 GET와 POST 방식을 소개하고자 한다.
<br>

## GET

> <span style="color: red">`GET`</span> 방식은 클라이언트가 서버에게 어떤 리소스를 요청하는 것이다.

- `GET`은 HTML 페이지를 요청하거나 어떤 API를 통하여 데이터를 읽어들이거나, 이미지를 로딩하기 위하여 자주 사용되는 메소드이다. 즉, 데이터를 받아오는 역할만 수행한다.
- QueryString을 활용해서 데이터를 요청하며, **보안에 아주 취약하다.**
- 대표적인 특징으로 URL에 파라미터(Parameter)를 붙여서 전송하기 때문에 body 영역을 사용하지 않는다.
  - URL 뒤에 `?`를 사용하여 파라미터를 작성하게 되고 `&`를 붙여 여러 개의 파라미터를 구분하게 된다.
- 한번 요청 시 URL 포함 255자까지 전송이 가능하고, HTTP/1.1에서는 2048자까지 가능하다.
  <br>

## POST

> <span style="color: red">`POST`</span> 방식은 클라이언트가 서버로 데이터를 보내고, 서버가 이 데이터를 처리해 달라고 요청하는 것이다.

- `POST`는 생성 및 업데이트를 하기 위해 서버로 데이터를 보내는 데 사용된다. 즉, 서버에 내가 요청한 데이터를 추가로 작업하기 원할 때 사용하는 메소드다.
- 메시지를 전송하거나, 신규 리소스를 생성하거나 클라이언트에 입력된 데이터를 처리하는 프로세스를 서버 측에서 실행하기 위해 자주 사용된다.
- 대표적인 특징으로 `GET`방식과 달리 body 영역에 데이터를 실어 보내 보안이 필요한 부분에 자주 사용된다. - body에 데이터를 실어 보내기 때문에 데이터 전송양에는 **길이 제한이 없어** 용량이 큰 데이터를 보내는데 적합하다.
  <br><br>

## GET과 POST의 차이점

1. <span style="background-color: #ffd33d">**사용용도**</span>
    - `GET`은 사용자에게 필요한 데이터를 받아오는 역할을 하고, `POST`는 사용자의 어떤 행동을 서버에 반영하는 역할을 한다.
    - DB로 예시를 들면,`GET`</span>은 SELECT에 가깝고,`POST`는 CREATE에 가깝다고 볼 수 있다.
2. <span style="background-color: #ffd33d">**캐싱**</span>
    - `GET`의 파라미터들은 URL의 일부분이기 때문에 브라우저 히스토리에 남게되고, `POST`는 브라우저에 기록이 남지 않는다.
    - `GET`은 URL로 인코딩되어 즐겨찾기가 가능하고, `POST`는 즐겨찾기가 불가능하다.
3. <span style="background-color: #ffd33d">**보안**</span>
    - `GET`은 URL을 통해 모든 파라미터를 전달하기 때문에 주소창에 전달 값이 노출되어 보안상 취약하고, `POST`는 데이터가 노출되지 않기 때문에 보안이 필요한 경우에 용이하다.
4. <span style="background-color: #ffd33d">**데이터양**</span>
    - `GET`은 URL 길이가 제한이 있어 전송 데이터양이 한정되어 있고, `POST`는 전송 데이터양 제한이 없다.
5.  <span style="background-color: #ffd33d">**보안**</span>
    - `GET`은 URL 길이가 제한이 있어 전송 데이터양이 한정되어 있고, `POST`는 전송 데이터양 제한이 없다.

<br>

## 나의 의견

`GET`과 `POST`는 둘 다 HTTP 프로토콜을 이용해 서버에 무언가 `request`할 때 사용하는 방식이지만, 사용하는 상황이 다르다는 것을 알 수 있다. 즉, <span style="background: #f1f8ff">가져올 때와 수행할 때를 파악해서 적합한 곳에 활용해야 한다. </span>

<br>


