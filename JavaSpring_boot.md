# Spring boot
Spring Boot <span style="color: indianred"> makes it easy</span> 쉽게 
만든다 
to create <span style="color: indianred">stand-alone, </span> 단독적인
<span style="color: indianred">production-grade </span> 상용화 수준의
<span style="color: indianred">Spring based Applications </span> 스프링 
기반 애플리케이션
that you can <span style="color: indianred">"just run".</span> 
# SpringBoot 장점과 특징
과거 스프링 프레임워크는 XML로 설정해야 했고, 외장 톰캣에 war 파일을 
만들어 배포 해야했으며, 설정이 필요한 부분들을 직접 구성하는 등의 
번거로움이 따랐다.

하지만 스프링 부트는 이러한 스프링의 문제점을 완벽히 해결해 손쉽게 웹 
애플리케이션을 만들고 실행할 수 있도록 도와주고 있다.

## 스프링 부트을 사용해야하는 이유
- XML 기반의 복잡한 설계 방식 지양

- 의존 라이브러리의 자동 관리
스프링부트 이전에는 애플리케이션에서 필요한 라이브러리를 사용하기 위해선 
필요한 라이브러리의 이름과 버전을 올바르게 조합해서 추가해 주어야했다.

하지만 스프링 부트의 starter	모듈 구성 기능을 통해 의존 라이브러리를 
수동을 설정해야하는 불편함이 사라졌다.
```java
dependencies {
	implementaion 'org.springframework.boot:spring-boot-starter-web'
	implementaion 'org.springframework.boot:spring-boot-starter-jdbc'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    implementation 'com.h2database:h2'
    }
    // 웹 애플리케이션 개발을 위한 Spring Boot의 의존 라이브러리 설정
```

- 애플리케이션 설정의 자동 구성
스트링 부트는 스타터 모듈을 통해 설치되는 의존 라이브러리를 기반으로 
애플리케이션의 설정을 자동으로 구성한다

개발을 진행하다보면 JSON 메세지 변환, 프로퍼티 설정 등 다양한 공통적인 
설정들이 필요하다
스프링 부트는 개발을 위해 필요한 공통적인 부분들을 자동으로 구성해준다.
그래서 우린 기본적으로 스프링 부트 프로젝트를 만들면 별도의 설정 없이 
서버를 바로 띄울 수 있다.
그 뿐 아니라 스프링부트는 ElasticSearch, Redis, Gson 등과 같은 자주 
사용되는 외부 라이브러리들 역시 자동 설정을 제공해준다.
해당 의존성을 추가하면 클래스 패스 기준으로 의존성이 존재하는지 파악해 
자동으로 설정해준다.

> 예시
- "implementation 'org.springframework.boot:spring-boot-starter-web'" 과 
같은 스타터가 존재한다면 애플리케이션이 웹 애플리케이션이라 추측 후, 웹 
애플리케이션을 띄울 서블릿 컨테이너(defalut: Tomcat) 설정을 자동으로 
구성한다
- "implementation 'org.springframework.boot:spring-boot-starter-jdbc'" 과 
같은 스터타가 존재한다면 애플리케이션에 데이터베이스 연결이 필요하다고 
추측 후, JDBC 설정을 자동으로 구성한다

사용자가 애플리케이션에 대한 설정을 직접하는 번거로움을 최소화 시켜준다.

이런 자동 구성을 활성화 하기 위해선 애너테이션을 코드로 추가해주면 된다
```java
@SpringBootApplication // (1)
public class SampleApplication {
	public static void main(String[] args) {
    	SpringApplication.run(SampleApplication.class, args);
        }
      }  
```
(1)과 같이 @SpringBootApplication 애너테이션을 스프링 어플리케이션 코드에 
추가해주면 스프링 부트에서 자동 구성 설정을 활성화한다




- 프로덕션급 애플리케이션의 손쉬운 빌드
스프링 부트를 사용하면 자신이 개발한 애플리케이션 구현 코드를 손쉽게 
빌드하여 자신이 직접 빌드 결과물을 War 파일 형태로 WAS(Web Application 
Server)에 올릴 필요가 없다.

