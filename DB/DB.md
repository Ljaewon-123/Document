# error

```
QueryFailedError: duplicate key value violates unique constraint "PK_bcead71157213380ddf6dcf35fa"
```

```
detail: 'Key (seq_id)=(1121) already exists.',
```

현재 시퀸스는 이미 5000번대인데 

이미 있는 예전 키가 이미 있다고 에러가 난다 

이게 현재 시퀸스 넘버랑 다음 시퀸스가 맞지 않아서 생긴 문제 인거 같다

```
https://velog.io/@kwakwoohyun/%EC%9D%B4%EC%8A%88%EC%B2%98%EB%A6%AC-ERROR-duplicate-key-value-violates-unique-constraint
```

그리고 지금

테이블이 시퀸스가 1씩 증가하고있지 않은 테이블이 있다

다른건다 1씩 증가하는데 혼자 지멋대로다 

insert 에러로 실패일수도 있긴한데 1분마다 stream데이터를 저장하는데 저장은 잘되는 

error문제는 아닌거 같은데 

cache문제 같기도 하다 

# other

// .andWhere("date_part('minute',plc_date)=0")

.select('extract(hour from plc_date) AS "hour"')
