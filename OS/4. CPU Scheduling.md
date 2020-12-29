# CPU Scheduling

## 프로세스를 스케줄링하기 위한 Queue의 3가지 종류
 
 - Job Queue: 현재 시스템 내에 있는 모든 프로세스의 집합
 
 - Ready Queue: 현재 메모리 내에 있으면서 CPU를 잡아 실행되기를 기다리는 프로세스 집합
 
 - Device Queue: I/O 작업을 기다리는 프로세스 집합
 
## 각각의 Queue에서 프로세스를 넣고 빼는 스케줄러 종류 3가지

### 장기 스케줄러 (Long-Term Scheduler or Job Scheduler)

 메모리는 한정되어 있는데, 한번에 많은 프로세스가 메모리에 로드된 경우 대용량 메모리(=Hdd)에 임시로 저장됨.
 이 Pool에 저장된 프로세스 중 어떤 프로세스를 메모리에 할당하여 Ready Queue로 보낼 지 결정하는 역할 수행
 
 - 메모리와 디스크 사이의 스케줄링 담당
   
 - 프로세스에 메모리 할당
 
 - Degree of Multiprogramming 제어 (실행중인 프로세스 수 제어)
 
 - 프로세스 상태: 'new' ⇒ 'ready (in memory)'
 
 메모리에 프로그램이 너무 많이 올라가거나 너무 적게 올라가지 않게 조절함. 
 
 > ❓ Time sharing system에서는 장기 스케줄러가 없다. (그냥 곧바로 메모리에 올라가 ready 상태가 됨)
 
### 단기 스케줄러 (Short-Term Scheduler or CPU Scheduler)

 - CPU와 메모리 사이의 스케줄링 담당
 
 - Ready queue에 존재하는 프로세스 중 어떤 프로세스를 Running 할 지 결정
 
 - Process에 CPU를 할당 (Scheduler dispatch)
 
 - 프로세스 상태: 'ready' ⇒ 'running' ⇒ 'waiting' ⇒ 'ready'
 
### 중기 스케줄러 (Medium-term Scheduler or Swapper)

 - 여유 공간 마련을 위해 프로세스 전부를 메모리에서 디스크로 쫓아냄 (Swapping)
 
 - 프로세스에게서 memory를 deallocate
 
 - Degree of Multiprogramming 제어. 현 시스템에서 메모리에 너무 많은 프로그램이 동시에 올라가는 것을 조절함.
 
 - 프로세스 상태: 'ready' ⇒ 'suspended'
 
 > ❓ Suspended
 > 
 >   Suspended(stopped)는 외부적 요인으로 프로세스의 수행이 정지된 상태로 메모리에서 내려간 상태를 의미함. 프로세스 전부가 디스크로 Swap out 된다.
 >  
 >   Blocked 상태는 다른 I/O 작업을 기다리는 상태이기 때문에 스스로 Ready state로 돌아갈 수 있지만, Suspended 상태는 외부적 요인으로
 >   suspending 되었기 때문에 스스로 돌아갈 수 없음.
 
- - - 

## CPU 스케줄러

### FCFS (First Come First Served)

 - 먼저 온 프로세스 순으로 처리
 
 - 비선점형 스케줄링 (Non-preemptive): CPU 선점시 CPU burst가 완료될 때 까지 CPU를 반환하지 않음
 
 - 문제점
   
   - Convoy Effect: 소요시간이 긴 프로세스가 먼저 도달해 효율성이 낮아지는 현상
   
### SRT (Shortest Remaining Time First)

 - 새로운 프로세스가 도착할 때 마다 새로운 스케줄링이 이루어짐
 
 - 선점형 스케줄링: 현재 수행중인 프로세스의 남은 Burst time보다 더 짧은 프로세스가 들어오는 경우 CPU를 선점당함
 
 - 문제점
 
   - Starvation: 남은 시간이 짧은 프로세스가 계속 들어와 긴 시간을 가진 프로세스가 수행되지 못하는 현상
   
   - 새로운 프로세스가 도착할 때 마다 스케줄링을 다시하기 때문에 CPU Burst Time(CPU 사용 시간)을 측정할 수 없음
   
### Priority Scheduling

 - 우선순위가 가장 높은 프로세스에게 CPU 할당 (작은 숫자일 수록 높은 우선순위)
 
 - 선점형 방식: 더 높은 우선순위의 프로세스가 들어오면 실행중인 프로세스를 멈추고 CPU 선점
 
 - 비선점형 방식: 더 높은 우선순위의 프로세스가 들어오면 Ready Queue Head에 push
 
 - 문제점
 
   - Starvation
   
   - 무기한 봉쇄 (Indefinite Blocking): 실행 준비는 되었으나 CPU를 사용하지 못하는 프로세스를 CPU가 무기한 대기하는 상태
   
 - 해결 방법
 
   - Aging: 오래 기다린 프로세스의 우선순위를 높여준다.
   
### Round Robin

 - 현대적인 CPU 스케줄링
 
 - 각 프로세스는 동일한 크기의 할당된 시간을 갖는다 (= Time Quantum)
 
 - 프로세스의 할당 시간이 지나면 CPU를 선점당하고, Ready Queue의 제일 뒤에 들어간다.
 
 - RR은 CPU 사용시간이 랜덤한 프로세스들이 섞여 있을 경우 효율적임
 
 - RR이 가능한 이유 ⇒ 프로세스의 Context를 저장할 수 있기 때문
 
 - 장점: Response Time이 빨라진다. N개의 프로세스가 Ready queue에 있고, 할당 시간이 Q(time quantum)인 경우 각 프로세스는 Q 단위로 
 CPU 시간의 1/n을 얻는다. 즉, 어떤 프로세스라도 (n-1)Q time unit 이상 기다리지 않게 된다.
 
 - 프로세스가 기다리는 시간이 CPU를 사용할 만큼 증가함 ⇒ 공정한 스케줄링.
 
 - 주의점: 설정한 Time quantum이 너무 커지면 FCFS와 같고, 너무 작아지면 스케줄링 알고리즘 목적에는 이상적이지만 잦은 Context Switching으로
 인한 Overhead가 발생한다. 따라서 적당한 Time quantum을 설정하는 것이 중요하다.
 
- - -

## System 별 스케줄링 목표

 - Batch System: 가능한 많은 일을 수행. 시간보다 처리량(Throughput)이 중요 
    
   - 일괄처리시스템(Batch Processing System) 이란 일정량 또는 일정 기간 동안 데이터를 모아서 한꺼번에 처리하는 방식
 
 - Interactive System: 빠른 응답 시간. 적은 대기 시간
 
 - Real time System: 기한(Deadline) 맞추기
 

## 스케줄링 척도

 - Response Time: 작업이 처음 실행되기까지 걸린 시간
 
 - Turnaround Time: 실행 시간과 대기 시간을 모두 합친 시간으로 작업이 모두 완료될 때까지 걸린 시간.
