## AOP가 필요한 이유
- 핵심 기능 (Core Concerns) : 업무 로직을 포함하는 기능
- 부가 기능 (CROSS-CUTTING CONCERNS) : 핵심 기능을 도와주는 부가적인 기능
   -  로깅, 보안, 트랜잭션 등이 있다.
- Aspect : 부가 기능을 정의한 코드인 어드바이스와 어드바이스를 어디에 적용할지 
결정하는 포인트컷(PointCut)을 합친 개념
<code>(Adivce + PointCut => Aspect)</code>



# 객체 지향 프로그래밍 (OOP)

- OOP의 핵심은 공통된 목적을 띈 데이터와 동작을 묶어 하나의 객체로 정의하는 것
- 객체를 적극적으로 활용함으로써 시능을 재사용할 수 있는 것이 큰 장점
- 객체를 잘 활용하기 위해선 관심사 분리(Separation of Concerns, SoC)의 디자인 
원칙을 준수해야 한다

> 관심사의 분리는 모듈화의 핵심이다
Spring MVC 구조는 @Controller, @Service, @Repository 와 강ㅌ이 관심사 별로 
계층을 나눠 객체를 관리

### 문제점
- 특정 관심사 업무 코드에 트랜잭션, 보안, 로깅 등의 코드가 존재
- 트랜잭션, 보안, 로깅 코드는 업무와는 관련없지만 애플리케이션에 필수적인 부가 
기능이다
- 트랜잭션, 보안, 로깅 기능은 불특정 다수의 클래스에서 존재하게 된다
- 관심사 관점에선 트랜잭션, 보안, 로깅 코드들을 횡단 관심사 (Cross-cutting 
Concerns, 부가기능)이라 한다
- 업무 관련 코드는 핵심 관심사 (Core Concerns, 핵심 기능) 이라 한다
- 비즈니스 클래스에 횡단 관심사와 핵심 관리사가 공존하게 된다
   - 메소드 복잡도 증가 => 비즈니스 코드 파악이 어려워짐
   - 부가 기능의 불특정 다수 메소드가 반복적으로 구현 => 횡단 관심사 모듈화가 
어렵다

# AOP란?
AOP(Aspect-Oriented Programming)는 기존과 드란 프로그램 구조 사고 방식을 
제공함으로써 객체 지향 프로그래밍의 부족한 ㅜ뿐을 보완해준다

OOP의 모듈화의 핵심 단위는 클래스이고, AOP의 모듈화의 핵심 단위는 관점이다
Aspect는 여러 유형과 객체 간에 발생하는 문제 (예)트랜젝션)의 모듈화를 가능하게 
한다

자체적인 언어라기보단 기존의 OOP를 보완하는 확장 형태로 사용되고 있다.


#### 자바진영에서 사용되는 AOP 도구
- AspectJ
- JBossAOP
- SpringAOP

