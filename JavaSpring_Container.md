# 스프링 컨테이너
스프링 컨테이너는 내부에 존재하는 애플리케이션 빈의 생명주기를 관리한다.
- 빈 생성, 관리, 제거 등의 역할을 담당한다

# 스프링 컨테이너는 뭘까?
<code>ApplicationContext</code>를 스프링 컨테이너라고 하고 인터페이스로 
구현되어 있다.(다형성 적용)

- 스프링 컨테이너는 XML, 애너테이션 기반의 자바 설정 클래스로 만들수 있다.
- 이전엔 개발자가 XML을 통해 모두 설정해야했지만, 이런 복잡한 부분들을 
Spring Boot를 사용하면서 더 이상 사용하지 않게되었다.
- 빈의 인스턴스화, 구성, 전체 생명 주기 및 제거까지 처리
(컨테이너는 개발자가 정의한 빈을 객체로 만들어 관리하고 개발자가 필요로 할 
때 제공)
- 스프링 컨테이너를 통해 원하는 만큼 많은 객체를 가질 수 있다.
- 의존성 주입을 통해 애플리케이션의 컴포넌트를 관리한다
   - 스프링 컨테이너는 서로 다른 빈을 연결해 어플리케이션의 빈을 연결하는 
역할
   - 개발자가 모듈 간에 의존 및 결합으로 인해 발생한 문제로부터 자유로울 
수 있다.
   - 메서드가 언제,어디서 호출되야하는지, 메서드를 호출하기 위해 필요한 
매개 변수를 준비해서 전달하지 않는다.

# 왜 사용할까?
- 객체를 사용하기 위해 new 생성자를 써야 했다.
- 애플리케이션에서 이런 객체가 무수히 많이 존재하고 서로 참조하게 
되어있었다.
  - 서로 참조가 심할수록 의존성이 높다
  - 낮은 결합도와 높은 캡슐화가 객체지향프로그래밍의 핵심 중 하나인데 높은 
의존성은 이를 지킬수없다.
- 객체 간의 의존성을 낮추기위해 Spring 컨테이너가 사용된다.

그래서, 기존의 방식으론 새로운 정책이 생기게 될 경우 변경 사항들을 
수작업으로 수정이 필요했다.
서로 의존이 많이 되어있지 않는 작업 초반부에는 하나 하나 수정이 
가능하지만, 점점 서비스의 코드가 방대해질 경우 의존도는 높아져있을거고 
그에 따른 코드의 변경도 많이 필요해질것이다
하지만, 스프링 컨테이너를 사용하면서 구현 클래스에 있는 의존을 제거하고 
인터페이스에만 의존하도록 설계할 수 있다.

# 어떻게 생성하나?
- 스프링 컨테이너는 Configuration Metadata를 사용한다
- 스프링 컨테이너는 파라미터로 넘어온 설정 클래스 정보를 사용해 스프링 
빈에 등록한다
- new AnnotationConfigApplicationContext(구성정보.class) 로 스프링에 있는 
@Bean의 메서드를 등록한다
- [애너테이션 기반 컨테이너 
구성](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-annotation-config)
- 애너테이션 기반의 자바 설정 클래스로 Spring을 만드는 것을 뜻한다

```java
// Spring Container 생성
ApplicationContext applicationContext = new 
AnnotationConfigApplicationContext(AppConfig.class);

```

XML 기반으로 만드는 ClassPathXmlApplicationContext도 있다

XML 기반 구성 메타데이터 기본 구조
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
        
    <bean id="..." class="...">  
        <!-- collaborators and configuration for this bean go here -->
     </bean>
        
     <bean id="..." class="...">
         <!-- collaborators and configuration for this bean go here -->
     </bean>
        
     <!-- more bean definitions go here -->
        
</beans>

