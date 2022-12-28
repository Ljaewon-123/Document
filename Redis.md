# NestJS in Redis

# Redis

```우와한
https://www.youtube.com/watch?v=mPB2CZiAkKM
```

- Sorted set 의 score는 double 타입이기 때문에 표현이 안되는 값은 다르게 나올수 있음!

## Redis Config recommand

실사용은 클라우드 올리고 나서???

```
Maxclient 설정 50000
RDB/AOF 설정 OFF
commands disable 특정
- KEYS
- SAVE
위 두 개에서 에러많이

적젏한 ZIPLIST사
```

KEYS => scan 으로 대체가능

설정파일 차이  [ 그러니 할때는 windows-service.conf ]

```tip
redis conf 설정 파일 차이점
redis.windows-service.conf : 서비스/데몬으로 실행될때 실행되도록 되어 있다. 백그라운드에서 실행되고 OS에 의해 관리된다.
redis.windows.conf : 명령줄이나 스크립트에서 실행되고 사용자 공간에서 관리된다.
```

외부 접근 허용 IP 설정

conf파일에서 bind 부분 찾아서 주석풀고 허용 아이피 써주면됨

RDB/AOF OFF 설정

```
https://kimdubi.github.io/redis/redis_persistence/
```

위가 좀더 자세한 한글 블로그

```
https://stackoverflow.com/questions/27681402/how-to-disable-redis-rdb-and-aof
```

```
http://redisgate.kr/redis/server/redis_conf_han.php
```

**위는 redis.config 한글문서**

## Ziplist 구조

속도는 느리나 메모리 사용량이 hast,set 등등에 비해서 20~30%정도 빠르다

list,hash,sorted set을 ziplist로 대체하는 설정이 존재하고 해당 설정값을 넘어가면 다시 자료구조가 본래로 돌아와서 속도는 유지하는데 메모리는 더 많이 낭비할수가 있다 프로젝트에 따라 설정 -> 지금은 redis가 빠르게 보여줘야 하니까 안해도?

## O(N) 관련 명령어

각 명령어마다 빅O가 다른 명령어가 있다 어느 명령어가 O(N)인데 어느명령어는 O^2라서 에러가 발생할 수 있다.

## Monitoring

- info 를 통한 정보
  
  - RSS 제일 중요
  
  - Used Memory 
  
  - Connection 수
  
  - 초당 처리 요청 수

- System
  
  - Cpu
  
  - Disk
  
  - Network rx/tx

## Consistent Hashing

요약하자면 서버가 증설되거나 죽었을때도 최소한에 데이터 리벨런싱만 일어나게 만듬

=> 서버를 hash키로 만들어서 데이터가 서버와 가장가까운 hash값으로 들어가게 만듬

# ioRedis

현재 프로젝트에서는 공홈대로 nestJS에서 ioRedis를 사용한다

```
https://docs.nestjs.com/microservices/redis
```

현 프젝에서 이미 mqtt로 Microservice를 쏴줘서 redis를 어찌쓰나 싶었지만 패키지만 공홈대로 ioredis를 쓰고 pub/sub을 사용하지 않으니 그냥 ioredis자체 기능으로 사용함

하지만 NestJS대로 app구분을 잘되지않았다 애초에 mqtt를 sub하는순간 DB와 Redis를 끝내니 그런거 같기도 하고

일단 ioRedis공홈

```
https://www.npmjs.com/package/ioredis
```

> 예제 잘되있음

예제 페이지 가면 다있다 그외 설정도 있지만 위는 일단 TypeScript 지원 위주긴 한대

하나 더 찾음 여기는 매소드자체는 훨씬많은거 같다

```
https://luin.github.io/ioredis/interfaces/ChainableCommander.html
```

> npmjs보다 훨씬 매소드 많음 

# scan

```
https://redis.io/commands/scan/
```

문서 아래로 내려서 확인잘하고 

(empty set or list 이런 명령어가 떴다면 count의 default가 10이니까 늘리거나 cursor을 움직이자 )

진짜 다른 블로그 답답해 뒤지는줄 알았음

흔히 말하는 scan에서의 cursor은 반복값임 count가 많으면 상관없지만 그렇지 않다면 cursor이 0 이 나올때까지 돌아야 한바퀴 전부 도는거라고 할수있음 

```typescript
 async getKeys(deviceName:string,siteId:number){
    let lst = [] as string []
    redis
    const stream = redis.scanStream({match:"ESS:"+siteId+":"+deviceName+"*"})
    const data = new Promise(() => {stream.on("data", (resultKeys) => {
      for (let i = 0; i < resultKeys.length; i++) {
        console.log(resultKeys[i]);
        lst.push(resultKeys[i])
      }
      stream.close()
      // Promise.all(resultKeys).then( res => {
      //   for (let i = 0; i < resultKeys.length; i++) {
      //     console.log(resultKeys[i]);
      //     lst.push(resultKeys[i])
      //   }
      // })
    })})
    stream.on("end", () => {
      console.log("all "+deviceName+" keys have been visited");

    });
    await data
    console.log(lst)
  }
```

스트림은 종료되는 놈이 아니라 계속 흐르는 놈이다.. 