![](https://velog.velcdn.com/images/minthug94_/post/8c9f23f9-bbe5-4c6b-89f0-f90aee911b61/image.png)

## AOP의 핵심 기능과 부가 기능
- 핵심 기능(Core Concerns)
객체가 제공하는 고유의 기능(업무 로직 등을 포함)
- 부가 기능(CROSS-CUTTING CONCERNS)
핵심 기능을 보조하기 위해 제공되는 기능
로그 추적 로직, 보안, 트랜젝션 기능 등이 있다
단독적으로 사용되지 않으며 핵심 기능과 함께 사용한다

![](https://velog.velcdn.com/images/minthug94_/post/dadf2cd2-1625-45f5-8478-857b52044260/image.png)
핵심 기능 helloController, memberSerivce, memberRepository 등
부가 기능 : 시간 측정 로직

## 여러 곳에서 공통으로 사용하는 부가 기능
부가 기능은 보통 여러 클래스에 걸쳐서 함께 사용된다
이런 부가 기능은 횡단 관심사라 한다

![](https://velog.velcdn.com/images/minthug94_/post/186f674d-0de3-4117-be32-c4b59efcf3af/image.png)


- 부가 기능을 여러 곳에 적용하려면 번거롭고 중복 코드가 발생한다
- 부가 기능에 수정이 필요하게 되면 사용되는 클래스에 모두 하나씩 찾아가며 
수정해야 한다.

## AOP 존재 이유
소프트웨어 개발에서 변경 지점은 하나가 될 수 있도록 잘 모듈화 되어야 한다, 
부가 기능처럼 특정 로직을 애플리케이션 전반에 적용하는 문제는 일반적인 OOP 
방식으로는 어렵기 때문에 핵심 기능과 부가 기능을 분리하는 AOP 방식이 필요하다.

# AOP 용어

- ## Aspect
   여러 객체에 공통으로 적용되는 기능을 말한다(공통 기능)
   어드바이스 + 포인트컷을 모듈화해서 애플리케이션에 포함되는 횡단 기능이다
   여러 어드바이스와 포인트컷이 함께 존재한다
   
- ## Join Point
   클래스 초기화, 객체 인스턴스화, 메소드 호출, 필드 접근, 예외 발생과 같은 
애플리케이션 실행 흐름에서의 특정 포인트를 의미한다
   애플리케이션에 새로운 동작을 추가하기 위해 조인포인트에 <code>관심 
코드(aspect code)</code>를 추가할 수 있다
   횡단 관심은 조인포인트 전/후에 AOP에 의해 자동으로 추가된다
   추상적인 개념이고 AOP를 적용할 수 있는 지점을 의미한다
   스프링 AOP는 프록시 방식을 사용하므로 조인 포인트는 항상 메소드 실행 
지점으로 제한 된다
   어드바이스 적용이 필요한 곳은 어플리케이션 내에 메서드를 갖는다
   
   -  어드바이스가 적용될 수 있는 위치, 메소드 실행, 생성자 호출, 필드 값 
접근, static 메서드 접근 같은 프로그램 실행 중 지점을 나타낸다
   - AspectJ를 사용해 컴파일 시점과 클래스 로딩 시점에 적용하는 AOP는 
바이트코드를 실제 조작하기 때문에 해당 기능을 모든 지점에 다 적용할 수 있다
   - 프록시 방식을 사용하는 스프링 AOP는 메서드 실행 지점에만 AOP를 적용할 수 
있다.
   - 프록시는 메서드 오버라이딩 개념으로 동작한다
   - 생성자나 static 메서드, 필드 값 접근엔 프록시 개념이 적용될 수 없다.
   - 프록시를 사용하는 스프링 AOP의 조인 포인트는 메서드 실행으로 제한된다
   - 프록시 방식을 사용하는 스프링 AOP는 스프링 컨테이너가 관리할 수 있는 
스프링 빈에만 AOP를 적용할 수 있다
   - JoinPoint 메서드는 어드바이스 종류에 따라 사용방법이 다소 다르지마 
기본적으로 어드바이스 메서드에 매개변수로 선언만 하면 된다.
   
![](https://velog.velcdn.com/images/minthug94_/post/fdaecc3d-d399-41d8-a22c-30f507424e92/image.png)

   
   
![](https://velog.velcdn.com/images/minthug94_/post/ff9fd1cb-efa8-4c05-93af-70bd39242445/image.png)

(S는 메서드 실행 전의 포인트, E는 메서드 수행 후의 포인트 이런 포인트를 
어드바이스 적용될 조인 포인트라고 한다)

---------
- ## Advice
조인포인트에서 수행되는 코드를 의미한다
Aspect를 언제 핵심 코드에 적용할지 정의한다
시스템 전체 Aspect에 API 호출을 제공한다
위 예시처럼 메소드를 홀출하기 전에 상세 정보와 모든 메서드를 로그로 남기기 
위해 메서드 시작 전의 포인트인 조인포인트S를 선택한다
부가 기능에 해당된다

Advice는 기본적으로 순서를 보장하지 않는다
- 순서를 지정하고 싶으면 @Aspect 적용 단위로 
org.springframework.core.annotation.@Order 애너테이션을 적용해야 한다
   -  어드바이스 단위가 아니라 클래스 단위로 적용할 수 있다.
   - 하나의 Aspect에 여러 어드바이스가 존재하면 순서를 보장 받지 못한다.
- Aspect를 별도의 클래스로 분리해야한다

### Advice 종류

### Before
- 조인 포인트 실행 이전에 실행한다
- 타겟 메서드가 실행되기 전에 처리해야할 필요가 있는 부가 기능을 호출 전에 
공통 기능을 실행한다
- Before Advice 구현한 메서드는 일반적으로 리턴 타입이 void이다
(리턴값을 갖더라도 실제 Advice 적용 과정에 아무 영향이 없다)
- 주의점으로 메서드에서 예외를 발생시킬 경우 대상 객체의 메서드가 호출되지 
않게 된다는 점이 있다
```java
@Before("hello.aop.order.Pointcuts.orderAndService()")
public void doBefore(JoinPoint joinPoint) {
	log.info("[before]{}", joinPoint.getSignature());
    }
```
- 작업 흐름을 변경할 수 없다.
- 메서드 종료 시 자동으로 다음 타겟이 호출된다 (예외 발생 시 다음 코드는 
호출되지 않는다)

### After returning
- 조인 포인트가 정상 완료 후 실행
- 메서드가 예외 없이 실행된 이후에 공통 기능을 실행
```java
@AfterReturning(value = "hello.aop.order.aop.Pointcuts.orderAndService()". 
returning = "result")
public void doReturn(JoinPoint joinPoint, Object result) {
	log.info("[return] {} return ={}", joinPoint.getSignature(), result);
    }
```
- 메서드 실행이 정상적으로 반활될 때 실행된다
- returning 속성에 사용된 이름은 어드바이스 메서드의 매개변수 이름과 일치해야 
한다
- returning 절에 지정된 타입의 값을 반환하는 메서드만 대상을 실행한다

### After throwing
- 메서드가 예외를 던지는 경우에 실행
- 메서드를 실행하는 도중 예외가 발생한 경우 공통 기능을 실행
```java
@AfterThrowing(value = "hello.aop.order.aop.Pointcuts.orderAndService()", 
throwing = "ex")
public void doThrowing(JoinPoint joinPoint, Exception ex) {
	log.info("[ex] {} message={}", joinPoint.getSignature(), 
ex.getMessage());
    }
```
- 메서드 실행이 예외를 던져서 종료될 때 실행한다.
- throwing 속성에 사용된 이름은 어드바이스 메서드의 매개변수 이름과 일치해야 
한다
- throwing 절에 지정된 타입과 맞은 예외를 대상으로 실행한다

### After(finally)
- 조인 포인트의 동작(정상 || 예외)과는 상관없이 실행(예외 동작의 finally를 
생각하면 된다)
- 메서드 실행 후 공통 기능을 실행
- 일반적으로 리소르를 해제하는데 사용한다

### Around
- 메서드 호출 전후에 수행하여 가장 강력한 어드바이스이다(조인 포인트 실행 여부 
선택, 반환 값 변환, 예외 변환 등이 가능)
- 메서드 실행 전 & 후, 예외 발생 시점에 공통 기능을 실행한다
- 가장 강력한 어드바이스이다
    - 조인 포인트 실행 여부 선택 - joinPoint.proceed()
    - 전달 값 변환 - joinPoint.proceed(args[])
    - 반환 값 변환
    - 예외 변환
    - <code>try ~ catch ~ finally </code>가 들어가는 구문 처리 가능
- 어드바이스의 첫 번째 파라미터는 ProceedingJoinPoint를 사용해야 한다
- proceed()를 통해 대상을 실행
- proceed()를 여러번 실행할 수 있다.

@Around만 있어도 모든 기능 수행이 가능하다
가장 강력한 어드바이스이며 대부분의 기능을 제공하지만 타겟 등 고려할 사항이 
있을땐 정상적으로 작동이 안되는 경우도 있다.
- @Before, @After와 같은 어드바이스는 기능은 적지만 원하는대로 작동되고 코드도 
단순하다
- 각 애너테이션만 봐도 타겟 실행 전에 어떤 일을 하는지 명확하게 알 수 있다.
- 좋은 설계는 @Around만 사용해서 모두 해결하는 것보단 제약을 가지더라도 실수를 
사전에 방지하는 것
- 제약을 두면 문제 자체가 발생하지 않게 되며, 역할이 명확해진다

----------------

- ## Pointcut
조인포인트 중에 어드바이스가 적용될 위치를 선별하는 기능, AspectJ 표현식을 
사용해 지정
프록시를 사용하는 스프링 AOP는 메서드 실행 지점만 포인트컷으로 선별 가능하다

![](https://velog.velcdn.com/images/minthug94_/post/ef93b3ae-80d1-463e-b191-074d3ca2045a/image.png)

### Pointcut 표현식
포인트컷과 표현식 & 지시자
포인트컷은 관심 조인 포인트를 결정하므로 어드바이스가 실행되는 시기를 제어할 
수 있다.
>AspectJ는 포인트컷을 편리하게 표현하기 위한 특별한 표현식을 제공
<code>@Pointcut("execution(* hello.aop.order..*(..))")</code>

```java
@Pointcut("execution(* transfer(..))") //포인트컷 표현식
private void anyOldTransfer(){} // 포인트컷 서명
```

![](https://velog.velcdn.com/images/minthug94_/post/7c2407aa-dc7b-4e38-8f10-94117e22eff2/image.png)

포인트컷 표현식은 AspectJ pointcut expression => AspectJ가 제공하는 포인트컷을 
줄여서 표현하는것

### 포인트컷 지시자 
execution 같은 포인트컷 지시자(Pointcut Designator, PCD)로 시작

![](https://velog.velcdn.com/images/minthug94_/post/d4ae7a6c-ffff-4f30-a113-a9bb7bd7d4f3/image.png)

### Pointcut 표현식 결합
포인트컷 표현식은 &&, ||, ! 를 사용해 결합할 수 있다.
이름으로 pointcut 표현식을 참조할 수 있기도하다
```java
@Pointcut("execution(public * *(..))")
private void anyPublicOperation() {} // (1)

@Pointcut("within(com.xyz.myapp.trading..*)")
private void inTrading() {} // (2)

@Pointcut("anyPublicOperation() && inTrading()")
private void tradingOperation() {} // (3)
```
(1) anyPublicOperation은 메서드 실행 조인 포인트가 고용 메서드의 실행을 
나타내는 경우 일치한다
(2) inTrading 메서드 실행이 거래 모듈에 있는 경우에 일치한다
(3) tradingOperation 은 실행이 거래 모듈의 공개 메서드를 나타내는 경우 
일치한다

## 일반적인 pointcut 표현식들
![](https://velog.velcdn.com/images/minthug94_/post/92c672cf-3100-49a8-9790-8c51c5277b31/image.png)
![](https://velog.velcdn.com/images/minthug94_/post/43ef6d2b-a4d1-4dbf-9787-a100c6b4a2b7/image.png)
![](https://velog.velcdn.com/images/minthug94_/post/a076d96b-4a67-479e-900e-5d6ad03ce23f/image.png)
![](https://velog.velcdn.com/images/minthug94_/post/b2137fcb-5610-46e5-bdfb-f40a6cd16580/image.png)
![](https://velog.velcdn.com/images/minthug94_/post/f94f0355-a1df-4c0b-8b41-9442a8be15b2/image.png)

------------------------

- ## Weaving
포인트컷으로 결정한 타겟의 조인포인트에 어드바이스를 적용하는 것(Advice를 핵심 
코드에 적용하는것을 의미함)
핵심 기능 코드에 영향을 주지 않고 부가 기능을 추가할 수 잇다.
AOP 적용을 위해 Aspect 객체에 연결한 상태
    - 컴파일 타임(AspectJ compoiler)
    - 로드 타임
    - 런타임 스프링 AOP는 런타임, 프록시 방식이다

- ## AOP proxy
AOP 기능을 구현하기 위해 만든 프록시 객체이다
스프링에서 AOP 프록시는 JDK 동적 프록시 || CGLIB 프록시이다

- ## Target
핵심 기능을 담고 있는 모듈로 타겟은 부가기능을 부여할 대상이 된다
Advice를 받는 객체이고 포인트컷으로 결정

- ## Advisor
하나의 어드바이스와 하나의 포인트컷으로 구성 (스프링 AOP에서만 사용되는 특별한 
용어이다)


    
    
