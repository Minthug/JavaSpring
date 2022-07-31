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
