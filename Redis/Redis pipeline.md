# Redis pipeline

> redis는 일반적인 clinet-server모델 기반 요청-응답방식인데 pipeline을 사용하여 요청 측면 개선 가능

**redis 파이프라인을 여러 명령을 redis서버로 전송하는데 도움이됨**

**Redis 파이프라인을** 사용하면 서버의 응답을 기다릴 필요가 없음

**Redis 파이프라이닝 방식에서는 모든 요청이 하나의 전송 또는 네트워크 요청으로 서버로 전송** .  또한 네트워크 대역폭을 절약하고 RTT 또는 왕복 시간을 줄일수있다.

redis pipeline의 각 명령은 개별적 시행 

> A Redis client uses pipelining to group several commands. However, **the server has to queue the responses in memory**. Therefore, if we need to send a very large number of commands, **it is better to batch the commands in chunks**. We can send a batch of commands, read the replies and then send the next batch. This will help manage the memory efficiently without affecting the overall processing speed..

청크단위로 명령을 일괄처리하여 효율을 높인다는 내용 

```js
const Redis = require('ioredis');

const redis = new Redis()

async function main() {
    const pipeline = redis.pipeline()

    pipeline.set('foo', 'bar')
    pipeline.get('foo')
    pipeline.set('count', 1)
    pipeline.incr('count')
    pipeline.get('count')
    pipeline.del('foo')

    const results = await pipeline.exec()

    console.log(results)
}

(async () => {
    await main()
})()
```

```js
    pipeline.set('foo', 'bar')
    pipeline.get('foo')
    pipeline.set('count', 1)
    pipeline.incr('count')
    pipeline.get('count')
    pipeline.del('foo')
```

아직 redis로 보내지않고 대기 

`const results = await pipeline.exec()` 여기에서 명령을 보냄 
