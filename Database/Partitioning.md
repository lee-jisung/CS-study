## DB Partitioning (파티셔닝)

### 배경

 서비스의 크기가 점점 커지고 DB에 저장하는 데이터의 규모 또한 대용량화 되면서 기존에 사용하는 DB 시스템의 용량(storage)의 한계와
 성능(performance)의 저하를 가져오게 되었다.
 
 즉, VLDB(Very Large DBMS)와 같이 하나의 DBMS에 너무 큰 table이 들어가면서 용량과 성능 측면에서 많은 이슈가 발생했고, 이런 이슈를 해결하기
 위해서 table을 **파티션(partition)**이라는 작은 단위로 나누어 관리하는 **파티셔닝(Partitioning)**기법이 나오게 되었다.
 
 파티셔닝 기법을 통해 소프트웨어적으로 DB를 분산처리하여 성능이 저하되는 것을 방지하고 관리를 보다 수월하게 할 수 있게 되었다.
 
### 개념

 논리적인 데이터 element들을 다수의 entity로 쪼개는 행위를 뜻하는 일반적인 용어
 
 즉 큰 table이나 index를 관리하기 쉬운 partition이라는 작은 단위로 물리적 분할하는 것을 의미한다.
 
  - 물리적인 데이터 분할이 있더라도 DB에 접근하는 Application의 입장에서는 이를 인식하지 못한다.
  
### 목적

#### 성능 (Performance)
 
 - 특정 DML과 Query의 성능을 향상시킨다.
 
 - 주로 대용량 Data write 환경에서 효율적
 
 - Full Scan에서 데이터 Access 범위를 줄여 성능 향상을 가져온다.
 
 - 많은 Insert가 있는 OLTP 시스템에서 Insert 작업을 작은 단위인 partition들로 분산시켜 경합을 줄인다.
 
#### 가용성 (Availability)

 - 물리적인 파티셔닝으로 인해 전체 데이터의 훼손 가능성이 줄어들고 데이터 가용성이 향상된다.

 - 각 분할 영역(partition별)을 독립적으로 백업, 복구할 수 있다.
 
 - table의 partition 단위로 Disk I/O를 분산하여 경합을 줄이기 때문에 Update 성능을 향상시킨다.
 
#### 관리용이성 (Manageability)

 - 큰 table들을 제거하여 관리를 쉽게 해준다.
 
## DB 파티셔닝의 장단점

### 장점

 #### 관리적 측면
 
 Parition 단위 백업, 추가, 삭제, 변경
 
  - 전체 데이터를 손시할 가능성이 줄어들어 데이터 가용성이 향상된다.
  
  - partition별로 백업 및 복구가 가능하다.
  
  - parition 단위로 I/O 분산이 가능하여 Update 성능을 향상시킨다.
  
 #### 성능적 측면
 
 Parition 단위 조회 및 DML 수행
 
  - 데이터 전체 검색 시 필요한 부분만 탐색해 성능이 증가
  
    - 즉, Full scan에서 데이터 Access 범위를 줄여 성능 향상을 가져온다.
    
    - 필요한 데이터만 빠르게 조회할 수 있기 때문에 쿼리 자체가 가볍다.

### 단점

 - table간 Join에 대한 비용이 증가
 
 - table과 index를 별도로 파티셔닝할 수 없고, 같이 파티셔닝 해야 함.
 
- - -

## DB 파티셔닝 종류

### 수평 (horizontal) 파티셔닝

