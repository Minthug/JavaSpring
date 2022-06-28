# 예외 처리 과정
- 메서드 내에서 예외 상황을 예측해서 처리하는 try-catch문을 이용하는 방법
- 요구사항에 의한 예외 처리(ex.Validation > 특정 값 0~255 범위가 아니면 
유효하지 않은 값으로 판단하고 예외 처리)
- 스프링 security에서 인터셉터로 잡아서 UnauthorizedException 같은 예외 
처리
등 기타 여러 예외 처리들을 적용하다보면 코드가 매우 복잡해진다
그것은 결국 유지 보수하기에 어려움을 겪는 상황을 발생시킬것이다.
또한 비즈니스 로직에 집중하기가 어렵고, 비즈니스 로직과 관련된 코드보다 
예외 처리를 위한 코드가 더 많아지는 경우가 생긴다.

이런 문제들을 개선하기 위해서 @ExceptionHandler && @ControllerAdvice 등을 
사용한다고 보면 이 과정을 이해하기 쉬울 것이다.

# @ExceptionHandler를 이용한 예외 처리
@ExceptionHandler 같은 경우 @Controller, @RestController가 적용된 Bean 
내에서 발생하는 예외를 잡아서 하나의 메서드에서 처리해주는 기능을 한다.

```java
@RestController
public class MyRestController {
	...
    ...
    
    @ExceptionHandler(NullPointException.class)
    public Object nullex(Exception e) {
    System.out.println(e.getClass());
    return "myService";
    }
 }   
```
위 처럼 적용하기만 하면된다. @ExceptionHanlder annotation을 쓰고 인자로 
캐치하고 싶은 예외 클래스를 등록해주면 된다.

> 📢 @ExceptionHandler({Exception1.class, Exception2.class}) 이런 식으로 
두 개 이상도 등록이 가능하다
위 예제처럼 하면 MyRestController에 해당하는 Bean에서 
NullPointerException이 발생하면, 
@ExceptionHandler(NullPointerException.class)가 적용된 메서드가 호출 
될것이다.

### 주의사항/ 알아둘 내용
- Controller, RestController에만 적용이 가능하다(@Service 같은 Bean에선 
안됨)
- 리턴 타입은 자유롭게 해도 된다. (Controller 내부 메서드들은 여러 타입의 
response를 할것, 해당 타입과 전혀 다른 리턴 타입이어도 상관없다)
- @ExceptionHandler를 등록한 Controller에만 적용
다른 Controller에서 NullPointerException이 발생하더라도 예외를 처리할 수 
없다.
- 메서드의 파라미터로 Exception을 받았지만 이것 또한 자유롭게 받아올 수 
있다.

# @ControllerAdvice
@ExceptionHandler가 하나의 클래스에 대한 것이라면, @ControllerAdvice 는 
모든 @Controller 즉, 전역에서 발생하는 예외를 잡아 처리해주는 
어노테이션이다.
```java
@RestControllerAdvice
public class Myadvice {
	@ExceptionHandler(CustomException.class)
    public String custom() {
    	return "hello custom";
        }
    }    
```
위 처럼 새로운 클래스 파일을 만들어서 어노테이션을 붙이면 된다.
그 다음엔 @ExceptionHandler로 처리하고 싶은 예외를 잡아 처리한다

별도의 속성값이 없어 사용하면 모든 패키지 전역에 있는 컨트롤러를 담당하게 
된다.

@RestControllerAdvice && @ControllerAdvice가 존재하는데 
@RestControllerAdvice 어노테이션을 확인해보면
```
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@ControllerAdvice
@ResponseBody
public @interface RestControllerAdvice {
	//...
}
```
@ControllerAdvice과 동일한 역할
즉, 예외를 잡아 핸들링 할 수 있도록 기능을 수행하면서 @ResponseBody를 통해 
객체를 리턴할 수 도있다는 뜻이다.
ViewResolver를 통해서 예외 처리 페이지로 리다이렉트 시키면 
@ControllerAdvice만 써도 되고, API 서버에서 에러 응답으로 객체를 
리턴해야한다면 @ResponseBody 어노테이션이 추가된 @RestControllerAdvice 
적용하면 되는 것이다

@RestController에서 예외가 발생하든 @Controller에서 예외가 발생하든 
@ControllerAdvice && @ExceptionHandler 2개의 어노테이션으로 다 캐치할 수 
있고, @ResponseBody의 필요 여부에 따라 적용하면 된다는 것이다.

만약 전역의 예외를 잡긴하되 패키지 단위로도 제한할 수 있다.
```
@RestControllerAdvice("com.example.demo.login.controller")
```
login 모듈에 있는 RestController에서 발생하는 예외를 잡으려면 이렇게 
하면된다.


--------------------------
참조 URL
https://jeong-pro.tistory.com/195