> what is WAS(Web Application Server) ?
Java 기반의 웹 어플리케이션을 배포하는 일반적인 방식은 개발자가 구현한 
어플리케이션 코드를 WAR(Web pplication ARchive)파일 형태로 빌드한 후에 
WAS(Java에선 서블릿 컨테이너라고도 부름)라는 서버에 배포해서 해당 
어플리케이션을 실행하는 것 (Tomcat 등)

- 내장된 WAS를 통한 손쉬운 배포
스트링 부트는 Apache Tomcat 이라는 WAS를 내장하고 있기 때문에 별도의 WAS를 
구축할 필요 없으며, 스프링 부트를 통해 빌드된 jar 파일을 이용해 명령어 한 
줄만 입력해주면 서비스가 가능한 웹 어플리케이션을 실행할 수 있다.
<code>java -jar <jar 파일명>.jar </code> 명령어를 통해 자신이 만든 
어플리케이션을 손쉽게 실행이 가능하다
  
  

 ## Spring Boot 내장 WAS 종류와 특징
 ### Tomcat
  다양한 성공 사례 및 강력한 커뮤니티를 가지고있어서, 자바 진영에서 실제로 
가장 널리 쓰는 WAS
  스프링 부트에서도 기본 내장 WAS
  
  ```
  <dependency>
    	<groupId>org.springframework.boot</groupId>
    	<artifactId>spring-boot-starter-web</artifactid>
    	</dependency>
  ```
  
  ```
  implementation('org.springframework.boot:spring-boot-starter-web')
  ```
  
### Jetty
경량 WAS
적은 메모리를 사용하며 가벼운 이점이 있고, 속도 역시 빠르다
그렇기 때문에 소형 장비, 소규모 프로그램 등에 내장해 사용하면 좋다

  물론 대규모 트래픽에 취약하다
  
```
  <dependency>
    	<groupId>org.springframework.boot</groupId>
    	<artifactId>spring-boot-starter-web</artifactId>
    	<exclusions>
              <exclusion>
          		<groupId>org.springframework.boot</groupId>
          		
<artifactId>spring-boot-starter-tomcat</artifactId>
              <exclusion>
        </exclusions>
    </dependency>
    
    <dependency>
      		<groupId>org.springframework.boot</groupId>
      		<artifactId>spring-boot-starter-jetty</artifactId>
      </dependency>

```

```
implementation('org.springframework.boot:spring-boot-starter-web') {
          exclude module: 'spring-boot-starter-tomcat'
 }
implementation('org.springframework.boot:spring-boot-starter-jetty')          
```
          
  ### Undertow
Blocking 과 Non-Blocking API를 모두 안정적으로 제공하는 유연한 고성능 
웹서버
    대규모 트래픽으로부터 Tomcat보다 안정적이라 평가 받고있으며
          Spring에서 공식적으로 내장 WAS를 지원한다
          
```
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
	<exclusions>
		<exclusion>
			<groupId>org.springframework.boot</groupId>
			
<artifactId>spring-boot-starter-tomcat</artifactId>
		</exclusion>
	</exclusions>
</dependency>

<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-undertow</artifactId>
</dependency>
```
          
```
implementation('org.springframework.boot:spring-boot-starter-web') {
	exclude module: 'spring-boot-starter-tomcat'
}
implementation('org.springframework.boot:spring-boot-starter-undertow')
   ```
### Netty
Netty는 Async, Event-Driven 방식 네트워크 어플리케이션 프레임워크이다
(Undertow도 Netty 기반)
   스프링 부트 2 부터 Webflux Framework를 사용해 Reactive Programming을 할 
수 있다.
          바로 이 Webflux를 사용하면 기본 내장 WAS는 Netty가 된다
```
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-webflux</artifactId>
</dependency>          
```          
   
          
```
 implementation('org.springframework.boot:spring-boot-starter-webflux')
```          