![image](https://user-images.githubusercontent.com/32594290/102599964-8ee5f380-4161-11eb-8b3c-5aafd16ef69a.png)

 하나의 테이블의 각 행을 다른 테이블에 분산시킨다. 

 #### 개념
 
  - 샤딩과 동일한 개념
  
  - 스키마를 복제한 후 샤드키를 기준으로 데이터를 나누는 것
  
  - 즉, 스키마가 같은 데이터를 두 개 이상의 테이블에 나누어 저장하는 것
  
 #### 특징
 
  - 퍼포먼스, 가용성을 위해 KEY 기반으로 여러 곳에 분산 저장
  
  - 일반적으로 분산 저장 기술에서 파티셔닝은 수평 분할을 의미
  
  - 보통 수평 분할을 한다고 했을 때는 하나의 DB 안에서 이루어지는 경우를 지칭
  
 #### 장점
 
  - 데이터 개수를 기준으로 나누어 파티셔닝
   
  - 데이터 개수가 줄면서 index 개수도 줄어들고, 성능은 향상된다.
  
 #### 단점
 
  - 서버간의 연결 과정이 많아진다.
   
  - 데이터를 찾는 과정이 기존보다 복잡하기 때문에 latency가 증가한다.
   
  - 하나의 서버가 고장나면 데이터의 무결성이 깨질 수 있다.
   
### 수직 (vertical) 파티셔닝

 ![image](https://user-images.githubusercontent.com/32594290/102600599-614d7a00-4162-11eb-85fe-816c21cc161b.png)

 #### 개념
 
  - 모든 컬럼들 중 특정 컬럼들을 쪼개서 따로 저장하는 형태
  
  - 스키마를 나누고 데이터가 따라 옮겨가는 것
  
  - 하나의 엔티티를 2개 이상으로 분리하는 작업
  
 #### 특징
 
  - 관계형 DB에서 제 3 정규화와 같은 개념으로 접근하면 이해하기 쉬움
  
  - 하지만 수직 파티셔닝은 이미 정규화된 데이터를 분리하는 과정
  
 #### 장점
 
  - 자주 사용하는 컬럼 등을 분리시켜 성능을 향상 시킬 수 있음
  
  - 한 테이블을 SELECT 하면 결국 모든 컬럼을 메모리에 올리게 되므로 필요없는 컬럼까지 올라가 한 번에 읽을 수 있는 row가 줄어든다.
  이는 I/O 측면에서 필요한 컬럼만 올리면 더 많은 수의 row를 메모리에 올릴 수 있어 성능상 이점이 있다.
  
  - 같은 타입의 데이터가 저장되어 저장 시 데이터 압축률을 높일 수 있다.
  
- - -

## DB 파티셔닝 분할 기준

![image](https://user-images.githubusercontent.com/32594290/102600896-c43f1100-4162-11eb-94f7-c6a9a6aef47d.png)

DBMS는 분할에 대해 각종 기준을 제공하며 분할은 '분할키(partitioning key)'를 사용한다.

### 범위 분할 (Range Partitioning)

![image](https://user-images.githubusercontent.com/32594290/102601492-94443d80-4163-11eb-987c-5c8ddac039af.png)

 - 연속적인 숫자나 날짜를 기준으로 Partitioning
 
 - ex) 우편번호, 일별, 월별, 분기별 등의 데이터에 적합
 
### 목록 분할 (List Partitioning)

![image](https://user-images.githubusercontent.com/32594290/102601527-a0c89600-4163-11eb-9079-ba8c22c10f48.png)

 - 특정 Parition에 저장 될 Data에 대한 명시적 제어가 가능
 
 - 분포도가 비슷하며 많은 SQL에서 해당 Column의 조건이 많이 들어오는 경우 유용
 
 - Multi-Column Partition Key를 제공하기 힘들다.
 
 - ex, [한국, 일본, 중국 -> 아시아] [노르웨이, 스웨덴, 핀란드 -> 북유럽]
 
### 해시 분할 (Hash Paritioning)

 - 해시 함수의 값에 따라 파티션에 포함할지 여부를 결정
 
 - SELECT 시 조건과 무관하게 병렬 Degree 제공 (질의 성능 향상)
 
 - 특정 Data가 어느 Hash Parition에 있는지 판단 불가
 
 - 파티션을 위한 범위가 없는 데이터에 적합
 
 - ex, 4개의 파티션으로 분할하는 경우 해시 함수는 0 ~ 3의 정수를 돌려준다.
 
### 합성 분할 (Composite Paritioning)

 - Parition의 Sub-paritioning을 의미
 
 - 큰 파티션에 대한 I/O 요처을 여러 parition으로 분산할 ㅅ ㅜ있다.
 
 - Range Paritioning 할 수 있는 Column이 있지만, Paritioning 결과 생성된 Parition이 너무 커서 효과적으로 관리할 수 없을 때 유용
 
- - -

### Ref.

 - https://nesoy.github.io/articles/2018-02/Database-Partitioning
 
 - https://gmlwjd9405.github.io/2018/09/24/db-partitioning.html
