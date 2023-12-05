# Thoughts on Nodejs



가정 만약 백엔드 API 서버용 코드를 작성하고 해당 코드를 4GB메모리에 더블코어 시스템에 배포한다고 하면 인터넷을통해 트래픽이 수신되기 시작함 이것은 nodejs서버가 되는

nodejs를 사용했다함은 싱글스레드라는 뜻이며 아래와 같이  단 하나의 코어만 사용하여 리소스자원의 낭비가 있다는 뜻이 된다 

![](C:\Users\jaewon\AppData\Roaming\marktext\images\2023-12-04-13-42-29-image.png)

그러므로 일반적으로 아래와 같이 여러 노드 또는 프로세스 관리자들을 사용하게 된다 

nodejs는 서로 다른 포트에서 실행되는 서버를 처리하며 이를 위해 nginx같은 로드밸런서를 사용하기도하고 프로세스관리자인 PM2 나 컨테이너 관리자인 쿠버네티스, 도커 또는 아마존이나 heroku azure같은 클라우드 시스템을 사용

![](C:\Users\jaewon\AppData\Roaming\marktext\images\2023-12-04-13-40-42-image.png)



하지만 js와는 다르게 서버용과 가까운 언어를 사용해서 서버를 구성하면 멀티스레딩이 자연스럽게 따라온다는 말이다

![](C:\Users\jaewon\AppData\Roaming\marktext\images\2023-12-04-13-46-17-image.png)

스레드도 가볍고 프로세스는 더 빠르게 실행되고 더 적은 메모리를 차지한다



따라서 node서버가 이를 따라 갈려면 여러 프로세스를 실행시켜야 한다

하지만 js는 비동기를 지원하고 node또한 당연히 비동기를 지원한다 

비동기 런타임이 있을때 멀티스레딩을 사용하려는 이유가 필요한대 

동시요청을 이미 처리하고 있으므로 멀티스레드가 필요한 이유는 무엇?



[Node.js is a serious thing now… (2023) - YouTube](https://www.youtube.com/watch?v=_Im4_3Z1NxQ)

2분정도 까지의 내용인데 그거에 대해 반박하는 글들이 흥미로워서 정리할겸 적는중이다



> i think there is some confusion as to the actual meaning of async. Async does not really have anything to do with threads. They are closely related but fundamentally separate. You can have async execution within a single thread, sometimes this is even more performant than using a thread pool. Async execution just means that the event loop can be passed off to other tasks



... 때로는 thread pool을 사용하는 것보다 성능이 훨씬 더 좋습니다 



흠 조금 안심되던 말이다




