# pm2 실행 설정 , 설정파일

```typescript
module.exports = {
  apps: [
    {
      name: 'app',
      script: 'server.js',
      instances: 4,
      exec_mode: 'cluster', // 실행 모드. cluster로 명시하지 않으면 기본 fork 모드가 된다.
      wait_ready: true, // Node.js 앱으로부터 앱이 실행되었다는 신호를 직접 받겠다는 의미
      listen_timeout: 50000, // 앱 실행 신호까지 기다릴 최대 시간. ms 단위.
      kill_timeout: 5000, // 새로운 프로세스 실행이 완료된 후 예전 프로세스를 교체하기까지 기다릴 시간
      max_memory_restart: '500M',
      env: {
        NODE_ENV: 'development',
      },
      env_production: {
        NODE_ENV: 'production',
        PORT: '8081',
      },
    },
  ],
}
PM2 Ecosystem File Reference
```

```typescript
    "start:prod": "pm2 start dist/main.js",
    "start:reload":"pm2 reload dist/main.js",
빌드 후에 package.json 파일 수정해서 
    이런식으로 해도됨 위에 script 부분이 dist/main.js 가 되겠지
```
