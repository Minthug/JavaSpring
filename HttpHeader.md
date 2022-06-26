# HTTP Headers?

HTTP 메시지의 구성 요소 중 하나로써, 클라이언트의 요청이나 서버의 응답에 
포함되어 부가적인 정보를 HTTP 메시지에 포함할 수 있도록 해준다.

## 사용 목적

### 클라이언트와 서버 관점에서의 대표적인 HTTP 헤더 예시
클라이언트와 서버의 관점에서 내부적으로 가장 많이 사용되는 헤더 정보로 
'Content-Type'이 있다.

'Content-Type' 헤더 정보는 클라이언트와 서버가 주고 받는 HTTP messages 
body의 데이터 형식이 무엇인지를 알려주는 역할을 한다

클라이언트와 서버는 'Content-Type'이 명시된 데이터 형식에 맞는 데이터들을 
주고 받는것

> ### 개발자들이 실무에서 사용하는 대표적인 HTTP 예시
- Authorization
'Authorization' 헤더 정보는 클라이언트가 적절한 자격 증명을 가지고 
있는지를 확인하기 위한 정보
일반적으론 REST API 기반 어플리케이션의 경우 클라이언트와 서버 간의 
로그인(사용자 ID/패스워드) 인증에 통과한 클라이언트들은 'Authorization' 
헤더 정보를 기준으로 인증에 통과한 클라이언트가 맞는지 확인하느 절차를 
거친다
- User-Agent
여러가지 유형의 클라이언트가 하나의 서버 애플리케이션에 요청을 전송하는 
경우
스마트폰, 데스크탑 등 들어오는 각각의 요청에 맞게 구분해서 응답 데이터를 
다르게 전송해줘야 하는 경우가 생길것이다
그때, 'User-Agent' 정보를 이용해 각각의 정보를 구분해 처리할 수 있다.

### HTTP Request header 정보 얻기

Spring MVC 에서 HTTP 헤더 정보를 읽어오는 몇가지 방법을 제공하고 있다.

- @RequestHeader로 개별 헤더 정보 받기
```java
import com.mine.example.api.mvc_examples.json.Coffee;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping(path = "/v1/coffee")
public class CoffeeController {
	@PostMapping
    public ResponseEntity postCoffee(@RequestHeader("user-agent") String 
userAgent,
								     
@RequestParam("korName") String korName,
                                     @RequestParam("engName") String 
engName,
                                     @RequestParam("price" int price) {
                                     
	System.out.println("user-agent: " + userAgent);
    return new ResponseEntity<>(new Coffee(korName, engName, price), 
HttpStatus.CREATED);
    }
}    
```

- @RequestHeader 로 전체 헤더 정보 받기
```java
@RestController
@RequestMapping(path = "/v1/members")
public class MemberController {
    @PostMapping
    public ResponseEntity postMember(@RequestHeader Map<String, String> 
headers,
    								 
@RequestParam("email") String email,
                                     @RequestParam("name")" String name,
                                     @RequestParam("phone") String phone) 
{

	for(Map.Entry<String, String> entry : headers.entrySet()) {
    	System.out.println("key: " + entry.getKey() +
        		", value: " + entry.getValue());
      }
      
      return new ResponseEntity<>(new Mamber(email,name,phone), 
HttpStatus.CREATED);
      }
 }     
```

postMember() 요청결과
```
key: user-agent, value: PostmanRuntime/7.29.0
key: accept, value: */*
key: cache-control, value: no-cache
key: postman-token, value: 6082ccc2-3195-4726-84ed-6a2009cbae95
key: host, value: localhost:8080
key: accept-encoding, value: gzip, deflate, br
key: connection, value: keep-alive
key: content-type, value: application/x-www-form-urlencoded
key: content-length, value: 54
```

- HttpServletRequest 객체로 헤더 정보 얻기
```java
@RestController
@RequestMapping(path = "/v1/orders")
public class OrderController {
	@PostMapping
    public ResponseEntity postOrder(HttpServletRequest 
httpServletRequest),
    								
@RequestParam("memberId") long memberId,
                                    @RequestParam("coffeeId") long 
coffeeId) {
		System.out.println("user-agent: " + 
httpServletRequest.getHeader("user-agent"));
        
        return new ResponseEntity<>(new Order(memberId, coffeeId), 
HttpStatus.CREATED);
        }
 }       
```

HttpServletRequest 객체를 이용하면 Request 헤더 정보에 다양한 방법으로 
접근이 가능하다
HttpServletRequest가 다양한 API를 지원하지만, 단순한 특정 헤더 정보에 
접근하고자 한다면 RequestHeader를 이용하는것이 더욱 낫다.

- HttpEntity 객체로 헤더 정보 얻기
HttpEntity는 Request 헤더와 바디 정볼를 래핑하고 있으며, 조금 더 쉽게 
헤더와 바디에 접근할 수 있는 다양한 API를 지원한다

