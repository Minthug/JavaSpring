# ResponseEntity
ResponseEntity는 HttpEntity의 확장 클래스로써 HttpStatus 상태 코드를 
추가한 전체 HTTP 응답 (상태 코드, 헤더 및 본문)을 표현하는 클래스이다.

## 어디서 사용할 수 있나?
ResponseEntitiy 클래스는 주로 @Controller || @RestController 애너테이션이 
붙은 Controller 클래스의 핸들러 메서드에서 요청 처리에 대한 응답을 
구성하는데 사용된다.

Used in RestTemplate as wall as in @Controller methods

@RestTemplate으로 외부의 API 통신에 대한 응답을 전달 받아서 처리할 경우, 
역시 ResponseEntity를 사용해야한다

## 어떻게 사용하나?
가장 일반적인 방식은 new 로 ResponseEntity 객체를 생성하는 방식

```java
@RestController
@RequestMapping("/v1/coffee")
public class ResponseEntityExam {
	@PostMapping
    public ResponseEntity postCoffee(Coffee coffee) {
    
    //	coffee 정보 저장
    
    return new ResponseEntity<>(coffee, HttpStatus.CREATED);
```

ResponseEntity 객체를 생성하면서 응답의 body 데이터와 HttpStatus의 상태를 
전달하고 있다.

```java
@RestController
@RequestMapping("/v1/coffee")
public class ResponseEntityExam {
	@GetMapping("/{coffee-id}")
    public ResponseEntity getCoffee(@PathVariable("coffee-id") long 
coffeeId)
    
    if(coffeeId > 0 ) {
    	return new ResponseEntity<>(
        		"coffeeId should be greater than 0 ",
                HttpStatus.BAD_REQUEST);
       }
       
       return new ResponseEntity<>(new coffee(), HttpStatus.OK);
    }
}
```

coffeeId에 따라 HttpStatus 상태를 동적으로 지정하는 예

> 📢 두번째 코드는 요청 데이터에 대한 유효성 검사가 API 계층에 포함에 직접 
포함 되어 있는데, 이는 좋은 개발 방식이 아니다.
Handler Method는 단순히 클라이언트의 요청을 전달 받고, 처리된 응답을 다시 
클라이언트에게 전달하는 역할만 하도록 로직을 단순화 하는 것이 좋으며, 
유효성 검사나 비즈니스 처리 등의 로직은 별도의 메서드 또는 클래스로 분리를 
하는 것이 좋다.

```java
@RestController
@RequestMapping("/v1/coffee")
public class ResponseEntityExam {
	@GetMapping("/{coffee-id}")
    public ResponseEntity getCoffee(@PathVariable("coffee-id") long 
coffeeId) {
    
    if(coffeeId > 0 ) {
    	return new ResponseEntity<>(
        		"coffeeId should be greater than 0",
                HttpStatus.BAD_Request);
    }
    
    HttpHeader header = new HttpHeaders();
    headers.add("Custom-Header", "bar");
    return new ResponseEntity<>(new Coffee(), headers, HttpStatus.OK);
    }
}    
```

만약 ResponseEntity에 Custom header를 포함하고 싶다면 세번째 코드와 같이 
HttpHeaders에 원하는 header를 추가하고 ResponseEntity의 생성자로 headers 
객체를 전달하면 된다.

## BodyBuilder를 이용한 메서드 체인 방식
ResponseEntity 클래스의 생성자로 body, header, HttpStatus 등을 추가하는 
방식과 달리 BodyBuilder 클래스를 이용하면 각각의 항목들을 메서드 체인 
방식으로 전달할 수 있다.

```java
@RestController
@RequestMapping("/v1/coffee")
public class ResponseEntity<Coffee> postCoffee(Coffee coffee) {

	long savedCoffeeId = 1L;
    return ResponseEntity.created(URI.create("/members/" + savedCoffeeID) 
    }
}    
```

ResponseEntity.BodyBuilder를 이용해 메서드 체인 방식으로 ResponseEntity 
객체를 리턴해주고 있다.

> created() 메서드의 경우 URI 를 지정할 수 있는데, 이는 새롭게 생성된 
리소스에 대한 접근 URI를 Location 헤더 값으로 포함시킴으로써 클라이언트 
쪽에서 이 정보를 이용해 해당 리소스에 접근할 수 있도록 한다

```java
@RestController
@RequestMapping("/v1/coffees")
public class ResponseEntityExample02 {
    @GetMapping("/{coffee-id}")
	public ResponseEntity getCoffee(@PathVariable("coffee-id") long 
coffeeId) {
    
    if(coffeeId > 0) {
    	return new ResponseEntity.badRequest().body(new Coffee());
    }
    
    HttpHeaders headers = new HttpStatus();
    headers.add("Custom-Header", "bar");
    return ResponseEntity.ok().headers(headers).body(new coffee());
    }
}    
```

BodyBuilder 를 이용해 메서드 체인 방식을 변경함


