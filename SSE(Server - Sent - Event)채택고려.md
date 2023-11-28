# SSE(Server - Sent - Event)

websocket에서 단방향 통신만 지원하는 시스템

SSE는 전통적인 HTTP를 통해 전송. **즉, 작동하려면 특별한 프로토콜이나 서버 구현이 필요하지 않음.**

SSE (Server-Sent Events)는 서버에서 클라이언트로 단방향으로 데이터를 전송하는 기술. SSE는 클라이언트와 서버 간의 실시간 통신을 가능하게 하며, 서버에서 주기적으로 업데이트되는 데이터를 클라이언트에게 전송할 수 있다.

## nestjs 문서 예제

```
@Sse('sse')
sse(): Observable<MessageEvent> {
  return interval(1000).pipe(map((_) => ({ data: { hello: 'world' } })));
}
```

```
front 부분에서
  let sse = new EventSource('http://localhost:1101/home/sse')
  sse.onmessage = function(msg) {
    console.log(msg)
  };
```

## SSE 기술의 채택을 고려한 이유와 사용 목적

> 현재 ESS에 메인페이지는 매초마다 api에 redis데이터를 요청한다 나중에 클라이언트가 많아질시 서버와 네트워크 부담을 줄이기위해 사용해보려했음

# 문제

연결이후 문제가 있는데 제대로 사용하기 전에 해결해야할 문제였다 

클라이언트에 요청은 필요없고 오진 서버의 응답만있으면 되는 현재 상황은 sse가 합당하지만 mdn에서 발견

**Warning:** When **not used over HTTP/2**, SSE suffers from a limitation to the maximum number of open connections, which can be specially painful when opening various tabs as the limit is *per browser* and set to a very low number (6). The issue has been marked as "Won't fix" in [Chrome](https://crbug.com/275955) and [Firefox](https://bugzil.la/906896). This limit is per browser + domain, so that means that you can open 6 SSE connections across all of the tabs to `www.example1.com` and another 6 SSE connections to `www.example2.com.` (from [Stackoverflow](https://stackoverflow.com/questions/5195452/websockets-vs-server-sent-events-eventsource/5326159)). When using HTTP/2, the maximum number of simultaneous *HTTP streams* is negotiated between the server and the client (defaults to 100).

문제가 있는데 요약하자만 현재 디폴트설정으로 사용된 http/1.1은 같은 도메인당 6개의 연결정도밖에 사용할수 없다는거다(최대 연결 제한 브라우저마다 도메인당 6개연)  실제로 사용해본 결과 6개 까지만 연결되고 그 이후는 연결되지 않았다 

설령 http/2를 사용한다해도 최대연결수에 한계가 있다면 사용하기에는 불가능 하지 않을까
