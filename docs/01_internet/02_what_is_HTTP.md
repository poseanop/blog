---
sidebar_position: 2
---

# What is HTTP?

> 1월 넷째주 글

### HTTP란

- 하이퍼텍스트 전송 프로토콜(HTTP)
- HTML과 같은 하이퍼미디어 문서를 전송하기 위한 **어플리케이션 레이어 프로토콜**
- 웹 브라우저와 웹 서버간의 통신을 위해 설계되었지만 다른 목적으로도 사용가능함.
- 클라이언트가 요청을 하기 위해 연결을 연 다음 응답을 받을때 까지 대기하는 전통적인 클라이언트-서버 모델을 따른다.
  - 연결 자체는 전송계층에서 제어되므로 HTTP 영역의 밖이지만, 연결을 의존하기 때문에 TCP 표준에 의존함.
- 무상태 프로토콜이며 어떤 데이터(상태)도 유지하지 않는다. (상태는 없지만 쿠키를 통해 세션은 만들 수 있다.)

### 클라이언트

- 대부분은 웹 브라우저
- 브라우저는 항상 요청을 보내는 개체. 결코 서버가 될 수 없다.
- 브라우저는 HTTP 요청에 대한 HTTP 응답을 해석하여 화면에 표시해준다.

### 서버

- 클라이언트(브라우저)의 요청에 대한 문서를 제공하는 서버
- 서버를 분산할 수도 있고 `HTTP/1.1`과 `Host 헤더`를 이용하여, 동일한 IP 주소를 공유할 수도 있다.
- **프록시** : 클라이언트 <-> 서버 사이에 존재하는 서버
  - 터널링 : 실제 주소를 숨기기 위한 프록시
  - 캐싱
  - 필터링
  - 인증
  - 로깅
  - 로드밸런싱 : 서버가 트래픽에 부하되지 않도록 여러대의 서버로 나누어 처리하는 것

### HTTP 흐름

1. TCP 연결을 연다.
2. HTTP 메시지를 전송한다. (HTTP/2 부터는 직접 읽는게 불가능)

```
GET / HTTP/1.1
Host: developer.mozilla.org
Accept-Language: fr
```

3. 서버에서 온 응답을 읽는다.

```
HTTP/1.1 200 OK
Date: Sat, 09 Oct 2010 14:28:02 GMT
Server: Apache
Last-Modified: Tue, 01 Dec 2009 20:18:22 GMT
ETag: "51142bc1-7449-479b075b2891b"
Accept-Ranges: bytes
Content-Length: 29769
Content-Type: text/html

<!DOCTYPE html... (here comes the 29769 bytes of the requested web page)
```

4. 연결을 닫거나 재요청

### MIME type vs Content type

- 둘다 전송되는 미디어의 타입을 의미함.
- MIME은 더 넓게 Internet 에서 사용되는 개념이고
- Content type은 웹에서 사용되는 개념이다.

### HTTP/1.1 – 표준 프로토콜

- 커넥션 재사용으로 시간 절약
- 파이프라이닝으로 레이턴시 낮춤
  - 레이턴시 : 자극과 반응 사이의 시간
- 청크 응답
- 캐시 ([Cache-Control](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Cache-Control) 헤더로 제어)
- HOST 헤더로 코로케이션 가능해짐
  - 코로케이션 : 서버를 호스팅 전문업체에 위탁하여 사용하는 것 (<->호스팅)

### HTTP 확장

- ssl (보안 레이어)
- REST API로 읽기 뿐만 아니라 저작이 가능해짐 (SPA)
  - 단점은 각각의 웹사이트에서 자신들만의 비표준 RESTful API를 정의하고 그에 대한 전권을 가진다는 사실
- 서버 전송 이벤트 : 서버가 브라우저로 이따금씩 보내는 메시지를 푸쉬할 수 있는 곳.
- 웹소켓 : 기존 HTTP 커넥션을 업그레이드하여 만들 수 있는 새로운 프로토콜.
- Same Origin Policy (동일출처정책)과 독립적인 특성으로, 보안제약 완화.
  - 그렇기에 헤더로 정책에 대한 리소스 공유 제어가 가능하다

### HTTP/2 – 더 나은 성능을 위한 프로토콜

- 텍스트 프로토콜 -> 바이너리 프로토콜
- 메시지 -> 프레임
- 더이상 read 불가능
- **다중화 프로토콜**/다중전송(multiplexing) : 동일한 커넥션 상에서 병렬 요청 가능
- 헤더 압축으로 **오버헤드** 개선
  - 오버헤드 : 어떤 처리를 하기 위해 들어가는 **간접**적인 처리 시간 or 메모리

### HTTP/3 - QUIC

- UDP 사용
- 연결 설정 시간 단축
- HTTP/2는 최초 연결 요청부터 전송까지 2~3회의 추가 트래픽이 소요되지만, HTTP/3은 연결과 동시에 실제 데이터 전송이 가능한 방식이다.

### CORS

- 브라우저에서 교차출처의 HTTP 요청은 제한함. ([Same Origin Policy](https://developer.mozilla.org/ko/docs/Web/Security/Same-origin_policy) 라고 함)
  - link, script, img 태그는 가능
- **CORS** : 추가 HTTP 헤더를 사용하여, 교차출처 요청을 가능하게 함
- 해결방법
  1. JSONP 이용
  2. 프록시 서버를 이용
  3. 서버측 `Access-Control-Allow-Origin` 헤더 제어 (credentials 사용시에는 주소명시 필수!!)

### 보안

- **CSP (콘텐츠 보안 정책)** : XSS (Cross-Site Scripting ) 및 데이터 삽입 공격 등 공격을 완화하는 보안 정책
  - HTTP 헤더를 통해 지정

```
Content-Security-Policy: default-src 'self' example.com *.example.com
```

- **SSL (Secure Sockets Layer)** : 데이터를 안전하게 보장하는 과거의 보안 표준 기술
- **TLS (전송계층 보안)** : 어플리케이션들이 네트워크 상에서 안전하게 통신하기 위해 사용된 프로토콜
- **HTTPS (HTTP Secure)** : SSL/TLS를 사용하는 HTTP 프로토콜의 암호화된 버전 (HTTP/2과 필요충분조건)
- **x-xss-protection:1; mode=block;** : param에 객체타입이 들어오는 등 위험이 감지되면 페이지 렌더링 X
- 가용 서버 부족으로 부하를 줄이기 위해 보안 조건을 올리는 경우도 있음.

### 추가로 공부하고싶은 부분

- OSI 7계층 -유튜브 (새 포스팅)
- [url](https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/Identifying_resources_on_the_Web) (새포스팅)
  - param, 스키마 등
