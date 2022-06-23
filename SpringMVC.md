# Spring MVC란?
클라이언트의 요청을 편리하게 처리해주는 프레임워크

<details>
<summary>
Servlet(서블릿)이란?</summary>
  
서블릿은 클라이언트의 요청을 처리하도록 특정 규약에 맞춰 자바 코드로 
작성하는 클래스 파일
그리고 아파치 톰캣은 이런 서블릿들이 웹 애플리케이션으로 실행이 되도록 
해주는 서블릿 컨테이저 중 하나이다

</details>

### Model
Spring 'M' VC
MVC 기반의 웹 어플리케이션이 클라이언트의 요청을 받으면 요청 사항을 
처리하기 위한 작업
이렇게 처리한 작업의 결과 데이터를 클라이언트에게 응답으로 돌려줘야하는데, 
이 때 클라이언트에게 응답으로 돌려주는 작업의 처리 결과를 Model이라 한다

>📢 클라이언트의 요청 사항을 구체적으로 처리하는 영역을 **Service 
Layer(서비스 계층)** 라 하며,
실제로 요청 사항을 처리하기 위해 자바 코드로 구현한 것을 **Business 
Logic(비즈니스 로직)** 이라 한다.

### View
Spring M'V'C
View는 Model 데이터를 이용해서 웹브라우저 같은 클라이언트 애플리케이션의 
화면에 보이는 리소스를 제공하는 역할

### 다양한 View 형태
- HTML 페이지 출력
   - 클라이언트 애플리케이션에 보여지는 HTML 페이지를 직접 렌더링해서 
클라이언트 측에 전송하는 방법
   - 기본적은 HTML 태그로 구성된 페이지 Model 데이터를 채워 넣은 후, 
최종적인 HTML 페이지를 만들어 클라이언트측에 전송해준다
   - Spring MVC에서 지원하는 HTML 페이지 출력 기술에는 Thymeleaf, 
FreeMarker, JSP + JSTL, Tiles 등이 있다.
- PDF, Excel 등의 문서 형태로 출력
   - Model 데이터를 가공해서 PDF 문서나 Excel 문서를 만들어서 클라이언트 
측에 전송하는 방법
   - 문서 내에서 데이터가 동적으로 변경되어야 하는 경우 사용할 수 있는 
방식
- XML, JSON 등 특정 형식의 포맷으로 변환
   - Model 데이터를 특정 프로토콜 형태로 변환해서 변환된 데이터를 
클라이언트 측에 전송하는 방법
   - 이 방식은 특정 형식의 데이터만 전송하고, 프론트엔드 측에서 이 
데이터를 기반으로 HTML 페이지를 만드는 방식이다
   
   - ### 장점
   프런트엔드 영역과 백엔드 영역이 명확하게 구분되므로 개발 및 유지보수가 
상대적으로 용이하다
   프런트엔드 측에서 비동기 클라이언트 애플리케이션을 만드는 것이 
가능해진다

### Controller
Spring MV'C'
Controller는 클라이언트 측의 요청을 직접적으로 전달 받는 엔드포인트로써 
Model과 View의 중간에서 상호 작용을 해주는 역할을 한다

## MVC 간의 처리 흐름 정리
Client가 요청 데이터 전송
➡️ 컨트롤러가 요청 데이터 수신 ➡️ 비즈니스 로직 처리 ➡️ 모델 데이터 생성
➡️ 컨트롤러에게 모델 데이터 전달 ➡️ 컨트롤러가 뷰에게 모델 데이터 전달
➡️ 뷰가 응답 데이터 생성

