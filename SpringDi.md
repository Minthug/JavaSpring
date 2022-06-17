# Spring DI (Dependency Injection)

의존관계 주입 방법
- 생성자 주입
- 수정자 주입(setter 주입)
- 필드 주입
- 일반 메서드 주입

## 생성자 주입
생성자에 @Autowired를 하면 스프링 컨테이너에 @Component로 등록된 빈에서 
생성자에 필요한 빈들을 주입한다

### 특징
- 생성자 호출 시점에 딱 1번만 호출되는 것이 보장된다
- 불변과 필수 의존관계에 사용된다
- 생성자가 1개만 존재하는 경우엔 @Autowired 를 생략해도 자동 주입
- NullPointerException 을 방지할 수 있다
- 주입받을 필드를 final 로 선언 가능

```java
@Component
public class OrderServiceImpl implements OrderService {
	private final UserRepository userRepository;
 private final DiscountInfo discountInfo;
 
 	@Autowired
    public OrderServiceImpl(UserRepository userRepository, DiscountInfo 
discountInfo)
    this.userRepository = userRepository;
    this.discountInfo = discountInfo;
    }
  }
```

## 수정자 주입(Setter 주입)
setter라 불리는 필드의 값을 변경하는 수정자 메서드를 통해 의존 관계 
주입하는 법

### 특징
- 선택과 변경 가능성이 있는 의존 관계에 사용된다
- 자바빈 프로퍼티 규약의 수정자 메서드 방식을 사용하는 법

```java
@Component
public class OrderServiceImpl implements OrderService {
	private UserRepository userRepository;
  private DiscountInfo discountInfo;
  
  @Autowired
  public void setUserRepository(UserRepository userRepository) {
  	this.userRepository = userRepository;
  }
  
  @Autowired
  public void setDiscountInfo(DiscountInfo discountInfo) {
  	this.discountInfo = discountInfo;
    }
  }  
```
- 생성자 주입과 다르게 생성자 대신 set 필드명 메서드를 생성해 의존 
관계에 주입한다
- 수정자의 경우 @Autowired를 입력하지 않으면 실행되지 않는다
    - @Component가 실행하는 클래스를 스프링 빈으로 등록
    - 스프링 빈으로 등록한 다음 의존 관계를 주입하게 되는데 @Autowired 
있는 것들을 자동으로 주입하게 된다

#### 생성자가 1개일 때 @Autowired가 없어도 작동 되는 이유는 뭘까?
- 스프링이 해당 클래스 객체를 생성하여 빈에 넣어야하는데 생성할 때 
생성자를 부를수밖에 없기 때문이다.
- 그렇기 때문에 빈을 등록하면서 의존 관계 주입도 같이 발생

## 필드 주입
필드에 @Autowired 붙여서 바로 주입하는 법

### 특징
- 코드가 간결해서 예전에 많이 사용된 방식, 외부에서 변경이 불가능해 
테스트하기 힘들다는 점이 단점이다
- DI 프레임워크가 없으면 아무것도 못한다
- 실제 코드와 상관 없는 특정 테스트를 하고 싶을 때 사용할 수 있다.
- 정상적으로 작동되게 하려면 결국 setter가 필요하게 되어서 수정자 주입을 
사용하는게 더 편리하다

```java
@Service
public class OrderServiceImpl implements OrderService {

	@Autowired
    private UserRepository userRepository;
    @Autowired
    private DiscountInfo discountInfo;
    }
```

## 일반 메서드 주입
이름 그대로 일반 메서드를 사용해 주입하는 방법

### 특징
- 한번에 여러 필드를 주입 받을 수 있다.
- 일반적으로 사용되지 않는다.


# 옵션처리
주입할 스프링 빈이 없을 때 동작해야하는 경우가 있다.

- @Autowired만 사용하는 경우 required 옵션 기본값인 true가 사용되어 자동 
주입 대상이 없으면 오류가 발생하는 경우가 있다
- 스프링 빈을 옵셔널하게 해둔 상태에서 등록이 되지 않고, 기본 로직으로 
동작하게 하는 경우
- 자동 주입대상 옵션처리 방법
    - @Autowired(required=false) : 자동 주입할 대상이 없으면 수정자 
메서드 자체가 호출되지 않게 된다
    - org.springframework.lang.@Nullalbe : 자동 주입할 대상이 없으면 
null이 입력된다.
    - Optional<> : 자동 주입할 대상이 없으면 Optional.empty가 입력된다.

# 생성자 주입을 사용해야하는 이유
과거엔 수정자, 필드 주입을 많이 사용했지만, 최근엔 대부분 생성자 주입 
사용을 권장한다

## 사용 이유
- 불변
  - 의존 관계 주입은 처음 어플리케이션이 실행될 때 대부분 정해지고 종료 
전까지 변경되지 않고 변경되어선 안된다.
  - 수정자 주입 같은 경우엔 이름 메서드를 public으로 열어두어 변경이 
