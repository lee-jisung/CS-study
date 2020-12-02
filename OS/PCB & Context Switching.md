## Process Management

 - 프로세스가 여러 개인 상황에서 CPU가 스케줄링을 통해 프로세스를 관리
 
 - CPU는 각 프로세스의 상태 정보를 알아야 함
 
 - Process Metadata ⇒ 프로세스의 특징을 갖고 있는 data
  
   - Process ID / State / Priority / CPU Registers / Owner / CPU Usage / Memory Usage
  
 - 이 Metadata는 프로세스가 생성되면 PCB에 저장된다.
 
   - 프로그램 실행 ⇒ 프로세스 생성 ⇒ 프로세스 주소 공간 (코드, 데이터, 스택) 생성 ⇒ 프로세스의 metadata가 PCB에 저장
   
 #### PCB는 왜 필요한가?
 
  - CPU가 프로세스를 교체하는 작업인 Context Switching을 수행할 때, waiting 상태가 되는 process의 상태 정보를 저장하기 위함
  
  - PCB 관리 방식
  
    - Linked list
    
    - PCB List Head에 PCB가 생성될 때 마다 연결. 주소 값으로 연결이 이루어져 있으며 삽입, 삭제가 용이함.
    

## Context Switching

 - CPU가 이전 프로세스 상태를 PCB에 보관하고 또 다른 정보를 PCB에서 읽어 Register에 적재하는 과정
 
 - 인터럽트 발생, CPU 사용 시간을 모두 소모, I/O 작업을 위해 대기해야 하는 경우 등... 에서 Context Switching이 발생
 
 - 스레드의 경우 프로세스 내부의 stack을 제외한 모든 메모리를 공유하기 때문에 Stack만 독립적으로 교체한다.
 
 ### Context Switching의 Overhead란?
 
  - Overhead는 간접비, 과부하라는 뜻으로 프로세스 작업 중 Context Switching을 하는 것은 Overhead를 발생시킴.
   하지만 이러한 Overhead를 감수해야 하는 상황이 있음
  
  - 프로세스 수행 도중 I/O 이벤트가 발생하여 대기상태로 전환 ⇒ 이 때, CPU를 놀게 놔두는 것이 아닌 다른 프로세스를 수행 시키기 위해 
  Context Switching이 이루어짐 ⇒ 퍼포먼스 향상
