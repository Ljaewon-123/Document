# PostgreSQL Partitioning

## 개요

파티셔닝은 시간이 지남에 따라 테이블 크기가 커지는 경우와 같이 

매우큰 테이블이 있는 데이터베이스의 성능을 유지하는데 중요함 ex) pcs, device

큰테이블 하나가있다하면 커질수록 비용이 증가하고 

보통 해당크기에 도달하기전 문제가 발생함 이러한 테이블을 논리적으로 테이블을 나눔으로서 좋은 솔루션이 될수있다 

쿼리가 훨씬더 작은 테이블을 스캔하게함 필요한 데이터를 찾기위한 인덱스 또한 파티셔닝

큰 논리적 테이블을 다른 저장 장치에 배치할 수 있는 더 작은 물리적 테이블로 분할하여 DB확장에 도움이됨 

## 언제필요??

- Table Size 테이블 크기가 클때:
  
  - 긴 응답 시간이 발생하고 테이블 최대치가200GB보다 큰 경우 파티셔닝이 여전히 파티셔닝 옵션이 필요한지 엄격한 규칙이 정해지진 않음 

- Table Bloat : (해석 제대)
  
  - 블롯이 업데이트되고 삭제된 행으로 인해 진공 프로세스를 정리해야하는 데드 튜플이 생성된다는 것을 의미함, 이는 테이블이 클수록 이 프로세스가 공격적이며 더 오래걸미 이는 테이블의 90개 데이터가 변경되지 않은 경우에도 특히 cpu가 더 많은 리소스를 사용함을 의미함 진공(bloat?)프로세스는 여전히 전체 테이블 데이터를 스캔해야 함으 파티션을 분할하는게 유리함 테이블은 더 작은 파티션이 있을때 vacuuming이 필요한 테이블을 줄이는 데 도움이 될수있다 

일부는 더 많은 변경사있고 다른 일부는 더 적다 vacuum프로세스는 처음에 많은 변경 사항이 있는 테이블만 정리한다 이로인해 전체 vacuuming 시간이 줄어들고 더 많은 시스템 리소스를 사용할수 있게 됨니다. 

## 시스템 유지관리

파티셔닝이 아닌 사용자 엑세스는 올바르게 수행되었을 때 테이블의 성능을 크게 향상시킬 수 있지만 필요하지 않은 상태에서 만들거나 잘못 수행되면 오히려 악영향을 불러일으킬수있다.

### 엑세스 패턴 (파티션 정의할때 필수적)

엑세스 패턴은 데이터베이스에 액세스하는 애플리케이션을 알거 로그를 모니터링하고 보고서를 생성하여 볼수있다 그런다음 테이블에 액세스하는 방법에 따라 좋은 파티션 전략에 대

옵션을 가질수있다 

## 두가지타입의 파티셔닝

- Declarative Partitioning 선언적  ( 유지관리가 거의 없으면 대부분 사용사례적합 )
  
  - (10부터 사용가능)Since Postgresql 10
  
  - Minimal Maintenance 
  
  - Best for most usecases 

- Inheritance Partitioning   상속적 ( 구버전 ) 
  
  - Difficult to setup
  
  - More flexible

## How to Create???

```powershell
postgres=# select version();
                          version
------------------------------------------------------------
 PostgreSQL 14.5, compiled by Visual C++ build 1914, 64-bit
(1개 행)


postgres=# create database temp;
CREATE DATABASE
postgres=# \c temp
접속정보: 데이터베이스="temp", 사용자="postgres".
temp=# create table customers (id integer, name text, age numeric) parition by range(age);
오류:  구문 오류, "parition" 부근
줄 1: ...le customers (id integer, name text, age numeric) parition b...
                                                           ^
temp=# create table customers (id integer, name text, age numeric) partition by range(age);
CREATE TABLE

temp=# create table cust_young partition of customers for values from (MINVALUE) to (25);
CREATE TABLE

                               ^
temp=# create table cust_medium partition of customers for values from (25) to (75);
CREATE TABLE

                                                           ^
temp=# create table cust_old partition of customers for values from (75) to (Maxvalue);
CREATE TABLE
temp=# \d+ customers
                               "public.customers" 파티션 테이블
 필드명 |  종류   | Collation | NULL허용 | 초기값 | 스토리지 | Compression | 통계수집량 | 설명
--------+---------+-----------+----------+--------+----------+-------------+------------+------
 id     | integer |           |          |        | plain    |             |            |
 name   | text    |           |          |        | extended |             |            |
 age    | numeric |           |          |        | main     |             |            |
파티션 키: RANGE (age)
파티션들: cust_medium FOR VALUES FROM ('25') TO ('75'),
          cust_old FOR VALUES FROM ('75') TO (MAXVALUE),
          cust_young FOR VALUES FROM (MINVALUE) TO ('25')


temp=# insert into customers values (1, 'Bob', 20),(2,'Alice',20), (3,'Doe',38), (4,'Richard',80);
INSERT 0 4
temp=# select * from customers;
 id |  name   | age
----+---------+-----
  1 | Bob     |  20
  2 | Alice   |  20
  3 | Doe     |  38
  4 | Richard |  80
(4개 행)

// 어느 파티션에서 왔는지 확인 가능 
temp=# select tableoid::regclass, * from customers;
  tableoid   | id |  name   | age
-------------+----+---------+-----
 cust_young  |  1 | Bob     |  20
 cust_young  |  2 | Alice   |  20
 cust_medium |  3 | Doe     |  38
 cust_old    |  4 | Richard |  80
(4개 행)
```

### info

-- 리눅스에서 postgresql access
sudo -i -u postgres
psql -U postgres -h [address]

psql -d <데이터베이스> -f <sql 파일명>
