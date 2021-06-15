#### HTTP 메시지

- ASCII로 인코딩된 텍스트 정보, 여러 줄로 되어있음.
- HTTP/1.1에서는 클라이언트 서버 연결을 통해 공개적으로 전달됨
- HTTP/2에서 최적화와 성능 향상을 위해 HTTP 프레임으로 나누어짐
- 소프트웨어, 브라우저, 프록시, 웹 서버가 HTTP 메시지를 텍스트로 작성
- HTTP 메시지는 설정파일(프록시 혹은 서버의 경우), API(브라우저의 경우) 혹은 다른 인터페이스를 통해 제공됨

![From a user-, script-, or server- generated event, an HTTP/1.x msg is generated, and if HTTP/2 is in use, it is binary framed into an HTTP/2 stream, then sent.](https://mdn.mozillademos.org/files/13825/HTTPMsg2.png)

- 사용 중인 API나 설정 파일 등을 변경하지 않도록 설계

##### 요청

- START Line
  - 시작 줄 +  HTTP 헤더를 묶어서 head라고 부름
  - 시작 줄
    - HTTP 메서드 (GET, PUT, POST or HEAD, OPTIONS) 를 이용해 서버가 수행해야 할 동작을 나타냄
    - 요청 타겟
    - HTTP 버전
  - 헤더
    - General 헤더
    - Request 헤더
    - Entity 헤더
- EMPTY Line
- Body 
  - HTTP 메시지의 Payload
  - Body의 종류
    - 단일-리소스 본문(single-resource bodies) : 헤더 두 개(Content-Type, Content-Length)로 정의된 단일파일로 구성됨
    - 다중-리소스 본문(multiple-resource bodies) : 멀티파트 본문으로 구성되는 다중 리소스 본문에서는 파트마다 다른 정보를 지니게 됨. 보통 HTML 폼과 관련됨



##### 응답

- 시작 줄

  - 상태 줄(status line)이라고 부름. 
  - 프로토콜 버전: 보통 HTTP/1.1
  - 상태 코드 : 요청의 성공여부를 나타냄. 200, 404 혹은 302.
  - 상태 텍스트 : 상태 코드에 대한 짧은 설명 글
  - HTTP/1.1 404 Not Found.

- 헤더

  - 다른 헤더와 동일한 구조를 지님
  - General 헤더
  - Request 헤더
  - Entity 헤더 : Content-Length와 같은 헤더는 요청 본문에 적용됨

- 본문

  - 201, 204와 같은 상태 코드 응답에는 본문이 없을 수 있음

  - 본분은 크게 세 가지 종류로 나뉨

    - 이미 길이가 알려진 단일 파일로 구성된 단일-리소스 본문
      - 헤더 두 개(Content-Type과 Content-Length 로 정의)
    - 길이를 모르는 단일 파일로 구성된 단일-리소스 본문
      - Transfer-Encoding이 chunked로 설정되어있음
      - 파일은 chunk로 나뉘어 인코딩되어 있음
    - 서로 다른 정보를 담고 있는 멀티파트로 이루어진 다중 리소스 본문

    

##### HTTP/2 프레임

- HTTP/1.x 메시지는 성능상 결함을 몇가지 내포함
  - 본문은 압축이 되지만 헤더는 압축이 안됨
  - 연속된 메시지들은 비슷한 헤더 구조를 띄기 마련인데, 메시지마다 반복되어 전송됨
  - 다중전송(multiplexing)이 불가능. 서버 하나에 연결을 여러 개 열어야함
- HTTP/2에서는 HTTP/1.x 메시지를 프레임으로 나누어 스트림에 끼워 넣음
- 데이터와 헤더 프레임이 분리되었기 때문에 헤더를 압축할 수 있음
- 스트림 여러개를 하나로 묶을 수 있어서(멀티 플렉싱), TCP 연결이 좀 더 효율적으로 이루어짐

![HTTP/2 modify the HTTP message to divide them in frames (part of a single stream), allowing for more optimization.](https://mdn.mozillademos.org/files/13819/Binary_framing2.png)

- HTTP 프레임은 HTTP/1.1 메시지와 그 기저를 이루는 전송 프로토콜 사이를 메워주는 존재임