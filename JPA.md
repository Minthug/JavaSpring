# JPA
JPA (Java Persistence API) 자바 진영에서 사용하는 ORM(Object-Relational 
Mapping) 기술의 표준 사양(또는 명세, Specification)이다

Persistence - 영속성


>표준 사양이란?
자바의 인터페이스로 사양이 정의되어 있기 때문에 JPA라는 표준 사양을 구현한 
구현체는 따로 있다는 것을 뜻한다.
.
정리하면 JPA를 학습한다는 JPA 표준 사양을 구현한 구현체에 대해서 
학습하다라고 정리할 수 있다.

## "JPA는 기술 명세이다"
JPA는 Java Persistence API의 약자로, 자바 어플리케이션에서 관계형 
데이터베이스를 사용하는 방식을 정의한 인터페이스
요점은 JPA는 말 그대로 '인터페이스' 라는 점이다, JPA의 특정 기능을 하는 
라이브러리가 아니고 마치 일반적인 백엔드 API가
클라이언트가 어떻게 서버를 사용해야 하는지를 정의한 것처럼, JPA 역시 자바 
어플리케이션에서
관계형 데이터베이스를 어떻게 사용해야 하는지를 정의하는 방법일 뿐이다.

JPA는 단순히 명세이기 때문에 구현이 없다,
JPA를 정의한 <code>javax.persistence</code> 패키지의 대부분은 
<code>interface</code>, <code>enum</code>, <code>Exception</code> 그리고 
각종 Annotation으로 이루어져 있다
예를 들어, JPA의 핵심을 이루는 <code>EntityManager</code> 
javax.persistence.EntityManager 라는 파일에 인터페이스로 정의되어있다.
```java
package javax.persistence;

import ...

public interface EntityManager {

    public void persist(Object entity);

    public <T> T merge(T entity);

    public void remove(Object entity);

    public <T> T find(Class<T> entityClass, Object primaryKey);

    // More interface methods...
}
```

## Hibernate ORM
JPA 표준 사양을 구현한 구현체로는 Hibernate ORM, EclipseLink, DateNucleus 
등이 있다

현재는 Hibernate ORM에 대해 학습할 것이다.

Hibernate ORM은 JPA에서 정의해둔 인터페이스를 구현한 구현체로써 JPA에서 
지원하는 기능 이외에 Hibernate 자체적으로 사용할 수 있는 API 역시 지원하고 
있다.

> JPA는 Java Persistence API의 약자지만, 현재는 Jakarta Persistence 라 
불린다

# 데이터 액세스 계층에서의 JPA 위치
![](https://velog.velcdn.com/images/minthug94_/post/761456c9-ca81-4bb3-a367-6f77355f4a37/image.png)
[출처: https://suhwan.dev/2019/02/24/jpa-vs-hibernate-vs-spring-data-jpa/]


![](https://velog.velcdn.com/images/minthug94_/post/df5f20bc-c459-42af-b476-6d57942a6eb4/image.png)


데이터 액세스 계층에서 JPA는 데이터 액세스 계층의 상단에 위치한다
데이터 저장, 조회 등의 작업은 JPA를 걸쳐 Hibernate ORM을 통해서 
이루어지며, Hibernate ORM은 내부적으로 JDBC API를 이용해 데이터베이스에 
접근한다.


### 영속성 컨텍스트 (Persistence Context)
ORM은 객체와 데이터베이스 테이블의 매핑을 통해 엔티티 클래스 객체 안에 
포함된 정보를 테이블에 저장하는 기술이다.
JPA에선 테이블과 매핑되는 엔티티 객체 정보를 '영속성 컨텍스트' 라는 곳에 
보관해서 애플리케이션 내에서 오래 지속되도록 한다.

그리고 이렇게 보관된 엔티티 정보는 데이터베이스 테이블에 데이터를 저장, 
수정, 조회, 삭제하는데 사용된다.

[자세히 알아보기 ](https://velog.io/@minthug94_/Persistence-Context)





---------------
## 간단하게 이해하기
### JPA
자바에서 제공하는 인터페이스로 ORM 기술에 대한 명세서
관계형 데이터베이스를 사용하는 방식을 나타내고 있다.
ORM 이므로 자바 클래스와 DB table을 Mapping 한다

### ORM
객체와 관계와의 설정이다

객체란? OOP에서 말하는 객체이며 관계는 관계형 데이터베이스 사용하는 그 
관계 이다.
ORM을 통해 관계형 데이터베이스의 관계를 객체에 반영하여 조금 더 객체 
지향에 근접한 프로그래밍을 위해 나온 기술

### Hibernate
Hibernate는 JPA 구현체의 한 종류,
JPA는 자바에서 제공하는 인터페이스로 ORM 기술에 대한 명세서 라고 했다.

Hibernate를 사용해 SQL을 직접 작성하지 않고, 메서드로 작성한다고 해서 JDBC 
API를 사용하지 않는 것이 아니다.
그 메서드 내부에는 JDBC API 와의 동작이 연결되어 있으며, 개발자가 SQL에 
대한 작업보다는 비즈니스 로직에 더 집중할 수 있도록 도와주는 역할을 맡고 
있다.