<beans /> 에 필요한 값을 설정한다
<bean id="...">: 빈 정의를 식별하는데 사용되는 문자열
<bean class="...">: 빈의 유형을 정의하고 클래스 이름을 사용한다
```

스프링 컨테이너를 만드는 다양한 방법은 ApplicationContext 인터페이스 
구현체이다

1. AppConfig.class 등의 구성 정보를 지정해줘 스프링 컨테이너를 생성해야 
한다
2. AppConfig에 있는 구성 정보를 통해 스프링 컨테이너는 필요한 객체들을 
생성하게 된다
3. 애플리케이션 클래스는 구성 메타데이터와 결합되어 AplicationContext 생성 
및 초기화된 후 완전히 구성되고 실행 가능한 시스템 또는 애플리케이션을 갖게 
된다.

스프링 빈 조회에서 상속관계가 있을 시 부모타임으로 조회하면 자식 타입도 
함께 조회한다.
만약 모든 자바 객체의 최고 부모인 object타입으로 조회하면 모든 스프링 빈을 
조회한다.

## 스프링 컨테이너의 종류
파라미터로 넘어온 설정 클래스 정보를 참고해서 빈의 생성, 관계 설정 등의 
제어작업을 총괄하는 컨테이너이다.

### BeanFactory
스프링 컨테이너의 최상위 인터페이스이다, 빈을 등록하고 생성하고 조회하고 
돌려주는 등 빈을 관리하는 역할을 한다
getBean() 메소드를 통해 빈을 인스턴스화 할 수 있다.
@Bean이 붙은 메서드의 명을 스프링 빈의 이름으로 사용해 'Bean 등록' 을 한다

### ApplicationContext
BeanFactory의 기능을 상속받아 제공한다 빈을 관리하고 검색하는 기능을 
BeanFactory가 제공하고 그 외에 부가 기능을 제공한다
> 부가기능
MessageSource : 메세지 다국화를 위한 인터페이스
EnvironmentCapable : 개발, 운영 등 환경변수 등으로 나눠 처리하고, 
애플리케이션 구동 시 필요한 정들을 관리하기 위한 인터페이스
ApplicationEventPublisher : 이벤트 관련 기능을 제공하는 인터페이스
ResourceLoader : 파일, 클래스 패스, 외부 등 리소스를 편리하게 조회

## 컨테이너 인스턴스화
ApplicationContext 생성자에 제공된 위치 경로 또는 경로는 컨테이너가 로컬 
파일 시스템, Java CLASSPATH 등과 같은 다양한 외부 리소스로부터 구성 
메타데이터를 로드할 수 있도록 하는 리소스 문자열
```java
// Annotation
ApplicationContext context = new 
AnnotationConfigApplicationContext(AppConfig.class);

//XML
ApplicationContext context = new 
ClassPathXmlApplicationContext("services.xml", "daos.xml");

```


### new 생성자와 주입 코드의 차이점

new 를 통해 직접 객체 생성 방식
```java
public class OrderServiceImpl implements OrderService {
	private final UserRepository userRepository = new 
UserRepositoryImpl();
    private final DiscountInfo discountInfo = new RateDiscountInfo();
    
    @Override
    public Order createOrder(Long userId, String itemName. int itemPrice) 
{
    	User user = userRepository.findByUserId(userId);
        int discountPrice = discountInfo.discount(user, itemPrice);
        return new Order(userId, itemName, itemPrice, discountPrice);
       }
     }  
```

생성자 주입을 통해 IoC & DI
```java
public class OrderServiceImpl implements OrderService {
	private final UserRepository userRepository;
    private final DiscountInfo discountInfo;
    
    public OrderServiceImpl(UserRepository userRepository, DiscountInfo 
discountInfo) {
    	this.userRepository = userRepository;
        this.discountInfo = discountInfo;
        }
    
    @override
    public Order createOrder(Long userId, String itemName, int itemPrice) 
{
    User user = userRepository.findByUserId(userId);
    int discountPrice = discountInfo.discount(user, itemPrice);
    return new Order(userId, itemName, itemPrice, discountPrice);
    }
 }   
        
```

new를 사용하는 대신 생성자를 통해 의존 객체가 주입되고, 느슨한 의존 관계가 
이루어진다
userRepository와 discountInfo의 구현 클래스는 Bean 설정에 따라 유연하게 
변하게 된다
OrderServiceImpl 입장에선 생성자를 통해 어떤 구현 객체가 주입될지 알 수 
없으며, 알 필요도 없다
어떤 객체가 주입될지는 외부(AppConfig)에서 결정한다
OrderServiceImpl 는 오로지 실행에만 집중하게 된다


