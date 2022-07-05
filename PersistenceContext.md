
# 영속성 컨텍스트 특징
- 영속성 컨텍스트와 식별자 값
영속성 컨텍스트는 Entity를 식별자 값 (@Id로 테이블의 기본 키와 매핑한 
값)으로 구분한다
즉, 영속 상태는 식별자 값이 반드시 있어야 한다.

- 영속성 컨텍스트와 데이터베이스 저장
JPA는 보통 트랜잭션을 커밋하는 순간 영속성 컨텍스트에 새로 저장된 Entity를 
데이터베이스에 반영한다
이것을 **flush** 라 한다.

- 영속성 컨텍스트가 Entity를 관리하면 얻게되는 장점
   - 1차 캐시
   - 동일성(identity) 보장
   - 트랜잭션을 지원하느 쓰기 지연 (transactional write-behind)
   - 변경 감지 (Dirty Checking)
   - 지연 로딩 (Lazy Loading)

## Entity 조회, 1차 캐시
![](https://velog.velcdn.com/images/minthug94_/post/c97cfb04-1ed6-41ee-bca9-3f1bfa44ef0d/image.png)

```java
Member member = new Member();
member.setId(1L);
member.setUsername("회원1");

em.persist(member);
```
em.persist(member) 메소드를 실행하게 되면 member 엔티티가 영속 상태가 
된다.
위 이미지 예제처럼 엔티티는 영속성 컨텍스트의 1차 캐시에 저장된다.

### 계속 언급되는 1차 캐시
1차 캐시란, 영속성 컨텍스트는 내부의 캐시를 가지고 있는데 이것을 '1차 
캐시'라 부른다.
영속 상태의 엔티니는 모두 이곳에 저장되고있으며, 쉽게 말하면 영속성 
컨텍스트 내부의 Map 이 하나 있는데 (1차 캐시), 
키는 @Id 로 매핑한 식별자고 값은 Entity 인스턴스 이다.

#### 그럼 왜 1차 캐시가 존재하므로써 얻는 장점이 있는가?
```java
Member member = new Member();
member.setId(1L);
member.setUsername("회원1");

// 1차 캐시에 저장
em.persist(member);

// 1차 캐시에서 조회
Member member = em.find(Member.class, 1L)
```
#### Entity를 조회하는 find()가 실행되면
1. 1차 캐시에서 식별자 값으로 엔티티를 찾는다
2. 만약 찾는 엔티티가 1차 캐시에 있으면 데이터베이스를 조회하지 않고, 
메모리에 있는 1차 캐시에서 엔티티를 조회한다.
3. 1차 캐시에 찾는 엔티티가 없다면 데이터베이스에서 조회한다.

![](https://velog.velcdn.com/images/minthug94_/post/c9b3ec83-c280-495d-af5f-b1bd8b5d5b61/image.png)

#### 데이터베이스에서의 조회
em.find() 메서드를 호출했는데 엔티티가 1차 캐시에 없으면 EntityManager는 
데이터베이스를 조회해서 엔티티를 생성한다.
그리고 1차 캐시에 저장한 후 영속 상태의 엔티티를 반환한다.
![](https://velog.velcdn.com/images/minthug94_/post/cd583f81-b6d9-4c92-b33c-10b771c727ec/image.png)

순서를 보자
em.find(Member.class, 1L) 를 실행한다
1차 캐시에는 현재 1L만 존재하고 2L은 없는 상태이므로 EntityManager는 
데이터베이스에서 2L을 조회한다
조회한 데이터로 2L Entity를 생성해 1차 캐시에 저장한다 (영속 상태)
조회한 엔티티를 반환한다

위 순서 후 1L이나, 2L 에 대해 find를 요청하면 데이터베이스를 접근하지 
않아도, 1차 캐시에 있는 엔티티를 반환할 것이다

### 1차 캐시로 인해 데이터베이스에 접근할 빈도가 줄어들어 성능향상의 
이점을 얻을 수 있다.

## 영속 엔티티의 동일성 보장
```java
Member a = em.find(Member.class, 1L);
Member b = em.find(Member.class, 1L);

System.out.println(a == b);
```
현재 1L이 영속성 컨텍스트에 존재하는 상황에서 <code>a == b</code> 참이 
될까? 
대답은 참이 된다.

영속성 컨텍스트는 1차 캐시에 있는 같은 엔티티 인스턴스를 반환하기 때문에 
참이 된다.
즉, 영속성 컨텍스트는 성능상 이점과 엔티티의 동일성을 보장한다.

## 트랜잭션을 지원하는 쓰기 지연
```java
EntityManager em = em.createdEntityManager();
EntityTransaction transaction = em.getTransaction();
// EntityManager는 데이터 변경 시 트랜잭션을 시작해야 한다

transaction.begin(); // 트랜잭션 시작 메서드

em.persist(memberA);
em.persist(memberB);

transaction.commit(); // 트랜잭션 커밋
```

영속성 컨텍스트에 등록하는 코드이다.
엔티티 매니저는 트랜잭션을 커밋하기 직전까지 데이터베이스에 엔티티를 
저장하지 않고, 내부 쿼리 저장소에 INSERT 쿼리문을 모아둔다.
그리고 트랜잭션을 커밋할 때, 모아둔 쿼리문을 데이터베이스에 보내서 
저장시킨다.
이를 '트랜잭션 지원하는 쓰기 지연' 이라 한다.

트랜잭션을 커밋하면, 엔티티 매니저는 영속성 컨텍스트를 flush 한다
flush는 영속성 컨텍스트의 변경 내용을 데이터베이스에 동기화하는 작업인데 
이때 등록, 수정, 삭제한 엔티티를 데이터베이스에 반영한다.

## 엔티티 수정
SQL을 사용하면 수정 쿼리문을 직접 작성해야한다, 그런데 프로젝트의 규모가 
커지고 요구사항이 늘어나면서 수정 쿼리문도 점점 추가될 것이다.
이런 방식의 문제점은 수정 쿼리문이 많아지는 것은 물론이고 비즈니스 로직을 
분석하기 위해 SQL을 계속 확인 해야한다.
결국 직간접적으로든 비즈니스 로직이 SQL에게 의존하게 된다.

```java
EntityManager em = em.createEntityManager();
EntityTransaction transaction = em.getTransaction();
transaction.begin();

Member memberA = em.find(Member.class, memberA);

memberA.setUsername("member5");
memberA.setAge(10);

transaction.commit();
```
JPA 엔티티 수정 코드이다
em.update() 같은 메서드 없이 memberA에 대한 setter로만 변경을 했는데 
데이터베이스의 엔티티는 수정되었다
그 이유는 JPA가 변경 감지(Dirty Checking ) 기능이 있기에 가능한 것이다.
변경 감지는 엔티티의 변경사항을 데이터베이스에 자동으로 반영하는 기능이다!

> 변경 감지는 영속성 컨텍스트가 관리하는 영속 상태의 엔티티에만 적용된다.
비영속, 준영속 상태의 엔티티들은 영속성 컨텍스트의 관리를 받지 못해 값을 
수정해도 데이터베이스에 반영되지 않는다.

변경 감지로 인해 실행된 UPDATE SQL의 쿼리는 변경된 부분만 수정 쿼리가 생성 
되나?
대답은, 아니다

JPA의 기본 전략은 엔티티의 모든 필드를 업데이트 한다, 모든 필드를 
업데이트하면 DB에 보내는 데이터 전송량이 증가하는 단점이 얻게되지만
아래의 장점으로 모든 필드를 업데이트 한다

- 모든 필드를 사용하면 수정 쿼리가 항상 같다.
즉, 애플리케이션 로딩 시점에 수정 쿼리를 미리 생성해두고 재사용 할 수 
있다.
- 데이터베이스에 동일한 쿼리를 보내면 데이터베이스는 이전에 한 번 파싱된 
쿼리를 재사용 할 수 있다.


## 엔티티 삭제
```java
Member memberA = em.find(Member.class, memberA);
em.remove(memberA);
```
remove() 메서드에 삭제할 엔티티를 넘겨주면 엔티티를 삭제한다
메서드를 호출하자마자 삭제되는 것이 아닌, 엔티티 등록과 비슷하게 삭제 
쿼리르 쓰기 지연 SQL 저장소에 등록한다
이 후 트랜잭션을 커밋해 flush를 호출하면 DB에 삭제 쿼리르 전달한다
(em.remove(memberA)는 호출하는 순간 영속성 컨텍스트에서 제거된다)

## flush
flush()는 영속성 컨텍스트의 변경 내용을 DB에 반영한다.
플러시를 실행 과정을 보자
1. 변경 감지가 동작해 영속성 컨텍스트에 있는 모든 엔티티를 스냅샷과 
비교해서 수정된 엔티티를 찾는다.
2. 수정된 엔티티는 수정 쿼리를 만들어 쓰기 지연 SQL 저장소에 등록
3. 쓰기 지연 SQL 저장소의 쿼리를 DB에 전송(등록, 삭제, 수정 쿼리)

> '스냅샷'이란?
JPA는 엔티티를 영속성 컨텍스트에 보관할 때, 최초 상태를 복사해서 
저장해두는데 이 작업을 스냅샷 이라 한다.

## 영속성 컨텍스트를 flush 하는 3가지 방법
- em.flush()
엔티티 매니저의 flush() 메서드를 직접 호출해 영속성 컨텍스트를 강제로 
플러시한다.

- commit()
DB의 변경 내용을 SQL로 전달하지 않고, 트랜잭션만 커밋하면 어떤 데이터도 
DB에 반영되지 않는다.
즉, 트랜잭션을 커밋하기 전 꼭 flush()를 호출해 영속성 컨텍스트의 변경 
내용을 DB에 반영해야 한다.
JPA는 이런 문제를 예방하기 위해 트랜잭션을 커밋할 때 플러시를 자동으로 
호출한다.

- JPQL 쿼리 실행
JPQL || Criteria 같은 객체 지향 쿼리를 호출할 때도 플러시가 자동 실행한다.

### flush mode option
- FlushModeType.AUTO : 커밋이나 쿼리를 실행할 때 default 값
- FlushModeType.COMMIT : 커밋할 때만 flush
JPQL 쿼리 실행 시 플러시가 자동으로 일어나는데, 만약 JPQL에서 사용할 
테이블이 이전 작업들과 전혀 관련이 없다면
이 옵션을 통해 플러시를 커밋할 때로 변경할 수 있다.

### 플러시를 정리하면
영속성 컨텍스트를 비우지는 않는다
영속성 컨텍스트의 변경 내용을 DB에 동기화한다.
트랜잭션이라는 작업 단위가 중요하다 -> 커밋 직전에 동기화하면 된다.

-----------

## 알아두기 : EntityManager && EntityManagerFactory
![](https://velog.velcdn.com/images/minthug94_/post/b3efcf9b-cf01-4060-9686-a9fc5321c0aa/image.png)
[출처: 
https://velog.io/@conatuseus/2019-09-06-0009-%EC%9E%91%EC%84%B1%EB%90%A8-cfk06vdfm9]

- 새로운 고객의 요청이 올때마다 엔티티 매니저 팩토리는 엔티티 매니저를 
생성
- 엔티티 매니저는 내부적으로 데이터베이스 커넥션을 사용해서 DB를 사용

### EntityManagerFactory
- 엔티티 매니저를 만드는 공장
- 엔티티 매니저 팩토리는 생성하는 비용이 커서 한 개만 만들어 어플리케이션 
전체에서 공유한다
- 여러 스레드가 동시에 접근해도 안전하다

### EntityManager
- 엔티티의 CRUD 등 Entity와 관련된 모든 일을 처리한다.
- 엔티티 매니저는 여러 스레드가 동시에 접근하면 동시성 문제가 발생해 
스레드 간의 절대 공유해선 안된다.

# Persistence Context ?
Entity를 영구 저장하는 환경 이라는 뜻이다.

> EntityManager.persist(member);

위 코드는 member 엔티티를 저장하는 코드이다.
데이터베이스에 저장하는게 아닌 엔티티 매니저를 사용해 회원 엔티티를 영속성 
컨텍스트에 저장하는 코드

영속성 컨텍스트는 엔티티 매니저를 생성할 때 하나만 생성된다, 엔티티 
매니저를 통해 영속성 컨텍스트에 접근할 수 있고, 영속성 컨텍스트를 관리할 
수 있다.

------------------------------------

# Entity lifeCycle, 엔티티 생명주기
Entity의 4가지 상태

- 비영속(new/transient) : 영속성 컨텍스트와 전혀 관계가 없는 새로운 상태
- 영속(managed) : 영속성 컨텍스트에 관리되는 상태
- 준영속(detached) : 영속성 컨텍스트에 저장되었다가 분리된 상태
- 삭제(removed) : 삭제된 상태

### 비영속 (new / transient)
Entity object가 생성된 순수 객체 상태
아직 영속성 컨텍스트나 데이터베이스와는 전혀 관계가 없는 상태이다

![](https://velog.velcdn.com/images/minthug94_/post/fda8d55f-104e-47af-9200-ba7f8ce119dd/image.png)

### 영속(managed)
EntityManager를 통해 엔티티를 영속성 컨텍스트 저장하면, 영속석 컨텍스트가 
엔티티를 관리해 영속 상태가 된다.

![](https://velog.velcdn.com/images/minthug94_/post/9d569c35-9d35-4dc2-b8ed-a0932cab19d3/image.png)


### 준영속(detached)
영속석 컨텍스트가 관리하던 영속 상태의 엔티티를 영속성 컨텍스트가 관리하지 
않으면 준영속 상태가 된다.

> ### 어떻게 준영속 상태를 만드는가?
- EntityManager.detach(entity) : 엔티티를 준영속 상태로 만든다
- EntityManager.close() : 영속성 컨텍스트를 닫는다
- EntityManager.clear() : 영속성 컨텍슽트를 초기화 시킨다.

### 삭제(removed) 
엔티티를 영속성 컨텍스트와 데이터베이스에서 삭제한다

> EntityManager.remove(entity)

# Entity lifecycle
![](https://velog.velcdn.com/post-images%2Fconatuseus%2F3861eed0-d482-11e9-9b0f-dd1a4f570095%2Fimage.png)


--------------
참고
https://velog.io/@conatuseus/2019-09-06-0009-%EC%9E%91%EC%84%B1%EB%90%A8-cfk06vdfm9


