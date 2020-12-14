# 프로세스 동기화

### Race Condition (경쟁 상태)

 공유 자원에 대해 여러 프로세스가 동시에 접근할 때, 결과값에 영향을 줄 수 있는 상태
 
 자료의 일관성을 해치는 결과를 낼 수 있음
 
### Ciritical Section (임계 영역)

 멀티스레딩 문제점에서도 나오듯 동일한 자원을 동시에 접근하는 작업(ex, 공유 변수 사용 등)을 실행하는 코드 영역
 
 Ciritical Section Problem: 프로세스들이 Critical Section을 함께 사용할 수 있는 프로토콜을 설계하는 것
 
### Requirements 

 - Mutual Exclusion (상호 배제)
  
   프로세스 'P1'이 Critical Section에서 실행중이라면 다른 프로세스들은 그들이 가진 Critical Section에서 실행될 수 없다.
   
 - Progress (진행)
 
   Critical Section에서 실행중인 프로세스가 없고 별도의 동작이 없는 프로세스들만 Critical Section 진입 후보가 될 수 있다.
   
 - Bounded Waiting (한정된 대기)
 
   'P1'이 Critical Section에 진입 신청 후부터 받아들여질 때까지 다른 프로세스들이 Critical Section에 진입하는 횟수는 제한이 있어야 한다.
   
### 해결책

 ### Lock
  
   **하드웨어 기반 해결책** 으로써 동시에 공유 자원에 접근하는 것을 막기 위해 Critical Section에 진입하는 프로세스는 Lock을 획득하고,
   Critical Section을 빠져나올 때 Lock을 방출하여 동시 접근을 막는다.
   
   한계: 다중처리기 환경에서는 시간적인 효율성 측면에서 적용할 수 없다.
   
 ### Semaphore
 
  **SW 상에서 Critical Section 문제를 해결** 하기 위한 동기화 도구
   
  #### 종류 (OS는 Counting, Binary semaphore를 구분한다.)
   
  - Counting Semaphore
      
     가용한 개수를 가진 자원에 대한 접근 제어용으로 사용. Semaphore는 **가용한 자원의 개수로 초기화** 된다. 자원을 사용하면 
     Semaphore가 감소, 방출하면 증가
      
  - Binary Semaphore
    
     MUTEX라고도 부르며 상호배제의 머리 글자를 따서 만들어졌다. 이름 그대로 **0과 1 사이의 값** 만 가능하며 다중 프로세스들 사이의 
     Critical Section 문제를 해결하기 위해 사용한다.
      
  #### 단점
   
  - Busy Waiting
      
    **Spin Lock** 으로도 불리며 Semaphore 초기 버전에서 CS에 진입해야 하는 프로세스는 진입 코드를 계속 반복 실행하여 CPU를 낭비하는 현상.
    특수한 상황이 아니면 비효율적이다. 
      
    일반적으로 Semaphore에서 CS 진입을 시도했지만, 실패한 프로세스를 Block 시킨 뒤 CS에 자리가 날 때 다시 깨우는 방식을 사용한다. 
    **(Sleep, wakeup)** 이 경우 Busy Waiting으로 인한 시간/자원 낭비 문제를 해결할 수 있다.
      
      
- - - 

## Semaphore vs MUTEX


#### MUTEX

 - 공유된 자원의 데이터를 여러 Thread가 접근하는 것을 막는 것
 
 - 상호배제라고도 하며 Critical Section을 가진 Thread의 Running Time이 서로 겹치지 않도록 각각 단독으로 실행하게 하는 기술
 
 - 다중 프로세스들의 공유 리소스에 대한 접근을 조율하기 위해 Synchronized 또는 Lock을 사용
 
   - 즉, MUTEX 객체를 두 Thread가 동시에 사용할 수 없다.
   
#### Semaphore

 - 공유된 자원의 데이터를 여러 프로세스가 접근하는 것을 막는 것
 
 - 리소스 상태를 나타내는 간단한 카운터로 생각
 
   - OS 또는 Kernel의 한 지정된 저장장치 내의 값
   
   - 일반적으로 비교적 긴 시간을 확보하는 리소스에 대해 이용
   
   - 유닉스 시스템 프로그래밍에서 Semaphore는 OS의 리소스를 경쟁적으로 사용하는 다중 프로세스에서 행동을 조정하거나 또는 동기화 시키는 기술
   
 - 공유 리소스에 접근할 수 있는 프로세스의 최대 허용치만큼 동시에 사용자가 접근하여 사용 가능
 
 - 각 프로세스는 Semaphore값을 확인하고 변경할 수 있음
 
   - 사용 중이지 않은 자원의 경우 그 프로세스가 즉시 자원 사용
   
   - 이미 다른 프로세스에 의해 사용중인 사실을 알게 될 경우 재시도하기 전 일정 시간 기다림
   
   - Semaphore를 사용하는 프로세스는 그 값을 확인하고, 자원을 사용하는 동안 그 값을 변경함으로써 다른 Semaphore 사용자들이 기다리도록 해야 함
   
 - Smeaphore는 이진수(0 & 1)을 사용하거나, 추가적인 값을 가질 수 있다.
 
#### 차이

 - 가장 큰 차이점은 관리하는 **동기화 대상의 개수**
  
   - MUTEX는 동기화 대상이 오직 하나뿐일 때, Semaphore는 동기화 대상이 하나 이상 일 때 사용
   
 - Semaphore는 MUTEX가 될 수 있지만, MUTEX는 Semaphore가 될 수 없음
 
   - MUTEX는 상태가 0, 1 두 개 뿐인 Binary Semaphore
   
 - Semaphore는 소유할 수 없는 반면, MUTEX는 소유가 가능하며 소유주가 이에 대한 책임을 지닌다.
 
   - MUTEX의 경우 상태가 두개 뿐인 Lock이므로 Lock을 가질 수 있음
   
 - MUTEX의 경우 MUTEX를 소유하고 있는 Thread가 이 MUTEX를 해제할 수 있다. 하지만 Semaphore의 경우 이러한 Semaphore를 소유하지 않는 Thread가 Smeaphore를 해제할 수 있다.
 
 - Semaphore는 시스템 범위에 걸쳐있고 파일 시스템 상 파일 형태로 존재하는 반면, MUTEX는 프로세스 범위를 가지며 프로세스가 종료될 때 자동으로 Clean up 된다.