```java
@RestController
@RequestMapping(Path = "/v1/coffees")
public class CoffeeController {
	@postMapping
    public ResponseEntity postCoffee(@RequestHeader("user-agent") String 
userAgent,
    								 
@RequestParam("korName") String korName,
                                     @RequestParam("engName") String 
engName,
                                     @RequestParam("price") int price) {

	System.our.println("user-agent: " + userAgent);
    return new ResponseEntity<>(new Coffee(korName, engName, price), 
HttpStatus.CREATED);
    
  }
  
  @getMapping
  public ResponseEntity getCoffee(HttpEntity httpEntity) {
  	for(Map.Entry<String, String> List<String> entry : 
httpEntity.getHeaders().entrySet()) {
    System.out.println("key: " + entry.getKey()
    			+ ", " + "value: " + entry.getValue());
	}
    
    System.our.println("host: " + httpEntity.getHeaders().getHost());
   	return null;
	}
}   
```

HttpServletRequest 객체를 사용할때와 마찬가지로 Entry를 통해 각각의 헤더 
정보에 접근할 수 있는데, 특이한 것은 자주 사용될만한 헤더 정보들을 
getXXXX()로 얻을 수 있다.

> getXXXX() 메서드는 자주 사용되는 헤더 정보만 얻어 올 수 있으며, 
getXXXX() 메서드로 원하는 헤더 정보를 읽어올 수 없다면 get() 메서드를 
사용해 get("host")와 같이 해당 헤더 정보를 얻을 수 있다.

HttpEntity 출력 결과
```
key: user-agent, value: [PostmanRuntime/7.29.0]
key: accept, value: [*/*]
key: cache-control, value: [no-cache]
key: postman-token, value: [368ad61b-b196-4f75-9222-b9a5af750414]
key: host, value: [localhost:8080]
key: accept-encoding, value: [gzip, deflate, br]
key: connection, value: [keep-alive]
host: localhost:8080
```

## HTTP Response Header 정보 추가
- ResponseEntity && HttpHeaders를 이용해 헤더 정보 추가
```java
@RestController
@RequestMapping(path = "/v1/members")
public class MemberController{
    @PostMapping
    public ResponseEntity postMember(@RequestParam("email") String email,
                                     @RequestParam("name") String name,
                                     @RequestParam("phone") String phone) 
{
        HttpHeaders headers = new HttpHeaders();
        headers.set("Client-Geo-Location", "Korea,Seoul");

        return new ResponseEntity<>(new Member(email, name, phone), 
headers,
                HttpStatus.CREATED);
    }
}
```

ResponseEntity && HttpHeaders 를 이용해 위치 정보를 커스텀 헤더로 추가하고 
있다.

> Use Custom Headers
Http Request에 기본적으로 포함된 헤더 정보는 개발자가 컨트롤 해야 될 
경우가 예상보다 많지 않다.
ResponseEntity && HttpHeaders를 이용해 만든 코드처럼 커스텀 헤더를 종종 
추가해서 부가적인 정보를 전달하는 경우가 있다.

> Naming Custom Header
2012년 이전엔 커스텀 헤더에 'X-'라는 Prefix를 추가하는 것이 관례 였으나, 
문제점이 발생한 가능성이 높아 더 이상 사용하지 않는다.
하지만 커스텀 헤더의 이름을 지을 때, 이 헤더를 사용하는 측에서 헤더의 
목적을 쉽게 이용할 수 있도록 대시(-)를 기준으로 의미가 명확한 용어를 
사용하기를 권장한다
대시(-)를 기준으로 각 단어의 첫 글자를 대문자로 작성하는 것이 관례이지만 
Spring에서 Request Header 정보를 확인할 때, 대/소문자를 구분짓지 않는다

- HttpServletResponse 객체로 헤더 정보 추가하기
```java
@RestController
@RequestMapping(path = "/v1/members")
public class MemberController{
    @GetMapping
    public ResponseEntity getMembers(HttpServletResponse response) {
        response.addHeader("Client-Geo-Location", "Korea,Seoul");

        return null;
    }
}
```

HttpServletResponse를 이용해 위치 정보를 커스텀 헤더로 추가하고 있다.

HttpServletResponse 의 addHeader() 메서드 역시 HttpHeaders 의 set() 
메서드와 메서드 이름만 다를 뿐 헤더 정보를 추가하는 방법은 같다

하나의 차이점을 두고 있는데 HttpHeaders 객체는 ResponseEntity에 포함을 
시키는 처리가 필요하지만 HttpServletResponse 객체는 헤더 정보만 추가할 뿐 
별도의 처리가 필요 없다.

> HttpServletRequest && HttpServletResponse는 Low Level(저수준)의 서블릿 
API를 사용할 수 있기 때문에 복잡한 HTTP Request/Response를 처리하는데 
사용할 수 있다.
반면 ResponseEntity나 HttpHeaders는 Spring에서 지원하는 High Level(고수준) 
API로써 간단한 HTTP Request/Response 처리를 빠르게 진행할 수 있다.
#### 복잡한 처리가 아니라면 코드의 간결성이나 생산성 면에서 가급적 
Spring에서 지원하는 High Level API를 사용하길 권장한다.
