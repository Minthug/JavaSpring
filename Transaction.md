# Transaction
이 글은 트랜잭션을 제대로 이해하고 정리하기위해 작성하는 글이다

### Transaction mechanism

![](https://velog.velcdn.com/images/minthug94_/post/7c89394a-9272-4910-8339-846ead92b2ac/image.png)

```sql
BEGIN TRAN
UPDATE accounts
SET balance = balance - 10000
WHERE user = '구매자'
SET balance = balance + 10000
WHERE user = '판매자'
COMMIT TRAN
```
### 과정을 보자

1. UPDATE accounts SET balance = balance - 10000 WHERE user = '구매자' 
쿼리 실행
2. 업데이트에 필요한 데이터를 데이터 캐시에 요청, 데이터 캐시에 해당 
데이터가 없다
3. 데이터 파일에서 데이터를 가져와야 한다
4. 그리고 데이터 캐시에 필요한 데이터가 로드 된다.
![](https://velog.velcdn.com/images/minthug94_/post/394fe2ad-27f9-44ea-9b4a-8adc94b2cc7a/image.png)
5. 데이터가 로드 된 후, 업데이트를 하면 되는데 그 전에 로그 캐시에 로그를 
기록해야 한다
6. ReDO 로그와 UnDO 로그에 기록
- ReDO 로그
    - 변경 후의 값을 기록
    - ```sql
    트랜잭션_1 START
    트랜잭션_1 UPDATE accounts 구매자.balance 0 
    ```
- UnDo 로그
   - 변경 전의 값을 기록
   - 
   ```sql
   로그_1 accounts 구매자.balance 10000
   ```
7. 로그 기록 후 데이터 캐시에 있는 값을 변경하면 된다
![](https://velog.velcdn.com/images/minthug94_/post/342ed8b4-866e-434d-904c-16582ad7fbe6/image.png)
8. UPDATE accounts SET balance = balance + 10000 WHERE user = '판매자' 
쿼리 실행
9. 판매자의 데이터가 없으므로 데이터 파일에서 데이터 캐시로 로드
![](https://velog.velcdn.com/images/minthug94_/post/cf7c3c82-44ee-4def-908c-0951be813909/image.png)

10. ReDo 로그와 UnDo 로그에 기록
- ReDo
   - 
   ```sql
    트랜잭션_1 START
    트랜잭션_1 UPDATE accounts 구매자.balance 0
    트랜잭션_1 UPDATE accounts 판매자.balance 10000
   ```
- UnDO
  - 
  ```sql
  로그_1 accounts 구매자.balance 10000
  로그_1 accounts 판매자.balance 0
  ```
11. 데이터 캐시의 데이터 업데이트
![](https://velog.velcdn.com/images/minthug94_/post/f4d7e960-2155-490f-a376-dc11620a1394/image.png)

12. 트랜잭션의 한 단위 끝


#### 오류

하나의 작업으로 이루어지는 여러 쿼리들을 트랜잭션이라는 논리적인 하나의 
작업 단위로 묶어 쿼리들이 한번에 작업되거나 아예 동작 하지 않거나 할수있다

트랜잭션은 사용자 혹은 시스템상의 실수가 있더라도 데이터베이스가 데이터를 
안정적으로 보장할 수 있도록 한다

하나의 트랜잭션이 모두 실행되거나 아무 쿼리도 실행 되지 않는것을 '커밋' 
혹은 '롤백'이라 한다

- 커밋이란?
일종의 확인 도장으로 트랜잭션 묶인 모든 쿼리가 성공되어 트랜잭션 쿼리 
결과를 실제 디비에 반영하는 것이고 

- 롤백은?
쿼리실행 결과를 취소하고 디비를 트랜잭션 이전 상태로 되돌리는 것

### 데이터 롤백 시 UnDo 로그를 통해 롤백한다
- UnDo를 통해 역순으로 기록을하게 되면 데이터가 이전 상태로 복구된다.
   - ```sql
   로그_1 accounts 구매자.balacne 10000
   로그_1 accounts 판매자.balance 0
   ```
   
![](https://velog.velcdn.com/images/minthug94_/post/9ca0ba0e-7808-4fb1-ab85-7f8bc28aa3ab/image.png)

### 예상치 못한 오류 발생 시 ReDo 로그와 UnDo 로그를 통해 복구
- ReDo 로그를 순차적으로 실행 해서 데이터들을 다시 일관성있게 만들어 준다.
   - ```sql
   트랜잭션_1 START
   트랜잭션_1 UPDATE accounts 구매자.balance 0
   트랜잭션_1 UPDATE accounts 판매자.balance 10000
   ```
- UnDo 로그를 역순으로 실행해서 다시 커밋이 되지 않은 것들을 이전 상태로 
돌린다
    -  ```sql
    로그_1 accoutns 구매자.balance 10000
    로그_1 accoutns 판매자.balance 0
    ```

### 간단하게 보는 ACID
Atomicity 원자성
트랜잭션은 DB에 모두 반영되거나, 전혀 반영되지 않아야 한다
(= 완료되지 않은 트랜잭션의 중간 상태를 DB에 반영해선 안된다)
예시) 구매자에 계좌에서 돈이 빠져나갔는데, 판매자에 계좌에 돈이 입금되지 
않는 일은 없을것이다

Consistency 일관성
트랜잭션 작업처리결과는 항상 일관성 있어야 한다, DB는 항상 일관된 상태로 
유지되어야한다
(= DB에 여러 제약 조건에 맞는 상태를 보장해준다)
예시) 마이너스 통장을 허락하지 않는 조건이 있다면, DB의 조건이 있다면 
조건에 위배될시 트랜잭션 종료

Isolation 독립성
둘 이상의 트랜잭션이 동시 실행되고 있을 때, 어떤 트랜잭션도 다른 트랜잭션 
연산에 끼어들 수 없다.
(=각각의 트랜잭션은 서로 간섭 없이 독립적으로 이루어져야 한다)
예시) 구매자의 계좌에서 돈이 빠져나가고, 판매자의 계좌에 돈이 들어가지 
않은 DB 상황을 다른 트랜잭션이 조회하면 안된다

Durability 지속성
트랜잭션이 성공적으로 완료되었으면 결과는 영구히 반영되어야 한다
예시) 한번 송금이 성공하면 은행 시스템에 문제가 생기더라도 송금이 성공한 
상태로 복구할 수 있어야 한다

ACID 성질은 이론적으로 보장해야하는 성질이고, 실제로는 성능을 위해 
손실보장이 완화되기도 한다


### 트랜잭션 격리 수준
동시에 DB에 접근할 때 그 접근을 어떻게 제어할지에 대한 설정

![](https://velog.velcdn.com/images/minthug94_/post/48783bca-4191-4131-a44c-7e5e18210e87/image.png)

밑으로 갈수록 격리 수준이 높아지지만 성능은 떨어진다
(데이터 정합성과 성능이 반비례한다, 케이스에 맞게 잘 선택하자)


- READ-UNCOMMITTED
커밋 전의 트랜잭션의 데이터 변경 내용을 다른 트랜잭션이 읽는것을 허용

- READ-COMMITTED
커밋이 완료된 트랜잭션의 변경사항만 다른 트랜잭션에서 조회 가능

- REPEATABLE-READ
트랜잭션 범위 내에서 조회한 내용이 항상 동일한 내용 보장

- SERIALIZABLE
한 트랜잭션에서 사용하는 데이터를 다른 트랜잭션에서 접근 불가

![](https://velog.velcdn.com/images/minthug94_/post/c7ccf35d-2df1-4f42-ac10-176d99f6c3c0/image.png)



> ⚠️ Dirty Read
Dirty page(메모리엔 변경 되었지만 디스크엔 아직 변경이 되지않은 데이터)에 
있는 데이터를 검색, 커밋되지 않은 데이터를 Read 하기 때문에 Dirty Read 후 
Dirty page가 롤백 되어 잘못된 데이터를 읽어온 상태가 된다
(이미지 예시)
트랜잭션 A가 만약 트랜잭션을 끝마치지 못하고 롤백 한다면 트랜젝션 B는 
무효가 된 데이터 값을 읽고 처리를 하기 때문에 문제 발생
![](https://velog.velcdn.com/images/minthug94_/post/6d682743-662f-4e73-bcc1-491215ab9f3a/image.png)


> ⚠️ NON-REPEATABLE READ
하나의 트랜젝션에서 같은 쿼리를 두 번 이상 수행 시, 똑같은 쿼리문임에도 
다른 결과를 나타내는 현상
(위의 이미지 예시)
같은 트랜잭션 내에서 READ 시 값이 다르게 나오는 데이터 불일치 문제
트랜잭션 중 데이터가 변경되면 문제가 발생할 수 있다.
![](https://velog.velcdn.com/images/minthug94_/post/1dea0c42-887f-4c73-808d-25cc26e5c64a/image.png)

> ⚠️ Phantom Read
NON-REPEATABLE READ 의 한 종류 이며,
하나의 트랜젝션에서 일정 범위의 레코드를 두 번 이상 읽어올때, 똑같은 
쿼리문임에도 첫 번째 쿼리에서 없던 레코드가 두번째 쿼리에서 나타나는 현상
![](https://velog.velcdn.com/images/minthug94_/post/b842b47e-d79c-455a-9f52-a22a7ed5dd66/image.png)

### Transaction Propagation (트랜잭션 전파 타입)
- 트랜잭션의 경계에서 트랜잭션이 어떻게 동작할 것인가 결정하는 것

```java
public class ServiceA {
	private ServiceB serviceB;
    ...
    
    @Transactional
    public void a() {
    serviceB.b();
   	  }
  	}  
   
	public class ServiceB {
    @Transactional
    public void b() {...}
	}
```

![](https://velog.velcdn.com/images/minthug94_/post/30222ff4-0315-441e-84d3-5b128f464f84/image.png)

- ### Timeout
```sql
@Transactional(timeout=10)
```
- 초 단위로 트랜잭션 제한시간을 설정하는 속성
- 설정된 시간이 지나면 예외가 발생하며 롤백
- 따로 설정하지 않으면 timeout 속성은 지정되어 있지 않다

- ### readOnly
```sql
@Transactional(readOnly=true)
```
- 트랜잭션 작업 내에서 update, insert, delete 작업이 일어나는 것을 
방지한다
- 해당 옵션을 적용하면 flush 모드가 manual로 설정되어 JPA의 DirtyChecking 
기능을 무시해 성능 향상에 도움을 준다

- ### rollbackFor
```sql
@Transactional(rollbackFor=NoSuchElementException.class)
```
- 체크 예외를 롤백 대상으로 삼고싶을때 사용
- 기본적으로 트랜잭션은 런타임 예외와 Error가 발생했을때만 롤백
- 체크 예외나 예외가 발생하지 않으면 커밋하도록 한다

- ### noRollbakckFor
```sql
@Transactional(noRollbackFor={IOException.class, SqlException.class}
```
- 롤백 대상이 지정된 런타임 예외를 롤백하지 않고 커밋하도록 한다

----------------------------------------------------
참고 url

https://youtu.be/e9PC0sroCzc
https://youtu.be/ImvYNlF_saE
https://devlopsquare.tistory.com/237
