## Isolation Level의 필요성

 DB는 무결성을 보장하는 것이 중요하며 무결성을 보장하기 위한 특징이 ACID이다. DB는 ACID가 의미하는 바와 같이 Transaction이 원자적이면서
 독립적으로 수행하도록 해야 한다. 그래서 등장하는 개념이 Locking이다.
 
 Locking은 Transactino이 DB를 다루는 동안 다른 Transaction이 관여하지 못하게 막는다. 하지만 무조건적인 Locking으로 동시에 수행되는 많은
 Transaction들을 일렬로 대기시킨다면 DB의 성능은 현저하게 떨어진다.
 
 반대로 **응답성**을 높이기 위해 Locking 범위를 줄인다면 잘못된 값이 처리 될 여지가 있다. 따라서 최대한 효율적인 Locking 방법이 필요하다.
 이와 관련된 Locking 방법이 Isolation Level이다.
 
## Isolation Level 종류

### Read Uncommitted

 Select 문장을 수행하는 경우 **해당 데이터**에 **Shared Lock**이 걸리지 않는 level이다.
 
 따라서 어떤 사용자가 A를 B로 데이터를 변경하는 동안 다른 사용자는 **완료되지 않은 (Uncommitted 혹은 Dirty Data)** B를 읽을 수 있다.
 
 즉, Transaction이 끝나지 않은 상황에서 각각 다른 Transaction이 변경한 내용에 대한 조회가 가능하다. 그렇게 되면 DB의 일관성을 유지할 수 없다.
 
 ![image](https://user-images.githubusercontent.com/32594290/102317247-8e5f2880-3fba-11eb-8e7a-600aff443e12.png)
 
 Transaction1이 최초 수행되고 그 뒤 Transaction2가 값을 변경할 경우 다시 Transaction1이 조회하게 되면 Transaction2가 Commit을 하지 않았지만
 이미 Transaction2가 값을 변경하였기 때문에 '아무개'에서 '개발자'로 값이 변경되어 조회가 된다.
 
 
### Read Committed

 SQL Server가 Default로 사용하는 Isolation level이다.
 
 이 Level에선 Select 문장이 수행되는 동안 **해당 데이터**에 **Shared Lock**이 걸린다.
 
 따라서 어떠한 사용자가 A에서 B로 데이터를 변경하는 동안 다른 사용자는 해당 데이터에 접근할 수 없다. 
 
 **Read Uncommitted**와 다르게 **Commit**이 이루어진 데이터가 조회된다.
 
 하지만 어떠한 사용자가 A에서 B로 데이터를 변경하는 동안 다른 Transaction은 접근할 수 없어 **대기**하게 된다.
 
![image](https://user-images.githubusercontent.com/32594290/102317499-fada2780-3fba-11eb-95b1-dbcb81531c69.png)

 1. Transaction2가 Update를 수행한다.
 
 2. 아직 Commit하지 않아 Transaction1은 Select하지 못하고 대기한다.
 
 3. Transaction2가 Commit 명령어를 날린다.
 
 4. 이후 Transaction1이 조회가 가능하다.
 
### Repeatable Read

 Transaction이 완료될 때까지 Select 문장이 사용하는 모든 데이터에 **Shared Lock**이 걸리므로 다른 사용자는 그 영역에 해당되는
 데이터에 대한 수정이 불가능하다.
 
 가령 'Select col1 from A where col1 between 1 and 10' 을 수행한다고 가정하고, 이 범위에 해당하는 데이터가 2건이 있는 경우 (col1 = 1, 5)
 다른 사용자가 col1 = 1 혹은 col1 = 5인 Row에 대한 Update 작업이 불가능 하다.
 
 하지만 col1이 1과 5를 제외한 **나머지 범위**에 해당하는 Row를 **Insert**하는 것은 **가능**하다.
 
 그 결과 Transaction이 최초 수행된 후 해당 범위내에서는 **조회한 데이터**의 내용이 항상 **동일함**을 보장한다.
 
 ![image](https://user-images.githubusercontent.com/32594290/102317836-9ff50000-3fbb-11eb-95e5-3d53420cbb95.png)

 1. Transaction1이 Select 시점에 '아무개'가 조회된다.
 
 2. Transaction2가 Update 후 Commit을 시행하였으나 Update가 안되지만, Insert는 된다.
 
 3. Transaction1이 다시 조회해도 Transaction2가 Commit되지 않았기 때문에 '아무개'로 조회된다. 하지만 Insert한 '동네개발자'는 조회된다.
 
 4. Transacion1이 종료되면 다시 Commit이 이루어지기 때문에 '개발자'로 조회된다.
 
 
### Serializable

 말 그대로 **모든 동작**이 **직렬화**되어 작동한다. **Repeatable Read**와 다르게 **Insert**를 하여도 작동하지 않는다.
 
 Transaction이 완료될 때까지 Select 문장이 사용하는 **모든 데이터**에 **Shared Lock**이 걸리므로 다른 사용자는 그 영역에 해당되는
 데이터에 대한 **수정 및 입력**이 불가능하다.
 
 예를 들어, Repeatable Read의 경우 1 ~ 10 사이에 해당되는 데이터에 대한 Update가 가능하지만 이 Level에서는 **Update 작업**도 허용하지 않는다.
 
 ![image](https://user-images.githubusercontent.com/32594290/102318080-13970d00-3fbc-11eb-9c6c-d03a2a7fe3fb.png)

- - -

## 부작용 (Side Effect)

 ![image](https://user-images.githubusercontent.com/32594290/102318108-1d207500-3fbc-11eb-97de-d4eb69d2d9af.png)


### Dirty Read
 
 A Transaction 입장에서 아직 실행이 끝나지 않은 B Transactinon에 의한 변경 사항을 보게 되는 경우를 **Dirty Read**라고 한다.
 
 만약 **수정한 Transaction**이 그 변경 사항을 **Roll back**하면 그 데이터를 읽은 다른 Transaction은 **Dirty Data**를 갖고 있다고 말한다.
 
### Non-Repeatable Read

 A Transaction에서 **같은 Query**를 여러번 하여도 B Transaction에서 변경한 사항을 반영하지 못하고 변경되기 전 **같은 데이터**만 읽는 경우를
 **Non-Repeatable Read**라고 한다.
 
 즉, Non-Repeatable Read Level을 사용하는 DB에서는 **다른 Transaction**에 의한 **변경 사항**을 볼 수가 없다.
 
 변경 사항을 보고 싶다면 Application에서 Transcation을 **새로 시작**해야 한다.
 
### Phantom Read

 Phantom Read는 **다른 Transaction**에 의한 **변경 사항**으로 인해 현재 사용중인 Transaction의 Where 절의 **조건**에 맞는 **새로운 행**이
 생길 수 있는 경우에 관한 것이다.
 
 ``` 
 예를 들어 잔고가 $100 미마인 계좌가 2개인 DB에서 $100 미만인 계좌를 찾는 Transaction이 있고, 
 그 Transaction 안에서 Select 쿼리를 2번 수행했다고 가정하자.
 
 처음 데이터를 읽으면 2개의 계좌를 찾는다.
 
 이 때 다른 Transaction에서 $0인 계좌를 새로 만들면 두번째 데이터를 읽을 때 3개의 계좌를 찾는다.
 ```
 
 이처럼 Where 절의 **조건**에 맞는 **새로운 행**이 생길 수 있는 경우를 말한다. DB의 Transaction Isolation Level에서 **Phantom Read**를
 **지원**하면 새로운 **Phantom Row**가 나오지만 지원하지 않으면 새로 생긴 행을 볼 수 없다.

