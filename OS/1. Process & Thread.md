# Process & Thread

 ### Process ?
 
  - 실행중인 프로그램
  
  - OS로부터 주소 공간, 파일, memory등을 할당 받음
  
  - process 주소 공간: Code, Data, Stack, Heap    
  
    - Code: 코드 자체를 구성하는 메모리 영역 (ex, 프로그램 명령)    
    - Data: 전역/정적 변수, 배열 등 (초기화된 데이터)   
    - Stack: 지역/매개 변수, 리턴 값 (임시 메모리 영역)   
    - Heap: 러닝 타임 중 동적 할당 시 사용    
  
  > 주소 공간을 왜 나누었나? 데이터를 최대한 공유하여 메모리 사용량을 줄이기 위해
    
 ### PCB (Process Control Block)
 
  - Process의 정보 저장
  
  - OS는 Process를 관리하기 위해 Process의 생성과 동시에 고유한 PCB 생성
  
  - Process가 CPU를 할당 받아 작업을 처리하는 도중 Context Switching이 발생하면 진행한 작업 상황을 PCB에 저장     
    다시 CPU를 할당 받게 되면 PCB를 참고하여 그 이후부터 작업 수행
 
 - - - 
 
 ### Thread
 
  - 한 Process 내에서 **동작되는 여러 실행 흐름**으로 Process 내의 주소 공간, 자원을 공유 (Code, Data, Heap 등)
  
  - Thread ID, Program Counter, Register Set, Stack으로 구성
  
  - Thread는 독립적인 작업 수행을 위해 각자의 Stack과 PC Register 값을 갖음
  
 #### Stack을 Thread마다 독립적으로 할당하는 이유?
   
   스택은 함수, 매개변수 되돌아갈 주소 값 및 함수 내 지역 변수를 저장하기 위해 사용되는 메모리 공간이다. 메모리 공간이 독립적이라는 것은
   독립적인 함수 호출이 가능하다는 의미. 따라서 독립적인 실행 흐름이 추가된다. 즉, Thread의 정의에 따라 독립적인 실행 흐름을 추가하기 위한 
   최소 조건으로 독립된 스택을 할당하는 것.
   
 #### PC register를 Thread 마다 독립적으로 할당하는 이유?
 
  PC(Program Counter)값은 Thread의 명령어가 어디까지 수행되었는가를 나타낸다. Thread는 CPU를 할당 받았다가 Schedular에 의해 다시 선점당하게 된다.
  명령어가 연속적으로 수행되지 못하고 어디까지 수행되었는지 기억할 필요가 있기 때문에 PC Register를 독립적으로 할당한다.
  

- - -

### Multi-Thread

 - 한 Process를 다수의 실행 단위로 구분하여 자원을 공유하고 **자원의 생성과 관리의 중복성을 최소화**하여 수행능력을 향상 시키는 것
 
 #### Multi-Threading의 장점
 
  - Process를 이용하여 동시에 처리하던 일을 Thread로 구현할 경우 **메모리 공간과 자원 소모가 줄어든다.**
  
  - Thread간 통신이 필요한 경우 별도 자원을 이용하는 것이 아닌 **전역 변수 공간** 또는 **동적 할당 공간인 Heap**을 이용하여 데이터를 주고 받을 수 있다.
 
  - 따라서 Process 간 통신방법에 비해 Thread 간 통신방법이 훨씬 간단함.
  
  - 심지어 Thread의 Context Switching은 Process와 달리 **Cache Memory**를 비울 필요가 없기 때문에 더 빠르다.
  
  - 따라서 시스템의 Throughput이 향상되고 자원 소모가 줄어들어 자연스럽게 Program의 응답 시간이 단축된다.
  
 #### Multi-Threading의 문제점
 
  - Multi Process 기반 프로그래밍은 Process 간 공유 자원이 없기 때문에 **동일한 자원에 동시 접근**하는 일이 없지만
    Multi-Thread 기반 프로그래밍은 이러한 경우가 발생
    
  - 서로 다른 Thread가 Data나 Heap을 공유하기 때문에 다른 Thread에서 사용중인 변수 및 자료구조에 접근하여 엉뚱한 값을 읽거나 수정하는 일이 발생
  
  - Multi-threading 환경에서는 **동기화 작업**이 필요함. 동기화를 통해 작업 순서를 컨트롤 하고 공유 자원에 대한 접근을 컨트롤.
  
  - 하지만 이로 인해 **병목 현상**이 일어나 성능 저하가 발생할 수 있음. 과도한 Lock으로 인한 병목 현상을 줄여야 한다.
  
- - -

### 👁‍🗨 MultiThread vs MultiProcess

 - Multi-Thread는 Multi-Process보다 **적은 메모리 공간**을 차지하고 Context Switching이 빠르지만, 오류로 인해 **하나의 Thread가 종료되면
   전체 Thread가 종료될 수 있으며(공유 메모리)**, 동기화의 문제가 있다(Parallel).
   
 - Multi-Process는 하나의 Process가 죽더라도 다른 Process에 영향을 미치지 않고 정상적으로 수행하지만, Multi-thread보다 **많은 메모리 공간과
   CPU 시간을 차지**하는 단점이 있다(Concurrency). 
 
 - 이 두가지는 동시에 여러 작업을 수행한다는 점에서 같으나, 적용해야 하는 시스템에 따라 적합 & 부적합을 구분해야 한다.
  
  
