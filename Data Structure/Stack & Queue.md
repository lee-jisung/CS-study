## Stack

 - 선형 자료구조
 
 - 나중에 들어간 원소가 먼저 나오는 구조, LIFO (Last In First Out)
 
 - 안드로이드의 Activity를 관리하는 데 스택이 사용된다.
 
 - 시간 복잡도: O(n)
 
 - 공간 복잡도: O(n)
 
 - 함수의 콜 스택, 문자열 역순 출력, 연산자 후위 표기법 등에 사용
 
#### 동적 배열 스택

 일반 배열 스택은 MAX_SIZE라는 최대 크기가 존재
 
 최대 크기가 없는 스택을 만드려면?
 
 ```
 기존 스택의 2배 크기만큼 임시 배열을 만들고 arraycopy를 통해 stack의 인덱스 0 ~ MAX_SIZE만큼 임시 배열에 복사한다.
 
 복사 후 임시배열의 참조 값을 stack 배열에 덮어 씌운다. 
 
 마지막으로 MAX_SIZE의 값을 2배로 증가시킨다.
 
 이러면 스택이 가득 찼을 때 자동으로 확장되는 스택을 구현할 수 있다.
 ```
 
## Queue

 - 선형 자료구조
 
 - 먼저 들어간 원소가 먼저 나오는 구조, FIFO (First In First Out)
 
 - 큐의 활용 분야는 다양하며 안드로이드의 경우 루퍼의 메시지 큐가 예시 중 하나
 
 - Java Collection에서 Queue는 Interface이다.
 
 - 시간 복잡도: O(n)
 
 - 공간 복잡도: O(n)
 
 - 버퍼, 마구 입력된 것을 처리하지 못하는 상황, bfs 탐색 등에 사용
 
