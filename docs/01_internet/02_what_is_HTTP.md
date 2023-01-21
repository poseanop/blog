---
sidebar_position: 2
---

# What is HTTP?

> 1월 넷째주 글

### HTTP란

- 하이퍼텍스트 전송 프로토콜(HTTP)은 HTML과 같은 하이퍼미디어 문서를 전송하기 위한 애**플리케이션 레이어 프로토콜**
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
- 청크 응답
- 캐시
- HOST 헤더로 코로케이션 가능해짐

### HTTP 확장

- ssl (보안 레이어)
- REST API로 읽기 뿐만 아니라 저작이 가능해짐 (SPA)
  - 단점은 각각의 웹사이트에서 자신들만의 비표준 RESTful API를 정의하고 그에 대한 전권을 가진다는 사실
- 서버 전송 이벤트 : 서버가 브라우저로 이따금씩 보내는 메시지를 푸쉬할 수 있는 곳.
- 웹소켓 : 기존 HTTP 커넥션을 업그레이드하여 만들 수 있는 새로운 프로토콜.
- [보안 모델 완화](https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/Evolution_of_HTTP#%EC%9B%B9%EC%9D%98_%EB%B3%B4%EC%95%88_%EB%AA%A8%EB%8D%B8_%EC%99%84%ED%99%94) (same origin 정책과 독립적) : CORS / CSP ...

### HTTP/2 – 더 나은 성능을 위한 프로토콜

- wip...

### 추가로 공부하고싶은 부분

- OSI 7계층 -유튜브 (새 포스팅)
- [QUIC](https://developer.mozilla.org/ko/docs/Web/HTTP/Overview#http%EC%99%80_%EC%97%B0%EA%B2%B0)
- [url](https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/Identifying_resources_on_the_Web) (새포스팅)
  - param, 스키마 등
- [ssl](https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/Evolution_of_HTTP#%EB%B3%B4%EC%95%88_%EC%A0%84%EC%86%A1%EC%9D%84_%EC%9C%84%ED%95%9C_http_%EC%82%AC%EC%9A%A9)
