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
![](https://velog.velcdn.com/images/minthug94_/post/f5b17515-06f6-4ca9-a3d5-d8a98c6b7876/image.png)

> ⚠️ Dirty Read
트랜잭션 A가 만약 트랜잭션을 끝마치지 못하고 롤백 한다면 트랜젝션 B는 
무효가 된 데이터 값을 읽고 처리를 하기 때문에 문제 발생
(etc. Non-Repeatable Read, Phantom Read)

- READ-COMMITTED
커밋이 완료된 트랜잭션의 변경사항만 다른 트랜잭션에서 조회 가능

- REPEATABLE-READ
트랜잭션 범위 내에서 조회한 내용이 항상 동일한 내용 보장

- SERIALIZABLE
한 트랜잭션에서 사용하는 데이터를 다른 트랜잭션에서 접근 불가

![](https://velog.velcdn.com/images/minthug94_/post/c7ccf35d-2df1-4f42-ac10-176d99f6c3c0/image.png)

### Transaction Propagation (트랜잭션 전파 타입)
- 트랜잭션의 경계에서 트랜잭션이 어떻게 동작할 것인가
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


