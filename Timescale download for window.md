# Timescale download for window

필수적 선행 패키지

- OpenSSL v3.x
- [Visual C++ Redistributable for Visual Studio 2015](https://www.microsoft.com/en-us/download/details.aspx?id=48145)

### timescale download url

```
https://docs.timescale.com/self-hosted/latest/install/installation-windows/
```

여기서 다운로드 받을수있다. 

### setup to timescaledb

기본적으로 환경변수 설정이 안되어있으니 해주고 

`C:\Program Files\PostgreSQL\<version>\bin`

psql 접속후에 

```
CREATE EXTENSION IF NOT EXISTS timescaledb;
```

`\dx`로 확인해주면 된다. 

```plsql
 \dx
                                                     설치된 확장기능 목록
 \u000C\xB8\xA7 |  \xB9  | \xBD\xBA키\xB8\xB6 |                                     \xBC\xB3\xB8

----------------+--------+-------------------+---------------------------------------------------------------------------------------
 plpgsql        | 1.0    | pg_catalog        | PL/pgSQL procedural language
 timescaledb    | 2.18.0 | public            | Enables scalable inserts and complex queries for time-series data (Community Edition)
(2개 행)
```

이렇게 나와야 된거

## Troubleshooting

--- 

### 오류: 문자열 안에 ' 사용이 안전하지 않습니다.

```
 create extension if not exists timescaledb;
오류:  문자열 안에 \' 사용이 안전하지 않습니다
힌트:  작은 따옴표는 '' 형태로 사용하십시오. \' 표기법은 클라이언트 전용 인코딩에서 안전하지 않습니다.
```

이딴 문자뜨면 

```
SET client_encoding = 'UTF8';
SET
```

바꿔줘야 제대로된 반응을 확인할수있다. 

---

### Error: extension "timescaledb" must be preloade

```
 CREATE EXTENSION IF NOT EXISTS timescaledb;
移섎챸?곸삤瑜?  extension "timescaledb" must be preloaded
힌트:  Please preload the timescaledb library via shared_preload_libraries.

This can be done by editing the config file at: C:/Program Files/PostgreSQL/17/data/postgresql.conf
and adding 'timescaledb' to the list in the shared_preload_libraries config.
        # Modify postgresql.conf:
        shared_preload_libraries = 'timescaledb'

Another way to do this, if not preloading other libraries, is with the command:
        echo "shared_preload_libraries = 'timescaledb'" >> C:/Program Files/PostgreSQL/17/data/postgresql.conf

(Will require a database restart.)


서버가 갑자기 연결을 닫았음.
        이런 처리는 클라이언트의 요구를 처리하는 동안이나
        처리하기 전에 서버가 갑자기 종료되었음을 의미함.
서버로부터 연결이 끊어졌습니다. 다시 연결을 시도합니다: 성공.
```

이거뜨면 성공이라고 봐도된다.

```
C:/Program Files/PostgreSQL/17/data/postgresql.conf
```

경로에서 `shared_preload_libraries = 'timescaledb'` 추가해주면 된다 

```
https://stackoverflow.com/questions/68435038/fatal-extension-timescaledb-must-be-preloaded-hint-please-preload-the-timesc
```

참고

restart를 필히 해주어야하는데 

## 🛠 **3. 수동으로 서비스 중지 및 시작 (GUI 방법)**

1. `Win + R` → `services.msc` 입력 후 실행
2. PostgreSQL 관련 서비스를 찾아서 선택
3. 다시시작 

그이후 

```
 create extension if not exists timescaledb;
CREATE EXTENSION
```

하고 `\dx`확인하면 끝이다. 

### could not load library "C:/Program Files/PostgreSQL/17/lib/timescaledb-2.17.2.dll": The specified module could not be found.

필자는 이거 종속성 다운로드 받아서 dll확인하고 다해봤는데 안돼서 지우고 다시했다. 

end
