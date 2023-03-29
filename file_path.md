# **dirname VS** filename VS process.cwd()

https://velog.io/@rlwjd31/dirname-filename-process.cwd

## why?

vue.config.js 에서

```ts
builderOptions: {
        extraResources: ['src/jsonSetup'],
}
```

같은 외부 json 파일을 사용하게 될경우 

```ts
import { writeFileSync, readFileSync } from 'fs';

const jsonString = JSON.stringify(ANYDATA)
writeFileSync(process.env.VUE_APP_JSON_URL+'jsonFile.json', jsonString) 
const someJSONdata = JSON.parse(readFileSync(process.env.VUE_APP_JSON_URL+'jsonFile.json','utf-8'))
```

`.env` 과 `.env.production`파일을 사용해서 경로를 설정하고

`fs`모듈을 사용하게 되는데 좀처럼 경로가 잡히지 않아 디렉토리를 찾지 못하는 에러가 발생했었음 

```ts
console.log(process.env.BASE_URL)
console.log(__dirname)
console.log(process.env.NODE_ENV)
console.log(process.env.VUE_APP_JSON_URL)
console.log(process.cwd()) // 현재작업경로
```

__dirname으로는 좀처럼 잡히지 않다가 `process.cwd()`를 사용하고 나서야 잡혔다 그런데 

build 환경에서는 서로다른 cwd경로가 뜨길래 뭔가 해서 알아봤더니 

node 실행경로?? 그랬다면 말이된다 개발은 윈도우에서 하지만 

build파일은 linux에서 APP형식으로 나타내는데 클릭실행과 파일실행 경로가 달라서 한번 찾아봤다. 

## 실행위치 상관없이 자유롭게 실행시켜주고 싶다면 `.env , .env.production`파일을 절대경로로 만들어줘도 된다.


