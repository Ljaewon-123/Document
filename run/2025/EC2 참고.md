# EC2 참고

## mongodb 와 compass연결

### 설치가이드

```
https://www.mongodb.com/ko-kr/docs/manual/tutorial/install-mongodb-on-amazon/
```

### 필수설정 계정 추가

```
https://www.mongodb.com/ko-kr/docs/manual/tutorial/configure-scram-client-authentication/
```

참고 블로그  ( 기본적으로 검색 많이 해볼것 다 조금씩 다른데 프록시 설정은 필요없고 퍼블릭 ip가 맞다. )

```
https://blog.naver.com/n_cloudplatform/223146716115
```

인증접근 

```
mongosh --port 27017  --authenticationDatabase \
    "admin" -u "myUserAdmin" -p
```

## docker

```
https://wscode.tistory.com/112
```

도커 데탑은 나중으로... 당장 GUI는 필요가없다.

## ebs 용량증설

```
https://velog.io/@saakmiso/AWS-EC2-%EC%9A%A9%EB%9F%89-%EC%A6%9D%EC%84%A4%ED%95%98%EA%B8%B0
```

내꺼 로그 

```
 sudo growpart /dev/xvda 1
CHANGED: partition=1 start=2099200 old: size=14677983 end=16777182 new: size=18872287 end=20971486
ubuntu@ip-172-31-9-118:~$ lsblk
NAME     MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
loop0      7:0    0 25.2M  1 loop /snap/amazon-ssm-agent/7993
loop1      7:1    0 38.8M  1 loop /snap/snapd/21759
loop2      7:2    0 55.7M  1 loop /snap/core18/2829
loop3      7:3    0 55.4M  1 loop /snap/core18/2846
xvda     202:0    0   10G  0 disk
├─xvda1  202:1    0    9G  0 part /
├─xvda14 202:14   0    4M  0 part
├─xvda15 202:15   0  106M  0 part /boot/efi
└─xvda16 259:0    0  913M  0 part /boot
ubuntu@ip-172-31-9-118:~$ sudo resize2fs /dev/xvda1
resize2fs 1.47.0 (5-Feb-2023)
Filesystem at /dev/xvda1 is mounted on /; on-line resizing required
old_desc_blocks = 1, new_desc_blocks = 2
The filesystem on /dev/xvda1 is now 2359035 (4k) blocks long.

ubuntu@ip-172-31-9-118:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/root       8.7G  6.4G  2.3G  75% /
tmpfs           479M     0  479M   0% /dev/shm
tmpfs           192M  924K  191M   1% /run
tmpfs           5.0M     0  5.0M   0% /run/lock
/dev/xvda16     881M  133M  687M  17% /boot
/dev/xvda15     105M  6.1M   99M   6% /boot/efi
tmpfs            96M   16K   96M   1% /run/user/1000
```

딱 이대로만 하면된다. 더도말고 덜도말고.

# EC2 메모리 증성 swap사용

```
ubuntu@ip-172-31-9-118:~$ sudo dd if=/dev/zero of=/swapfile bs=128M count=16sudo dd if=/dev/zero of=/swapfile bs=128M count=16^C
ubuntu@ip-172-31-9-118:~$ ^C
ubuntu@ip-172-31-9-118:~$ sudo dd if=/dev/zero of=/swapfile bs=128M count=16
16+0 records in
16+0 records out
2147483648 bytes (2.1 GB, 2.0 GiB) copied, 14.8275 s, 145 MB/s
ubuntu@ip-172-31-9-118:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/root        14G   11G  3.3G  77% /
tmpfs           479M     0  479M   0% /dev/shm
tmpfs           192M  916K  191M   1% /run
tmpfs           5.0M     0  5.0M   0% /run/lock
/dev/xvda16     881M  133M  687M  17% /boot
/dev/xvda15     105M  6.1M   99M   6% /boot/efi
tmpfs            96M   16K   96M   1% /run/user/1000
ubuntu@ip-172-31-9-118:~$ sudo chmod 600 /swapfile
ubuntu@ip-172-31-9-118:~$ sudo mkswap /swapfile
Setting up swapspace version 1, size = 2 GiB (2147479552 bytes)
no label, UUID=760ce928-a108-4232-b38b-1d99cb30d003
ubuntu@ip-172-31-9-118:~$ sudo swapon /swapfile
ubuntu@ip-172-31-9-118:~$ sudo swapon -s
Filename                                Type            Size            Used   Priority
/swapfile                               file            2097148         0      -2
ubuntu@ip-172-31-9-118:~$ sudo vi /etc/fstab
ubuntu@ip-172-31-9-118:~$ free -h
               total        used        free      shared  buff/cache   available
Mem:           957Mi       395Mi       167Mi       932Ki       550Mi       561Mi
Swap:          2.0Gi          0B       2.0Gi
```