# MVC 동작 방식과 구성요소
![](https://velog.velcdn.com/images/minthug94_/post/75356762-9590-407b-887d-739cbc64b974/image.png)

흐름대로 따라가보자

1. 클라이언트가 요청을 전송하면 DispatcherServlet 이라는 클래스로 요청이 
전달된다.

2. DispatcherServlet은 클라이언트의 요청을 처리할 Controller에 대한 검색을 
HandlerMapping 인터페이스에 요청한다

3. HandlerMapping은 클라이언트 요청과 매핑되는 컨트롤러 정보를 다시 
DispatcherServlet에게 리턴

<details>
<summary>
Click me</summary>
  
컨트롤러 정보에는 해당 컨트롤러 안에 있는 Handler 메서드 정보를 포함하고 
있다.
Handler 메서드는 컨트롤러 클래스 안에 구현된 요청 처리 메서드를 뜻한다
</details>

4. 요청을 처리할 컨트롤러 클래스를 찾았으니 이제 실제로 클라이언트 요청을 
처리할 Handler 메서드를 찾아서 호출해야 한다
DispatcherServlet 은 직접 Handler 메서드를 호출하지 않고, 
HandlerAdater에게 Handler 호출을 위임한다

5. HanlderAdapter는 DispatcherServlet 으로 부터 전달 받은 컨트롤러 정보를 
기반으로 해당 컨트롤러의 Handler 메서드를 호출

6. 컨트롤러의 Handler 메서드는 비즈니스 로직 처리 후 리턴 받은 모델 
데이터를 HandlerAdapter에게 전달

7. HandlerAdapter 는 전달 받은 모델 데이터와 View 정보를 다시 
DispatcherServlet에게 전달

8. DispatcherServlet는 전달 받은 View 정보를 다시 ViewResolver에게 
전달해서 뷰 검색을 요청

9. ViewResolver는 뷰 정보에 해당하는 뷰를 찾아 뷰를 DispatcherServlet에게 
다시 리턴

10. DispatcherServlet는 ViewResolver로 부터 전달 받은 뷰 객체를 통해 모델 
데이터를 넘겨주면서 클라이언트에게 전달할 응답 데이터 생성을 요청

11. 뷰는 응답 데이터를 생성해서 다시 DispatcherServlet에게 전달

12. DispatcherServlet는 뷰로부터 전달 받은 응답 데이터를 최종적으로 
클라이언트에게 전달

<details>
<summary>
DispatcherServlet</summary>
  
DispatcherServlet 은 굉장히 많은 일을 하는 것처럼 보인다.
이렇게 굉장히 많은 일을 처리하는거 같지만 실제로 요청에 대한 처리는 다른 
구성 요소들에게 위임(Delegate) 하고 있다.

"HandlerMapping아 Handler Controller 좀 찾아줄래 ?" -> ViewResolver야 View 
좀 찾아줄래? 라고 하는것처럼
  
이렇게 DispatcherServlet이 애플리케이션 가장 앞단에 배치되어 다른 
구성요소와 상호작용하면서 클라이언트의 요청을 처리하는 패턴을 Front 
Controller Pattern이라 한다
</details>

> Handler 용어의 의미
Spring MVC에서 Handler는 어떤것일까?
요청 Handler는 바로 개발자들이 작성하는 Controller 클래스를 뜻한다, 
컨트롤러 클래스에 있는 '@GetMapping, @PostMapping' 같은 애너테이션이 붙어 
있는 메서드들을 핸들러 메서드라 한다
#### HandlerMapping은 의미는 사용자의 요청과 이 요청을 처리하는 Handler를 
매핑해주는 역할이다
그렇다면, 사용자의 요청과 Handler의 메서드의 매핑은 어떤 기준으로 
이루어지나?
@GetMapping("/coffee")처럼 HTTP Request Method(GET,POST 등)와 Mapping 
URL을 기준으로 해당 Handler와 매핑이 되는데 Spring에선 여러가지 유형의 
HandlerMapping 클래스를 제공한다
##### 실무에선 Spring에서 디폴트로 정한 'RequestMappingHandlerMapping'을 
대부분 사용하는데 얼마든지 HandlerMapping 전략을 바꿀 수 있다고한다.

> ViewResolver 란?
ViewResolver는 DispatcherServlet에서 '이런 이름을 가진 View를 줘' 라고 
요청하면 DispatcherServlet에서 전달한 View 이름을 해석한 뒤 적절한 View 
객체를 리턴해주는 역할을 한다.


