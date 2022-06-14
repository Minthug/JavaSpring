# Java bean 이란?

<code>Spring IoC 컨테이너가 관리하는 자바 객체를 빈(bean)</code> 이라 
부른다
우리가 알던 기존의 자바 프로그래밍에선 class를 생성하고 new를 입력하여 
원하는 객체를 직접 생성한 후에 사용했었다
하지만 Spring 에선 직접 new 를 이용하여 생성한 객체가 아니라, Spring 
의하여 관리 당하는 자바 객체를 사용합니다.
이렇게 SPring에 의하여 생성되고 관리되는 자바 객체를 bean이라 한다

Spring Framework 에선 Spring Bean 을 얻기 위해 
ApplicationContext.getBean() 와 같은 메소드를 사용하여 Spring에서 직접 
자바 객체를 얻어서 사용한다

## Spring Bean을 Spring IoC Container에 등록하는 방법
### 자바 어노테이션(Java Annotation)을 사용하는 방법

Java에선 <code>Annotation</code> 이라는 기능이 있다.
사전상의 의미론 주석의 의미이지만 Java에선 주석 이상의 기능을 가지고 있다.
Annotation은 자바 소스 코드에 추가하여 사용할 수 있는 메타 데이터의 
일종이다.
소스코드에 추가하면 단순 주석 기능을 하는것이 아닌 특별한 기능을 사용할 수 
있다.

자바에선 @Override, @Deprecated 와 같은 기본적인 어노테이션을 제공한다

```java
public class Parent {
	Public void doSomething() {
    System.out.println("This is Parent");
    }
  }
public class Son extends Parent {
	@Override
    public void doSomething() {
    System.out.println("This is Son");
    }
  }  
```

스프링에선 여러 가지 어노테이션을 사용하지만, Bean을 등록하기 위해선 
@Component 어노테이션을 사용한다
@Component 어노테이션이 등록되어 있는 경우엔 스프링이 어노테이션을 
확인하고 자체적으로 빈으로 등록한다

실제 예시
```java
//HelloController.java
@Controller
public class HelloController {
 // Http Get 메소드의 /hello 경로로 요청이 들어올 때 처리할 메소드를 
아래와 같이 @GetMapping 어노테이션을 사용해 Mapping을 사용할 수 있다.
 @GetMapping("hello")
 public String hello (Model model) {
 	model.addAttribute("data","This is data!");
    return "hello";
    }
  }  
```

@Controller 어노테이션을 IntelliJ에서 커맨드를 눌러서 이동해보면 아래와 
같은 소스를 확인 할 수 있다.
@Controller 어노테이션에는 @Conponent 어노테이션이 있는 것을 확인할 수 
있다.
@Component 어노테이션으로 인해 스프링은 해당 컨트롤러를 빈으로 등록한다

```java
// Controller.java

// -- 일부 생략 --
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Controller {

	/**
	 * The value may indicate a suggestion for a logical component 
name,
	 * to be turned into a Spring bean in case of an autodetected 
component.
	 * @return the suggested component name, if any (or empty String 
otherwise)
	 */
	@AliasFor(annotation = Component.class)
	String value() default "";

}
```

## Bean Configuration File에 직접 빈 등록하는 방법
@Configuration 과 @Bean 어노테이션을 이용하여 빈에 등록할 수 있다.

```java
// Hello.java
@Configuration
public class HelloConfiguration {
	@Bean
    public HelloController sampleController() {
    return new SampleController;
    }
  }  
```
@Configuration 을 이용하면 Spring Project에서의 Configuration 역할을 하는 
class를 지정할 수 있다.

해당 File 하위에 Bean 으로 등록하고자 하는 class @Bean 어노테이션을 
사용해주면 간단하게 빈으로 등록할 수 있다.

-----------
참고 url
https://melonicedlatte.com/2021/07/11/232800.html