참고 사이트 

```
https://velog.io/@kwontae1313/AWS-EC2-%EB%A9%94%EB%AA%A8%EB%A6%AC%EC%9A%A9%EB%9F%89-%EC%A6%9D%EC%84%A4
```

참고

# mongo docker 컨테이너 접속

```
docker exec -it 3f180b2e0219 mongosh -u max -p secret
```

# 필요한 내부 설정들

### db연결

결국 mongo도 redis도 docker container로 대체했다.....
어찌보면 더 편햇을수도...

관련설정 따로 남기겠다.

mongodb url이 핵심 

```env
DATABASE_URL=mongodb://max:secret@mongodb:27017
DBNAME=llama
ENCRYPTION_KEY=2586
AI_PATH=/home/ubuntu/local-ai
```

볼륨으로 데이터 영구저장을 꽤했다.

```docker-compose
version: '3.8'

services:
  redis:
    image: redis:latest
    ports:
      - '6379:6379'
    volumes:
      - redis-data:/data
  mongodb:
    image: 'mongo'
    volumes:
      - mongodb-data:/data/db
    environment:
      MONGO_INITDB_ROOT_USERNAME: max
      MONGO_INITDB_ROOT_PASSWORD: secret


  local-llama:
    build: ./
    ports:
      - '3000:3000'
    volumes:
      - /home/ubuntu/local-ai:/app/local-ai # 호스트 디렉토리를 컨테이너로 마운트
    depends_on:
      - redis
      - mongodb

  nginx:
    image: nginx:alpine
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/conf.d/default.conf
    ports:
      - '80:80'
    depends_on:
      - local-llama

volumes:
  redis-data:
  mongodb-data:
```

### nuxt.config.ts

> 이파일에서 설정과 밀접한 부분만 

```
runtimeConfig:{
  dburl: process.env.DATABASE_URL,
  dbName: process.env.DBNAME,
  llamaName: "Meta-Llama-3.1-8B-Instruct.Q6_K",
  public:{
    encryptionKey: process.env.ENCRYPTION_KEY,
    rootPath: process.env.NODE_ENV === 'production' ? process.env.AI_PATH : process.cwd(),
  }
},
nitro: {
  experimental: {
    websocket: true
  },
  storage: {
    redis: {
      driver: "redis",
      url: "redis://redis:6379",
    },
  },
},
```

### mongodb plugin

```
import mongoose from 'mongoose'


const config = useRuntimeConfig()

const dbOptions = {
  dbName: config.dbName,
  useNewUrlParser: true,
  useUnifiedTopology: true,
}
await mongoose.connect( config.dburl, dbOptions)
```

### PageAuth.ts

> https배포를 하려면 필요했던 옵션들.. 이렇게 안하면 배포시 기억못한다.

```
const isProd = process.env.NODE_ENV === "production";

private redis = useRedis()

async createSession(event: any ){
  /** 보안 강화 하려면 password env로 */
  return await useSession(event, {
    password: "80d42cfb-1cd2-462c-8f17-e3237d9027e9",
    cookie: {
      sameSite: isProd ? "strict" : false, // 개발: false, 배포: Lax
      secure: isProd,                  // 배포: true, 개발: false
      httpOnly: isProd,                // 배포: true
    },
  });
}
```

일단 설정 파일은 이만하면 될듯하다.

혹시 몰라서 추가

Dockerfile

```Dockerfile
FROM node:20.14.0 as build

WORKDIR /app

COPY package.json .

# RUN npm install

COPY . .

RUN npm run build

EXPOSE 3000

CMD [ "node", ".output/server/index.mjs" ]
```

### nginx

> 변함없다.

```nginx
server {
    listen 80;

    server_name localhost;

    location / {
        proxy_pass http://local-llama:3000;
        # proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

end
