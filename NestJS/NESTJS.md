# NESTJS DataSource

> 엔티티가 아니라 db자체로 만드는거 쿼리빌더쓰지 이거 잘안씀

```typescript
export const dataSource = new DataSource({
  type: 'postgres',
  host: 'localhost',
  port: 5432,
  username: 'postgres',
  password: 'password',
  database: 'DB',
  entities: [__dirname + '/../**/*.entity.{js,ts}'],
  synchronize: true
})


// 복잡한 쿼리사용을 위한 쿼리빌더 옵션 근데 거이 쓸일은 없을듯하다 
// configs에 넣으면 해당 코드나 파일을 못찾음 

// Query
// const user = await dataSource
// .createQueryBuilder()
// .select("startDate,eventType,eventError")
// .from(Event, "event")
// .where("event.eventType = :type", { type: eventType })
// .andWhere("event.eventError = :err", { err: eventError })
// .getOne()
```

# nestjs 추후 다른 파일로~~ build 경로문제

```typescript
tsconfig.json 파일에서 paths 하든 그냥 하든 build시 똑같이 dist/ 경로로 들어감 
build시 파일구조가 달라지면 특히 import 하는 경로가 달라지면 dist/ (배포파일)의 
구조가 달라지게 되는데 이건 baseDir이 './' 일때 즉 따로 src/ 로 설정해주지 않았으
면 문제가 생김 이건 배포폴더 경로인 outDir도 마찬가지 앞으론 미리미리 설정해놓고
주의하자 
배포 파일 구조 달라지면 경로로된 import module들을 못찾는 에러가 발생 
```

**build시 생기는 파일들은 즉 `폴더`가 가장중요 폴더에 따라 dist폴더의 구조가 생기고 아마도`동적 import` 시 해당 폴더경로를 찾아 들어가는 것이기 때문에 처음 폴더 구조만 잘 잡으면 문제 없음**

`tsconfig.ts`의 dir을 잘잡아야하는이유? 다 필요없이 이건 dev모드뿐만 아니라 build시 에 쓰이는 prod모드에서의 경로도 되기 때문에 시작부터 굉장히 잘 설정해줄 필요가있음

무조건 해주자 경로 안맞아서 머리아픔

`tsconfig.json`과 실제 import받는 src의 경로가 서로 달라서 에러나고 dist/ 에서도 달라서 머리아픔 그래서 결국 이런식으로 해결

```typescript
// tsconfig.json
"baseUrl": "./",
    "paths":{
      "@src/*":["./src"],
      "@setting/*":["@src/../*"]
    },
```

### tsconfig.json paths 모듈 못찾는 문제

```
https://velog.io/@davelee/typescript-paths-%EA%B4%80%EB%A0%A8-%EB%AC%B8%EC%A0%9C-3%EA%B0%80%EC%A7%80%EC%99%80-%ED%95%B4%EA%B2%B0%EC%B1%85
```

### typescript dynamic json import

```
https://datacadamia.com/web/javascript/module/dynamic_import
```

# NESTJS winston logs

일단 http 접근이 없는 용도라 logs 저장만 해놓음 좋은 웹페이지

```
https://velog.io/@aryang/NestJS-winston%EC%9C%BC%EB%A1%9C-%EB%A1%9C%EA%B7%B8-%EA%B4%80%EB%A6%AC#winstonutilts
```

```
https://choseongho93.tistory.com/317
```

# 의존 주입

service나 module를 서로 import하려고 하다보면 에러가 날때가 있는데 

그때 `forwardRef(() => ModuleB)` 같은 식으로 의존주입하여 해결한다

또는 생성자 부분에서 

```typescript
 @Inject(forwardRef(() => ModuleBService))
```

같은 방법으로도 가능하다 

# Nest MiddleWare

- Pipes: 유효성체크, 요청이 컨트롤러에 도달하기 전에 클라이언트에서 보낸 값을 미리 봐서 미리 정수변환같은 작업이나 에러를 던짐
  
  - 요청 유효성 검사 및 페이로드 변환을 위해 만들어짐 데이터를 예상한대로 직렬화함

- Filters:  front단에서 Catch 를 받기전 back쪽에서 한번 들렸다 (가는곳 원하는 에러 지정가능)
  
  - 필터는 오류처리 미들웨어. 특정 오류 처리기를 사용할 경로와 각 경로 주변의 복잡성을 관리하는 방법을 알 수 있음

- Guards: 인증 미들웨어 특정조건을 만족하냐 못하냐에 따라 걸러줌 컨트롤러 전에 미리 가드에 잡힘 

- interceptors: 인터센터는 응답 매핑 및 캐시 관리와 함께 요청 로깅과 같은 전후 미들웨어 입니다. 각요청 전후에 이를 실행하는 기능은 매우 강력하고 유용합니다.

    

## 각 미들웨어 불러지는 (called)순서

middleware -> guard -> interceptor (before) -> pipe -> controller ->

service -> controller -> interceptor (after) -> fillter ( if applicable) -> client

# 쿼리빌더 .getMany() .getRawMany() 차이?

일단 many는 전부 가져온다는건데 raw가 붙게되면 엔티티에 포함되지 않는 `typeof any`로 가져온다는거 같다 many를 사용하면 엔티티 타입을 가져온다. 