가능하기 때문에 적합하지 않다.
  - (누군가 실수로 변경할 수도 있고, 애초에 변경하면 안되는 메서드가 
변경할 수 있게 설계하는 것은 좋은 방법이 아니다.
  - 생성자 주입은 객체를 생성할 때 최초로 1번만 호출되고 이 후에는 
다시는 호출되는 일이 없기 떄문에 불변하게 설계할 수 있다.

- 누락
  - 호출했을 때 NPE(Null Point Exception)이 발생하는데 의존관계 주입이 
누락되었기 때문에 발생한다
  - 생성자 주입을 사용하면 주입 데이터 누락 시 컴파일 오류가 발생

- final 키워드 사용 가능
  - 생성자 주입을 사용하면 필드에 final 키워드를 사용할 수 있다.
  - 생성자에서 값이 설정되지 않으면 컴파일 시점에서 오류를 확인할 수 
있다.
  - java: variable (data name) might not have benn initialized
  - 생성자 주입을 제외한 나머지 주입 방식은 생성자 이후에 호출되는 
형태이므로 final 키워드를 사용할 수 없다

- 순환 참조
  - 순환 참조를 방지할 수 있다
  - 개발하다보면 여러 컴포넌트 간에 의존성이 생기게 된다 (A -> B를 
참조하고, B -> A 를 참조)
  - 필드 주입과 수정자 주입은 빈이 생성된 후에 참조를 하기 때문에 
애플리케이션이 어떠한 오류와 경고 없이 구동(실제 코드가 호출될 때까지 
문제를 알수 없다)
  - 생성자를 통해 주입하게 되면 BenaCurrentlyInCreationException이 발생
  
  
 
# Component 스캔


## Component Scan
스프링은 설정 정보 없이 스프링 빈을 등록하는 컴포넌트 스캔이라는 기능을 
제공했다.

- 지금까지는 스프링 빈을 등록할 때 자바 코드의 @Bean or XML의 등의 설정 
정보에 등록할 스프링 빈들을 직접 작성했다, 하지만 이렇게 수작업을 통해 
설정 정보를 작성하면 설정 정보가 커지고, 누락하는 다양한 문제를 
발생시킬수 있다.
- @ComponentScan 은 @Component가 붙은 모든 클래스를 스프링 빈으로 
등록해주기 때문에 설정 정보에 붙여주면 된다
   - 의존관계도 자동으로 주입하는 @Autowired 기능도 제공
```java
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration
import org.springframework.context.annotation.FilterType;

@Configuration
@ComponentScan
public class AutoAppConfig {
}
```
기존에 작성하던 AppConfig 와 비교한다면 @Bean 으로 등록한 클래스를 볼 수 
없다.
(주의) 컴포넌트 스캔을 사용하면 @Configuration이 붙은 설정 정보도 
자동으로 등록

@Configuration 이 붙은 설정 정보가 자동 등록 되는 이유는 @Configuration 
코드에 @Component 애너테이션이 붙어있기 때문이다
![](https://velog.velcdn.com/images/minthug94_/post/4c7b43e2-bc2f-4f1e-9ef5-9af36a6e0c77/image.png)

기존에 작성한 AppConfig가 있다면 정상적인 작동이 되지 않는다, 새 
프로젝트로 진행할 경우엔 문제되지 않는다

AppConfig 등 @Configuration 설정이 된 파일이 있을 시 아래 코드 추가:
@ComponentScan(ExcludeFilters = @Filter(type = FilterType.ANNOTATION, 
classes = Configuration.class))

```XML
<beans>
  <context:component-scan base-package="com.acme"/>
</beans>
```

## basePackages
탐색할 패키지의 시작 위치를 지정하고, 해당 패키지부터 하위 패키지 모두 
탐색한다

- @ComponentScan()의 매개변수로 basePackages = ""를 줄 수 있다
- 지정하지 않으면, @ComponentScan이 붙은 설정 정보 클랫의 패키지가 시작 
위치가 된다
   - 설정 정보 클래스의 위치를 프로젝트 최상단에 두고 패키지 위치는 
지정하지 않는 방법이 가장 편할수 있다.
- 스프링 부트를 사용하면 @SpringBootApplication 를 이 프로젝트 시작 루트 
위치에 두는 것을 추천
  - @SpringBootApplication 에 @ComponentScan이 들어있다

## 컴포넌트 스캔 기본 대상
- @Component : 컴포넌트 스캔에서 사용된다
- @Controller & @RestController : 스프링 MVC 및 REST 전용 컨트롤러에서 
사용된다
- @Service : 스프링 비즈니스 로직에서 사용된다
   - 특별한 처리를 하지않는다
   - 개발자들이 핵심 비즈니스 로직이 여기에 있다는 비즈니스 계층을 
인식하는데 도움을 준다
- @Repository : 스프링 데이터 접근 계층에서 사용된다
   - 스프링 데이터 접근 계층으로 인식하고, 데이터 계층의 예외를 스프링 
예외로 변환해준다
- @Configuration : 스프링 설정 정보에서 사용된다
   - 스프링 설정 정보로 인식되고, 스프링 빈이 싱글톤을 유지하도록 추가 
처리한다

![](https://velog.velcdn.com/images/minthug94_/post/c22f5cb9-2d98-4ea5-a2fd-3a300f03b7bd/image.png)

 해당 클래스의 소스 코드엔 @Component를 포함하고 있다
 
 ### 필터
 
 - includeFilters : 컴포넌트 스캔 대상을 추가로 지정한다
 - excludeFilters : 컴포넌트 스캔에서 제외할 대상을 지정한다
 - ### FilterType options
    - ANNOTATION : 기본값, 애너테이션으로 인식해서 동작
    - ASSIGNABLE_TYPE : 지정한 타입과 자식 타입을 인색해서 동작
    - ASPECTJ : AspectJ 패턴을 사용
    - REGEX : 정규 표현식을 나타낸다
    - CUSTOM : TypeFilter 라는 인터페이스를 구현해 처리한다

