# JWT with nestjs

대부분 jwt하면 access만 사용해서 넘기는데 refresh까지 사용하는 사용법은 검색 안되더라 
한번 해봤다.

보통 access를 발급해주면 최대15분정도 유효기간을 정해주고 해당 토큰이 있으면 pass아니면 reject를 한다 이후 access가 만료되면 

refresh를 서버에 저장해서 확인하고 refresh가 도난되면 서버에서 수동으로 삭제 
access만료시 서버에 새로고침토큰만 확인해서 작업하면 됨