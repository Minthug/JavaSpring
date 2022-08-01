![](https://velog.velcdn.com/images/minthug94_/post/ceed8154-06ff-484e-9f1a-4f6e0316a25f/image.png)

OAuth 과제 실습을 진행하는 도중 해당 오류가 계속 잡혀서 이렇게 포스팅을 
한다.

인텔리제이에서 Gradle 프로젝트 첫 실행할 때 발생하는 오류라고는 하는데,
어느 순간 뜬금없이 오류가 생겨버려 매우 당황스럽게 문제 해결을 하려고 
노력중이다.

구글에서 찾은 해결 방법은
1. [ File > Setting ] 메뉴 클릭 ( Mac 기준 Commnd + , )
2. [Build, Execution, Deployment > Build Tools > Gradle] 메뉴 이동
3. Build and run using 을 IntelliJ IDEA로 변경
4. Run tests using 을 IntelliJ IDEA로 변경
5. Gradle JVM을 jdk11로 변경 

![](https://velog.velcdn.com/images/minthug94_/post/a4f6d93e-540c-44b2-9e5b-8dde3255212b/image.png)

![](https://velog.velcdn.com/images/minthug94_/post/67259c0d-cf49-4442-b98f-4ca147202f2f/image.png)

일단 오류 자체는 없앴지만, 내가 잘못 이해를 하고 있는건지 localhost를 
사용할수 없게되어버렸다...

금요일엔 괜찮았는데 ?!

조금 더 해결해보자 한 스텝은 넘겼으니깐
--------------------

하루 지나고 다시 코드를 처음부터 진행하는 도중 어디서 문제가 생긴지 
확인했다.
![](https://velog.velcdn.com/images/minthug94_/post/2f906b9c-7bc1-49a4-8d79-166542208606/image.png)

패스워드 암호화하는 과정에 딱 처음 봤던 오류가 그대로 발생했다
일단 배제하고 결과를 본 후 다시 문제를 해결해야겠다

![](https://velog.velcdn.com/images/minthug94_/post/23fed6c1-7274-4d05-b224-997b3b2d6212/image.png)

Edit Configurations 를 통해 Build and run 을 자바 11 JDK로 바꿔 진행을 
했지만,
여전히 오류를 발생시켰다.

![](https://velog.velcdn.com/images/minthug94_/post/cfd33a47-ce3b-4816-aa68-1c399491572c/image.png)


![](https://velog.velcdn.com/images/minthug94_/post/92110990-4a24-4968-8053-a7902c043386/image.png)

같이 오늘 공부한 페어님께서 이 방법은 어떠한지 제안을 해주셔서 
해결해버렸다 ! 너무 기쁘다!!

sourceCompatibility = '11' 이 왜인지 모르겠지만 인식을 하지않아 오류를 
계속 발생시켰다
그래서 해결방법은 targetCompatibility = '11' 을 추가해 인식하게 만드니

약 이틀 간 날 괴롭힌 문제를 해결했다!

