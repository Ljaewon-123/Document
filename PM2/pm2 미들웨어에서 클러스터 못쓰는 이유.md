# pm2 미들웨어에서 클러스터 못쓰는 이유





![](C:\Users\jaewon\AppData\Roaming\marktext\images\2023-09-15-10-58-09-image.png)

nestjs microserve를 사용하는 중인데 

각각 메모리 공유x (전역변수간 프로세스 공유안됨)

독립적인 프로세스 확동 7개 프로세스 모두 하나의 mqtt 토픽을 구독해서 똑같은 데이터가 프로세스 수만큼 중복 저장됨

db 풀링이나 , 락 같은 방법도 있지만 너무 복잡해서 지양됨

![](C:\Users\jaewon\AppData\Roaming\marktext\images\2023-09-15-10-59-43-image.png)





## 번외로

api같은 경우 같은 nestjs지만 6개의 프로세스를 돌리지만 mqtt명령어는 단 한번씩만 날라갔다 

****어쩌면 미들웨어는 마이크로 서비스여서 그럴수도 있겠다는 생각이 강하게 든다****


