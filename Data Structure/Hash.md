## Hash Table

 데이터를 효율적으로 관리하기 위해서 임의의 길이 데이터를 고정된 데이터로 Mapping하는 것
 
 Hash Function을 구현하여 데이터 값을 Hash 값으로 mapping한다.
 
 Hash는 내부적으로 배열을 사용하여 데이터를 저장하기 때문에 빠른 검색 속도를 갖는다.
 특정 값을 Search 하기위해 데이터 고유의 Index로 접근하여 average case에 대해 Time Complexity가 O(1)이 된다.
   
  > 항상 O(1)은 아니며 Average case에 대해 O(1)인 것은 Collision 때문
 
 ```
  ABCDE → Hash Function → 6
  FGHIJ → Hash Function → 2
  KLMNO → Hash Functoin → 4
  ...
  VWXYZ → Hash Function → 2 // 'FGHIJ'와 해시값 충돌
 ```
 
 데이터가 많아지면 다른 데이터와 동일한 해시 값으로 충돌이 발생하는 현상이 일어나는데 이를 **Collision**이라고 한다.
 
 이와 같이 동일한 Hash 값을 리턴하는 Hash Function은 좋다고 할 수 없다. 
 
  #### _그래도 Hash Table을 사용하는 이유는?_
 
  - 적은 자원으로 많은 데이터를 효율적으로 관리
  
  - HDD나 Cloud에 존재하는 무한한 데이터들을 유한한 개수의 Hash 값으로 Mapping 하여 작은 Memory로 프로세스 관리가 가능
  
  - 언제나 동일한 Hash값 리턴, Index를 알면 빠른 데이터 검색이 가능
  
  - Hash Table의 시간 복잡도: O(1)
 
 #### 그렇다면 좋은 **Hash Function**은 어떠한 조건을 갖추어야 하는가?
  
 일반적으로 좋은 Hash Function은 키의 일부분을 참조해서 Hash 값을 만들지 않고 키 전체를 참조하여 만든다. 하지만 좋은 Function은 
 키가 어떤 특성을 가지고 있느냐에 따라 달라진다.
 
 Hash Function은 무조건 1:1로 만드는것 보다 Collision을 최소화하는 방향으로 설계하고 발생하는 Collision에 대비해야 한다. 
 
 1:1 대응이 되도록 만드는 것이 거의 불가능하지만 그러한 Hash Function은 array와 다를바 없으며 메모리를 너무 많이 차지하게 된다.
 
 Collision이 많아질수록 Search에 필요한 Time complexity가 O(1)에서 O(n)에 가까워진다. 따라서 Hashing된 인덱스에 이미 값이 들어있다면
 새 데이터를 저장할 다른 위치를 찾아 저장할 수 있는 것이다. 이러한 충돌 해결 방법에 대해 알아보자.
 
