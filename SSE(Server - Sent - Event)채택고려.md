# SSE(Server - Sent - Event)

websocket에서 단방향 통신만 지원하는 시스템

SSE (Server-Sent Events)는 서버에서 클라이언트로 단방향으로 데이터를 전송하는 웹 기술. SSE는 클라이언트와 서버 간의 실시간 통신을 가능하게 하며, 서버에서 주기적으로 업데이트되는 데이터를 클라이언트에게 전송할 수 있다.

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

### async await 사용이 필수적인데 사용시 데이터 제대로 전달되지 않는 문제

```
sse() {
    return interval(1000).pipe(map( async(_) => {
      const pcs = await this.homeService.getDevice('pcs', 1, 1)
      return  ({ data: { hello: 'world' } })
    }));
```

사용시 전달안됨

에러 x, warn x front쪽 콘솔에 데이터 안찍힘

# 예상이유

SSE가 observable 비동기 스트림을 나타내는 객체인데 promise는 동기

Observable은 해당 Promise를 기다리지 않고 바로 다음 값을 방출


