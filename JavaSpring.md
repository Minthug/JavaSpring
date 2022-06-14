<iframe src="https://giphy.com/embed/oNPfwCas8wM0sItsJz" width="320" 
height="150" frameBorder="0" class="giphy-embed" 
allowFullScreen></iframe><p><a 
href="https://giphy.com/gifs/ShalitaGrant-yay-spring-is-here-oNPfwCas8wM0sItsJz">via 
GIPHY</a></p>

# Framework
Frame이란 단어는 여러가지의 의미가 있지만 '틀', '뼈대', '구조'로 알아두자
일상생활에 빗대어 비유하면 벽에 거는 <span style="color: indianred"> 
**그림이나 사진 등의 액자를 프레임**</span> 이라고도 하며, <span 
style="color: indianred"> **자동차의 뼈대**</span> 가 되는 강판으로 된 
껍데기 역시 자동차의 프레임이라고 한다

Frame은 어떤 대상의 큰 틀이나 외형적인 구조를 의미하는데 프로그래밍 
세계에서도 비슷한 의미를 갖고있다.

```html
<frameset cols="33%,*,33%">
  <frame name="left" src="/left_menu"/>
  <frame name="center" src="/context"/>
  <frame name="right" src="/right_menu"/>
</frameset>
```
HTML5 버전에선 지원하고 있지 않지만 웹의 초창기 시절에는 HTML 문서를 
구성하는 태그 중에서 frame이라는 태그가 존재했다.

Java에서 Framework는 Collections Framework이다
우리가 이미 사용한 Map이나 Set, List 등의 Collection들은 데이터를 저장하기 
위해 널리 알려져 있는 자료구조를 바탕으로 비슷한 유형의 데이터들을 가공 및 
처리하기 쉽도록 표준화된 방법을 제공하는 클래스의 집합이다

하지만 왜 Java의 Collection에 Framework 라는 용어를 붙였을까?
'틀', '뼈대' 같은 단어를 듣게 되면, Java의 구성 요소 중 어떤 것이 가장 
떠오르는가?
Java 클래스의 유형 중에서 기본적인 뼈대로만 구성되어 있는 것은 바로 추상 
메서드만 정의되어 있는 인터페이스 이다
그리고 Java에선 Collection은 바로 Map,Set,List 같은 인터페이스와 그 
인터페이스들을 구현한 구현체들의 집합인것이다.

쉽게 프로그래밍 상에서의 Framework는 기본적으로 프로그래밍을 하기 위한 
어떠한 틀이나 구조를 제공한다는 것

## What is difference library and framework?이 둘의 차이점은 무엇일까?
Framework는 단지 미릴 만들어 둔 반제품이나, 확장해서 사용할 수 있도록 
준비된 추상 library의 집합이 아니다
Framework가 어떤 것인지 이해하려면 둘의 차이가 어떻게 다른지 확실히 알아야 
할 것이다.