### Resolve Conflict

 #### 1. Open Address 방식 (개방 주소법)
 
  Collision이 발생하면 **다른 hash bucket에 해당 자료를 삽입**하는 방식이다. bucket이란 바구니와 같은 개념으로 데이터를 저장하기 위한
  공간이다. 
  
   - **Linear Probing**: 순차적으로 탐색하며 비어있는 bucket을 찾을 때 까지 계속 진행
   
   - **Quadratic Probing**: 2차 함수를 이용해 탐색
   
   - Double Hahsing Probing: 하나의 Hash Function에서 충돌이 발생하면 다른 Hash Fuction을 이용해 새로운 Hash 값을 리턴 받아 이용한다.
   위의 2가지 방법에 비해 많은 연산량을 요구한다.
  
  공개 주소 방식이라고도 불리며 Collision이 발생하면 위의 3가지 방법을 통해서 데이터를 저장하기 위한 장소를 찾는다. 
  Worst case의 경우 비어있는 bucket을 찾지 못하고 시작한 위치까지 되돌아올 수 있다. 
  
 #### 2. Separate Chaining 방식 (분리 연결법)
 
  Separate Chaining 방식경우 Hash 충돌이 잘 발생하지 않도록 보조 hash function을 통해 조정할 수 있다면 worst case에 가까워지는
  빈도를 줄일 수 있다. Java 7에서는 Separate Chaining 방식을 사용하여 HashMap을 구현하고 있다.
  
   - **Linked List를 사용하는 방법**
     
     - 각각의 bucket을 Linked List로 만들어 Collision이 발생하면 해당 bucket의 list에 추가하는 방식
     
     - linked list의 특징을 그대로 이어받아 삽입, 삭제가 간단함
     
     - 작은 데이터들을 저장할 때 linked list 자체의 오버헤드가 부담되는 단점이 있다.
     
     - bucket을 계속 사용하는 Open Address 방식에 비해 table 확장을 늦출 수 있다.
     
   - **Tree를 사용하는 방법**
    
     - 기본적인 알고리즘은 Separate Chaining과 동일하며 linked list 대신 tree를 사용하는 방식
     
  Linked list와 Tree 중 어느 것을 고를지에 대한 기준은 하나의 Hash bucket에 할당된 Key-Value 쌍의 개수이다. 데이터 개수가 적다면
  Linked list를 사용한다. Tree는 기본적으로 메모리 사용량이 많기 때문이다. 데이터 개수가 적을 때 Worst case를 살펴보면 Tree와 Linked list
  의 성능 상 차이가 거의 없다. 따라서 메모리 측변에서 데이터 개수가 적을 때는 Linked list를 사용한다
  
  #### _데이터가 적다는 것의 기준은?_
  
   key-value 쌍의 개수로, 이 쌍의 개수가 6개, 8개를 기준으로 결정한다. 기준이 2개인 것이 이상할 수 있는데, Linked list 기준과 Tree 기준을
   6과 8로 잡은 것은 저장 자료구조를 변경하는데 소요되는 비용을 줄이기 위함이다.
   
   ```
    hash bucket에 6개 key-value가 들어있다고 가정
    
    기준이 6과 7이라면 하나가 추가되자마자 자료구조를 Linked list에서 Tree로 교체
    
    하나가 삭제되면 다시 바로 Tree에서 Linked list로 교체해야 한다.
    
    자료구조가 변하는 기준이 차이 값이 1이라면 Switching 비용이 과다하게 소요된다.
    
    따라서 2라는 여유를 두고 기준을 잡는다. 
    
    즉, 데이터 개수가 6 → 7이 되었을 때는 linked list, 8 → 7 이 되었을 때는 Tree의 구조를 취하고 있을 것이다.
  ```
   
 - - -
   
### Open Address vs Separate Chaining
 
  두 방식 모두 Worst Case에서 O(M)이다. 그러나 Open Address 방식은 연속된 공간에 데이터를 저장하기 때문에 Separage Chaining에 비해
  캐시 효율이 높다. 따라서 데이터 개수가 충분히 적다면 Open address가 Separate chaining보다 성능이 더 좋다.
  
  한가지 차이점은 Separate chaining 방식에 비해 Open address방식이 bucket을 계속 사용한다. 따라서 Separate Chaining 방식은
  테이블 확장을 보다 늦출 수 있다.
  
 
### 보조 해시 함수

 Supplement Hash Function의 목적은 Key의 Hash 값이 변형하여 Hash 충돌 가능성을 줄이는 것이다. Separate Chaining 에서 사용되며, 
 보조 해시 함수로 Worst Case에 가까워지는 경우를 줄일 수 있다.
 
### Hash Bucket 동적 확장 (Resize)

 Hash Bucket의 개수가 적다면 메모리 사용을 아낄 수 있으나, Hash 충돌로 인해 성능 상 손실이 발생한다.
 
 따라서 HashMap은 Key-Value 쌍 데이터 개수가 일정 개수 이상이 되면 Hash Bucket 개수를 두배로 늘린다. 이렇게 늘리면 Hash
 
 충돌로 인한 성능 손실 문제를 어느 정도 해결할 수 있다. 또 애매모호한 '일정 개수 이상'이라는 표현이 등장했는데, Hash Bucket를 두 배로
 확장하는 임계점은 현재 데이터 개수가 Hash Bucket의 75%가 될 때이다. 0.75라는 숫자는 **load factor**라고 불린다.
   
   
   
   
   
   
   
   
  
  
  
  
  
  
  
  
  
  
  
  
  

 
