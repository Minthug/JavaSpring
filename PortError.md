## Web server failed to start. Port 8080 was already in use.

할때마다 새로운 상황을 만나니 신기하다 늘

[에러내용]
스프링부트 프로젝트 생성 후 복습 겸 다시 설정하고 진행하는 도중

![](https://velog.velcdn.com/images/minthug94_/post/1709faed-59ed-4281-9bc0-57b353fd003a/image.png)

이러한 에러를 만나버렸다

아마 기존의 진행했던 프로젝트에서 포트 8080을 사용하다가 따로 Disconnect을 
하지않아서인지,
해당 오류를 만나버렸다

원래는 본래 프로젝트에 가서 종료하면 되지만 어째서인지 따로 프로젝트가 
실행되고있진않아서

다른 해결 방법을 찾아 적용했다

## application.properties 포트 설정 변경

 application.properties 에서 사용포트를 변경해주는 방법이다
 
![](https://velog.velcdn.com/images/minthug94_/post/380be342-5fc4-430b-88ec-18cdccaa5d3d/image.png)
 application.properties 파일 내부에서 이런식으로 작성만해도 사용포트를 변경할 수 
있는데
 굉장히 편하고 쉽게 해결했다
 
 ## 터미널내에서 포트 죽이기
 터미널에 접속 후,
 'sudo lsof -i :[확인하고싶은 포트번호]'를 입력하여 실행중인 포트를 확인해보자
 
![](https://velog.velcdn.com/images/minthug94_/post/11dfef3b-6f30-4076-b7af-0aeeb43a77ed/image.png)
이렇게 작성하면 이렇게 화면이 나타나고,
'sudo kill -9 [실행중인 포트의PID]' 를 입력하여 실행중인 포트를 중지시킨다