![](https://velog.velcdn.com/images/minthug94_/post/6c38cc22-dcc1-4bc6-9884-9f0d0141128b/image.png)

### Framework
위에서 언급했다시피 '뼈대'나 '기반구조'를 뜻하고, 제어의 역전 개념이 
적용된 대표적인 기술이다
소프트웨어에서의 프레임워크는 '소프트웨어의 특정 문제를 해결하기 위해서 
상호 협력하는 클래스와 인터페이스의 집합' 이라 할 수 있으며, 완성된 
어플리케이션이 아닌 프로그래머가 완성시키는 작업을 해야한다.

객체 지향 개발을 하게 되면서 통합성, 일관성의 부족이 발생되는 문제를 
해결할 방법 중 하나라고 할 수 있다.

Framework 특징
- 특정 개념들의 추상화를 제공하는 여러 클래스나 컴포넌트로 구성되어 있다.
- 추상적인 개념들이 문제를 해결하기 위해 같이 작업하는 방법을 정의한다
- 컴포넌트들은 재사용이 가능하다
- 높은 수준에서 패턴들을 조작화 할 수 있다.

### Library
Library는 단순 활용가능한 도구들의 집합을 말한다
즉, 개발자가 만든 클래스에서 호출하여 사용, 클래스들의 나열로 필요한 
클래스를 불러서 사용하는 방식을 취하고 있다.

### 둘의 차이점
Library와 Framework 의 차이는 제어 흐름에 대한 주도성이 누구에게/어디에 
있는가에 있다.
즉, 어플리케이션의 <span style="color: orange">**Flow를 누가 쥐고 
있느냐**</span> 에 달려 있다.

Framework는 전체적인 흐름을 스스로가 쥐고 있으며 사용자는 그 안에서 필요한 
코드를 짜 넣는다, 반면에 라이브러리는 사용자가 전체적인 흐름을 만들며 
라이브러리를 가져다 쓰는 것이라고 할 수 있다.

라이브러리는 라이브러리를 가져다가 사용하고 호출하는 측에 전적으로 
주도성이 있으며, 프레임워크는 그 틀 안에 이미 제어 흐름에 대한 주도성이 
내재되어 있다.

프레임워크는 가져다가 사용한다기보다는 거기에 들어가서 사용한다는 느낌으로 
접근할 수 있다.

> 라이브러리를 사용하는 어플리케이션 코드는 어플리케이션 흐름을 직접 
제어한다
단지 동작하는 중에 필요한 기능이 있을 때 능동적으로 라이브러리를 사용할 
뿐이다
반면 프레임워크는 거꾸로 어플리케이션 코드가 프레임워크에 의해 사용 되는 
것뿐이다.
보토 프레임워크 위에 개발한 클래스를 등록해두고, 프레임워크가 흐름을 
주도하는 중에 개발자가 만든 어플리케이션 코드를 사용하도록 만드는 
방식이다.
프레임워크에는 분명한 <span style="color: orange"> 제어의 역전 </span> 
개념이 적용되어 있어야 한다.
어플리케이션 코드는 프레임워크가 짜놓은 틀에서 수동적으로 동작해야 한다.

이해를 돕기위한 예시 코드
```java
@SpringBootApplication
@RestController
@RequestMapping(path = "/v1/message")
public class SampleApplication {
    @GetMapping
    public String getMessage() {  // (2)
        String message = "hello world";
        return StringUtils.upperCase(message); // (1)
    }

    public static void main(String[] args) {
        SpringApplication.run(SampleApplication.class, args);
    }
}
```

명확하게 Library를 사용하는 부분은 (1)의 
<code>StringUtils.upperCase(message)'</code>이다
StringUtils 클래스는 Apache Commons Lang3 라이브러리의 유틸리티 클래스 중 
하나인데 애플리케이션이 동작하는 중에 <code>StringUtils</code>클래스의 
<code>upperCase()</code> 메서드의 파라미터로 전달하는 문자열을 대문자로 
변환하고 있다.

그런데 StringUtils 클래스가 라이브러리라는 것을 어떻게 판단할수 있을까?

예시와 같이 어플리케이션 코드는 어플리케이션을 개발하는 사람 즉, 개발자가 
작성을 할 것이다.
이렇게 개발자가 짜 놓은 코드내에서 필요한 기능이 있으면 해당 라이브러리를 
호출해 사용하는 것이 Library이다.

즉, 어플리케이션 흐름의 주도권이 개발자에게 있는 것

예시에서 사용한 애너테이션이나 <code>main()</code> 메서드 내의 
<code>SpringApplication.run()</code>메서드는 Spring Framework에서 지원하는 
기능들인데 이런 기능들은 라이브러리와 다르게 코드 상에는 보이지 않는 
상당히 많은 일들을 한다

(2)의 <code>getMessage()</code> 메서드 내부의 코드처럼 개발자가 메서드 
내에 코드를 작성해두면, Spring Framework에서 개발자가 작성한 코드르 
사용해서 어플리케이션의 흐름을 만들어낸다.

즉, 어플리케이션 흐름의 주도권이 개발자가 아닌 Framework에 있는 것이다

이건 ioC(Inversion Of Control,제어의 역전)이라는 개념이였다.

## POJO(Plain Old Java Object)
Spring 삼각형
![](https://velog.velcdn.com/images/minthug94_/post/ea740030-7cb3-417f-ae11-e6312c5f67b8/image.png)

그림을 보면 POJO는 Spring에서 사용하는 핵심 개념들에 둘러 싸여져 있는 
모습이다.
이렇게 POJO 라는 것을 IoC/DI,AOP,PSA를 통해서 달성할 수 있다는걸 뜻한다.

PO'JO'
JO는 Java 로 짜여진 코드는 어떤식으로든 객체와 객체가 관계를 맺을 수 밖에 
없는 객체지향프로그래밍 을 뜻한다

PO 는?
Plain Object 즉, 자바로 생성하는 순수한 객체를 의미한다

'POJO' 프로그래밍은 POJO를 이용해 프로그래밍 코드를 작성하는 것을 
뜻하고있다.
순수 자바 객체만을 사용해서 프로그래밍 코드를 작성한다고 POJO 
프로그래밍이라고 볼 수 없다.

POJO 프로그래밍으로 쓴 코드는 두 가지으 정도의 기본적인 규칙이 있다고 
한다.

- ### Java 나 Java의 스펙에 정의된 것 이외에는 다른 기술이나 규약에 
얽매이지 않아야 한다.

```java
public class MEssageForm extends ActionForm { // (1)
String message;

	public String getMessage() {
		return message;
	}

	public void setMessage(String message) {
		this.message = message;
	}
	
}

public class MessageAction extends Action{ // (2)
	
	public ActionForward execute(ActionMapping mapping, ActionForm 
form,
		HttpServletRequest request, HttpServletResponse response)
        throws Exception {
		
		MessageForm messageForm = (MessageForm) form;
		messageForm .setMessage("Hello World");
		
		return mapping.findForward("success");
	}
	
}
```
위 코드는 Java 코드가 특정 기술에 종속적인 예를 보여주기 위한 코드이다
ActionForm 클래스는 과거에 Struts 라는 웹 프레임워크에서 지원하는 
클래스였다.
(1) 에선 Struts 라는 기술을 사용하기 위해 ActionForm을 상속하고 있다.
(2) 에선 역시 Struts 기술의 Action 클래스를 상속 받고 있다.

이렇게 특정 기술을 상속해서 코드를 작성하게 되면 추후 애플리케이션 
요구사항이 변경되서 다른 기술로 변경할때 Struts의 클래스를 명시적으로 
사용했던 부분을 전부 다 일일이 제거하거나 수정해야한다.

그리고 Java는 다중 상속을 지원하지 않기 때문에 'extends' 키워드를 사용해서 
한 번 상속하게 되면 상위 클래스를 상속받아서 하위클래스를 확장하는 
객체지향 설계 기법을 적용하기 어려워진다.

- ### 특정 환경에 종속적이지 않아야한다.

순수 Java로 작서한 어플리케이션 코드내에 Tomcat이 지원하는 API를 직접 
가져다 사용했다고 가정하자
그런데 만약 시스템의 요구 사항이 변경되어 Tomcat 말고 Zetty라는 다른 
Servlet Container를 사용하면 어떻게 될까?

어플리케이션 코드에서 사용하는 Tomcat API 코드들을 모두 걷어내 Zetty로 
수정하던지 최악의 경우엔 어플리케이션을 전부 수정해 고쳐야하는 상황을 
만날지도 모른다

## POJO 필요한 이유
- 특정 환경이나 기술에 종속적이지 않고 재사용이 가능하며, 확장 가능한 
유연한 코드를 작성할 수 있다.
- 저수준 레벨의 기술과 환경에 종속적인 코드를 애플리케이션 코드에서 제거 
함으로써 코드가 깔끔해진다.
- 코드가 깔끔해지기 때문에 디버깅하기도 상대적으로 쉽다.
- 특정 기술이나 환경에 종속적이지 않기 때문에 테스트 역시 단순해진다.
- 객체지향적인 설계를 제한없이 적용할 수 있다. (제일 중요함)

## POJO와 Spring 관계
Spring은 POJO 프로그래밍을 지향하는 Framework 이다.

최대한 다른 환경이나 기술에 종속적이지 않도록 하기 위해 POJO 프로그래밍 
코드를 작성하기 위해 Spring에서 세가지 기술에 지원한다
바로 **IoC/DI, AOP, PSA** 이다

# IoC(Inversioin of Control)/DI(Dependency Injection)

![](https://velog.velcdn.com/images/minthug94_/post/9fa590f7-b8a7-4d69-93d7-f73e441cd96b/image.png)

Library는 애플리케이션 흐름의 주도권이 개발자에게 있고, Framework는 
애플리케이션 주도권이 Framework에 있다고 했다.

여기서 말하는 애플리케이션의 흐름 주도권이 뒤바뀐 것을 바로 IoC 라고 한다.

```java
Public class Ex2 {
	Public static void main(String[] args) {
    	System.out.println("Hello IoC!");
        }
    }
```
일반적으로 Java 콘솔 어플리케이션을 실행하려면 <code>main()</code> 
메서드가 존재해야 한다
그래야 이 <code>main()</code> 메서드 안에서 다른 객체의 메서드를 
호출하던지 하는 프로세스가 진행이 되니

위 코드에선 메인 메서드가 호출 된 후 <code>System</code> 클래스를 통해 
<code>static</code> 멤버 변수인 <code>out</code> 의 <code>println()</code> 
을 호출한다

이렇게 개발자가 작성한 코드를 순차적으로 실행하는게 어플리케이션의 
일반적인 제어 흐름이다

### Java web 어플리케이션에서 IoC가 적용되는 예

![](https://velog.velcdn.com/images/minthug94_/post/465e9b7c-0452-42ab-9d32-b9336ad6ab83/image.png)

서블릿 기반의 어플리케이션을 웹에서 실행하기 위한 서블릿 컨테이너의 
모습이다

Java 콘솔 어플리케이션의 경우엔 <code>main()</code> 메서드가 종료되면 
어플리케이션의 실행이 종료된다.

하지만 웹에서 동작하는 어플리케이션의 경우 클라이언트가 외부에서 접속해서 
사용하는 서비스이기 때문에 <code>main()</code> 메서드가 종료되지 않아야 할 
것이다

그런데 서블릿 컨테이너에는 서블릿 사양(Specification)에 맞게 작성된 서블릿 
클래스만 존재하고 별도의 메인 메서드가 존재하지 않는다

#### 그럼 어떻게 실행되는것일까? 메인 메서드가 없는데 ?

서블릿 컨테이너의 경우엔 클라이언트 요청이 들어올 때마다 서블릿 컨테이너 
내의 컨테이너 로직(<code>service()</code>메서드)이 서블릿을 직접 실행시켜 
주기때문에 메인 메서드가 필요없다

이 경우엔 서블릿 컨테이너가 서블릿을 제어하고 있기 때문에 어플리케이션 
주도권이 서블릿 컨테이너에게 있다
그러니 서블릿과 웹 어플리케이션 간에 IoC의 개념이 적용되어 있는 것이다

## DI?
IoC는 서버 컨테이너 기술, 디자인 패턴, 객체 지향 설계 등의 적용하게 하는 
일반적인 개념인데 반해서 DI는 IoC개념을 조금 더 구체화 시켰다고 볼 수 
있다.

Dependency == '의존하는 또는 종속되는' / Injection == '주입'

의존성 주입은 제어의 역전이 일어나는 것을 전제로 스프링 내부의 객체들 간의 
관계를 관리할 때 사용한다.
의존성 주입은 특정 개체에 피룡한 객체를 외부에서 결정하여 연결시키는 것을 
말한다.
자바에서는 인터페이스를 사용하려 의존적인 관계를 처리한다

- 메소드나 객체의 호출 작업은 제어의 역전을 통해 외부에서 이루어진다. 
(여기서 외부라 함은 객체를 기준으로 봤을 때의 외부를 뜻한다.)
- 제어의 역행을 전제조건으로 의존성 주입이 일어난다.
- 의존성을 가진 객체에 대해 스프링에서 의존성 주입이 발생하도록 한다.
- 의존성 주입 특징으로 인해 개발자가 POJO 개발이 가능하게 된다.

> 실무에선 주로 정형화된 컨트롤러, 서비스, 레포지토리 같은 코드는 컨포넌트 
스캔을 이용한다
그리고 정형화 되지 않거나, 상황에 따라 구현 클래스를 변경해야 하면 설정을 
통해 스프링 빈으로 등록한다.

### 컴포넌트 스캔과 자동 의존관계 설정
예를 들어, MemberController의 생성자에 @Autowired를 붙임으로써, 스프링은 
스프링이 저장하고 있는 memberService 객체를 컨트롤러가 생성될 때 
가져와준다. 즉, 생성자에 @Autowired가 있으면 스프링이 연관된 객체를 스프링 
컨테이너에서 찾아서 넣어준다.
이렇게 객체 의존관계를 외부에서 넣어주는 것을 의존성 주입이라 한다

스프링이 시작될 때, @Controller, @Service, @Repository와 같은 컴포넌트들을 
스캔하고 스프링에 빈으로 등록한다.
기본적으로 @Component 가 있으면 스프링 빈으로 자동등록 되며, 위의 세 
어노테이션도 @Component를 포함하고 있기에 스프링 빈으로 자동 등록 된다. 
스프링 빈으로 등록된다면, "스프링 컨테이너에서 빈이 관리된다" 라고 말한다.
@Autowired 어노테이션으로 등록된 빈들의 의존 관계를 연결한다.

👉🏼 [Bean이 뭐야?](https://velog.io/@minthug94_/Java-bean)

![](https://velog.velcdn.com/images/minthug94_/post/fc85fbeb-d535-4ea8-85f6-f049d568bc0b/image.png)

- ### 의존성 주입의 방법
1. 필드 주입
2. setter 주입
3. 생성자 주입 **(권장)**

### 왜 생성자 주입을 권장하는가?
1. 순환 의존성 확인: 필드 주입으로는 순환 의존성을 파악하기 어렵다.
생성자 주입을 하게되면 서버 기동 시 순환 의존성을 가지는 요소들을 파악할 
수 있게 에러메시지를 표시하면서 서버 기동이 되지 않는다.
2. 불변성: 필드 주입은 final을 선언할 수 없지만, 생성자 주입은 final을 
선언함으로써 객체가 변하지 않도록 방지해준다.
3. 단일 책임 원칙 위반 확인

### @Autowired 어노테이션을 이용한 의존성 주입
객체 연결을 할 때 사용,
필드 혹은 생성자에 사용하면 컨트롤러, 서비스 등이 생성될 때 스프링 빈에 
등록되어 있는 객체를 가져다 넣어준다.

1. 필드 주입 방법
```java
public class ExampleCase {
	@Autowired
    private ChocolateService chocolateService;
    @Autowired
    private DrinkService drinkService;
```
2. 생성자 주입 방법 **(권장)**
```java
public class ExampleCase {
	private final ChocolateService chocolateService;
    private final DrinkService drinkService;
    
    @Autowired
    public ExampleCase(ChocolateService chocolateService, DrinkService 
drinkService){
    this.chocolateService = chocolateService;
    this.drinkService = drinkService;
    }
 }   
```

3. @RequiredArgsConstructor 어노테이션을 이용한 의존성 주입
```
@RequiredArgsConstructor // final로 선언된 멤버 변수를 자동으로 생성한다.
@RestController // JSON으로 데이터를 주고받음을 선언한다.
public class ProductRestController {
	private final ProductService productService;
    private final ProductRepository productRepository;
    
    // 등록된 전체 상품 목록 조회
    @GetMapping("/api/products")
    public List<Product> getProducts() {
    	return productRepository.findAll();
        }
     }   
```

@RequiredArgsConstructor라는 어노테이션을 붙이면 final 필드나 @NonNull이 
붙은 필드에 대해 생성자를 생성해준다.
주로 의존성 주입의 편의성을 위해서 사용된다.

어떠한 빈에 생성자가 오직 하나만 있고, 생성자의 파라미터 타입이 빈으로 
등록 가능한 존재라면 이 빈은 @Autowired 어노테이션 없이도 의존성 주입이 
가능하다.


이해를 돕기 위해 예시를 써보자


```java
class BurgerChef {
	private HamBurgerRecipe hamBurgerRecipe;
    public BurgerChef() {
    	hamBurgerRecipe = new HamBurgerRecipe(;
        }
     }   
```

위 코드의 경우, 햄버거 레시피가 변하게 되었을때, 변화된 레시피에 따라 
BurgerChef 클래스를 수정해야한다.
레시피의 변화가 요리사의 행위에 영향을 미쳤기 때문에 요리사는 레시피에 
의존한다고 말할수 있다.

Dependency를 인터페이스로 추상화한다면?

위 코드에선 BurgerChef는 HambergerRecipe만 의존할 수 있는 구조로 되어 
있었다.
더 다양향 햄버거 레시피를 의존할 수 있게 구현하려면 인터페이스로 
추상화해보면 된다

```java
class BurgerChef {
	private BurgerRecipe burgerRecipe;
    
    public BurgerChef() {
    	burgerRecipe = new HamBergerRecipe();
        //burgerRecipe = new CheeseBurgerRecipe();
        //burgerRecipe = new ChickenBurgerRecipe();
        }
    }    
     
    interface BurgerRecipe {
    	newBurger();
    }
    
class HamBurgerRecipe implements BurgerRecipe {
	public Burger newBurger() {
    		return new HamBerger();
    	}
    }    
```
 새로 쓴 코드에서 볼 수 있듯이, 다양한 버거 레시피에 의존할 수 있는 
BurgerChef가 되었다.
 이처럼 의존 관계를 인터페이스로 추상화하게 되면, 더 다양한 의존 관계를 
맺을 수 있고, 실제 구현 클래스와의 관계가 느슨해지며 결합도가 낮아진다.
 
 지금까지의 구현에선 BurgerChef 내부적으로 의존 관계인 BurgerRecipe가 어떤 
값을 가질지 직접 정하고 있다.
 이때 DI는 어떤 햄버거 레시피를 만들지는 버거 가게 사장님이 정하는 
상황이라 할 수 있다.
 즉, BurgerChef가 의존하고 있는 BurgerRecipe를 외부(사장님)에서 결정하고 
주입한 것이다.
 
 ```java
 class BurgerChef {
 	private BurgerRecipe burgerRecipe;
    
    public BurgerChef(BurgerRecipe burgerRecipe) {
    	this.burgerRecipe = burgerRecipe;
        }
     }
     
 //의존관계를 외부에서 주입 -> DI
 new Burgerchef(new HamBurgerRecipe());
 new Burgerchef(new CheeseBurgerRecipe());
 new Burgerchef(new ChickenBurgerRecipe());
 ```
 
 이처럼 그 의존 관계를 외부에서 결정하는 것을 DI(의존 관계 주입)라 한다
 
![](https://velog.velcdn.com/images/minthug94_/post/f92b77da-b62a-4e2a-88e4-9bc48bf60a58/image.png)

스프링에선 외부의 대상이 IoC 컨테이너가 되어, 빈을 알아서 주입해준다.

# AOP(Aspect Oriented Programming)
![](https://velog.velcdn.com/images/minthug94_/post/44709ba2-7259-4e3c-bb9d-5a34fdafbe0f/image.png)

AOP 를 한글로 직역하면 "관심 지향 프로그래밍" 정도로 해석이 가능하다

스프링은 AOP를 통해 반복적인 코드를 줄이고 개발자가 핵심 비즈니스 로직에만 
집중할 수 있도록 지원한다.

>  공통 관심 사항 (cross-cutting concern) vs 핵심 관심 사항(core concern) 

비즈니스 로직은 아니지만 보안, 로그, 트랜잭션과 같이 반드시 처리가 필요한 
부분을 스프링에서는 공통 관심사항이라 한다

예시를 들면 회원 가입과 회원 조회에서 시간을 측정하는 기능은 핵심 관심 
사항이 아니다
여기서 시간을 척정하는 기능은 공통 관심 사항 이다.
이들을 같은 Class 안에 메소드로 구현하면 핵심 비즈니스 로직과 공통 관심 
사항에 해당하는 로직이 섞여 유지보수가 매우 어려워진다.
하지만 시간을 측정하는 로직은 별도의 공통 로직으로 만들기도 어렵다.
이 경우 시간을 측정하는 로직을 변경할 땐 모든 로직을 찾아가며 변경해야 
하는 불상사가 생긴다.

![](https://velog.velcdn.com/images/minthug94_/post/b6b3f0fd-34c8-40bc-bdbf-653dae862328/image.png)

모든 Class에 시간 측정 로직을 일일이 작성해야 했지만,
![](https://velog.velcdn.com/images/minthug94_/post/40047a2c-f5d1-4269-8bd5-813a96749124/image.png)

AOP를 통해 한번에 해결할 수 있다.

```java
@Component
@Aspect
public class TimeTraceAop {
	@Around("execution(* hello.helloSpring..*(..))")
    
    public Object execute(ProceedingJoinPoint joinPoint) throws Throwable 
{
    	long start = System.currentTimeMillis();
        System.out.println("START: " + joinPoint.toString());
        try {
        	return joinPoint.proceed();
            } finally {
            	long finish = System.currentTimeMillis();
                long timeMs = finish - start;
                System.out.println("END: " + joinPoint.toString() + " " + 
timeMs + "ms");
                }
           	}
        }    
```
AOP로 활용한 객체에는 @Aspect 어노테이션을 붙여줘야 한다.

@Around로 AOP 를 적용할 객체를 지정한다. 반복되는 패턴이라 단순하다.
위는 hello.helloSpring 하위 패키지에 모두 AOP 로직을 적요하라는 의미의 
예시코드이다.

회원가입과 회원조회와 같은 핵심 관심사항과 시간을 측정하는 공통 관심 
사항을 분리하였다
시간을 측정하는 로직을 별도의 공통 로직으로 만들었다.
이제 핵심 관심 사항을 깔금하게 유지할 수 있다.
공통 관심 사항 로직을 변경할 일이 생기면 AOP로 사용하는 로직의 코드만 
바꾸면 된다.
심지어, 원하는 적용 대상을 정해 적용할수 있다.

------------------------
참고 url
https://joychae.tistory.com/27
https://webclub.tistory.com/458
