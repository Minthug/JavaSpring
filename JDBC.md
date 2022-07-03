# JDBC 
### Java Datebase Connectivity
자바 프로그램이 데이터베이스와 연결되어 데이터를 주고 받을 수 있게 해주는 
프로그래밍 인터페이스

# JDBC는 무엇인가?

![](https://velog.velcdn.com/images/minthug94_/post/da8bb24e-861d-4b02-b384-0731138897da/image.png)

통역자의 역할을 수행 하고 있다
- 응용프로그램과 DBMS간의 통신을 중간에서 번역해주는 역할을 한다.

### JDBC 드라이버(JDBC Driver)
JDBC 드라이버는 데이터베이스와의 통신을 담당하는 인터페이스인데, Oracle || 
MS SQL, MYSQL 같은 다양한 벤더에서는 해당 벤더에 맞는 JDBC 드라이버를 
구현해서 제공을 하게 되고, 이 JDBC 드라이버의 구현체를 이용해 특정 벤더의 
데이터베이스에 액세스 할 수 있다.


![](https://velog.velcdn.com/images/minthug94_/post/a8e348fd-af52-4af2-8a69-b13ffdfe7953/image.png)

1. JDBC 드라이버 로딩
사용하고자 하는 JDBC 드라이버를 로딩한다, JDBC 드라이버는 DriverManager라는 
클래스를 통해 로딩

2. Connection 객체 생성
JDBC 드라이버가 정상적으로 로딩되면 DriverManager를 통해 데이터베이스와 
연결되는 세션인 Connection 객체를 생성

3. Statement 객체 생성
Statement 객체는 저장된 SQL 쿼리문을 실행하기 위한 객체로써 객체 생성 후에 
정적인 SQL 쿼리 문자열을 입력으로 가진다

4. Query 실행
생성된 Statement 객체를 이용해 입력한 SQL 쿼리를 실행

5. ResultSet 객체로부터 데이터 조회
실행된 SQL 쿼리문에 대한 결과 데이터 셋

6. ResultSet 객체 / Statement 객체 / Connection 객체 Close
JDBC API를 통해 사용된 객체들은 사용 이후에도 사용한 순서의 역순으로 차례로 
Close 해주어야한다.

## Connection Pool
그림 삽입 예정

JDBC API를 사용해 데이터베이스와 연결을 위한 Connection 객체를 생성하는 
작업은 비용이 많이 드는 작업 중 하나

따라서 애플리케이션 로딩 시점에 Connection 객체를 미리 생성해두고 
애플리케이션에서 데이터베이스에 연결이 필요할 경우, Connection 객체를 새로 
생성하는것이 아니라 미리 만들어 둔 Connection 객체를 사용함으로써 
애플리케이션 성능을 향상 시킬 수 있다.

이처럼 데이터베이스 Connection을 미리 만들어서 보관하고 애플리케이션이 
필요할 때 이 Connection을 제공해주는 역할을 Connection 관리자를 바로 
Connection Pool 이라 한다.

## 데이터 액세스 기술 유형
Spring에선 데이터베이스에 액세스하는 다양한 기술들을 활용할 수 있다.

Spring에서 대표적인 데이터 액세스 기술
- mybatis
- Spring JDBC
- Spring Data JDBC
- JPA
- Spring Data JPA 등

## SQL 중심 기술
mybatis && Spring JDBC 는 대표적인 SQL 중심 기술이다

즉, SQL 중심 기술은 애플리케이션에서 데이터베이스에 접근하기 위해 SQL 
쿼리문을 애플리케이션 내부에 직접적으로 작성하는 것이 중심이 되는 기술

```sql
<select id="findMember" resultType="Member">
	SELECT * FROM MEMBER WHERE member_id = #{memberId}
    </select>
```

mybatis에서 사용되는 SQL Mapper의 예이다

mybatis의 경우, SQL Mapper 라는 설정 파일이 존재하는데 이 SQL Mapper 에서 
SQL 쿼리문을 직접적으로 작성한다

작성된 SQL 쿼리문을 기반으로 데이터베이스의 특정 테이블에서 데이터를 조회한 
후, Java 객체로 변환해주는 것이 mybatis의 대표적인 기술적 특징

```java
Member member = this.jdbcTemplate.queryForObject(
										
"select * from member where member_id=?", 1, Member.class);
```

Spring JDBC의 jdbcTemplate 이라는 템플릿 클래스를 사용한 데이터베이스 접근의 
예

> 이처럼 SQL 쿼리문이 직접적으로 포함되는 방식은 과거부터 많이 사용한 
방식이고 현재도 사용이 되긴하지만 Java 진영에서는 SQL 중심의 기술에서 객체 
중심의 기술로 지속적으로 이전을 하고 있는 추세이다

## 객체 중심 기술
객체 중심 기술은 데이터를 SQL 쿼리문 위주로 생각하는 것이 아닌 모든 데이터를 
객체 관점으로 바라보는 기술

즉, 객체 중심 기술은 데이터베이스에 접근하기 위해서 SQL 쿼리문을 직접적으로 
작성하기 보단 데이터베이스의 테이블에 데이터를 저장하거나 조회할 경우, Java 
객체를 이용해 애플리케이션 내부에서 이 Java 객체를 SQL 쿼리문으로 자동 
변환한 후 데이터베이스의 테이블에 접근한다

이런 객체 중심의 데이터 액세스 기술을 ORM(Obejct-Relational Mapping)이라 
한다

>자바에서 대표적인 ORM 기술이 바로 JPA(Java Persistence API) 이다.








