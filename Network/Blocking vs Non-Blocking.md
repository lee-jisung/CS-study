# Blocking I/O vs Non-Blocking I/O

## I/O 작업 

 파일 입출력을 다루는 작업
 
 ex) 두 대 이상의 PC 끼리 서로 네트워크를 통해 통신한다고 가정하자. 한 PC에서 출력(send)하고 다른 PC에서 입력(read)을 받는 과정을 통해
 통신할 수 있다.
 
 I/O 작업은 User Level에서 직접 수행할 수 없고, **실제 I/O 작업을 수행하는 것은 Kernel Level**에서만 가능하다.
 
 User Process, Thread는 커널에게 요청하고 작업 완료 후 커널이 반환되는 결과를 기다린다.
 
## Blocking Model

 가장 기본적인 I/O 모델로, 리눅스에서 모든 소켓 통신은 기본 Blocking으로 동작한다.
 
 I/O 작업이 진행되는 동안 유저 프로세스는 자신의 작업을 중단한채 대기한다.
 
![image](https://user-images.githubusercontent.com/32594290/101231541-5a991e80-36ef-11eb-85ae-ac3ee8553e55.png)

#### How?

 1. 유저는 커널에게 read 작업을 요청

 2. 데이터가 입력 될 때까지 대기

 3. 데이터가 입력되면 유저에게 결과가 전달되어야 유저 자신의 작업에 복귀 가능

말 그대로 Block되고, 어플리케이션에서 다른 작업을 수행하지 못한채 대기하므로 자원 낭비가 발생한다.

## Non-Blocking Model

 Blocking 방식의 비효율성을 극복하기 위한 방법
 
 I/O 작업이 진행되는 동안 유저 프로세스의 작업을 중단시키지 않는 방법
 
 ![image](https://user-images.githubusercontent.com/32594290/101231578-90d69e00-36ef-11eb-9ecb-f4b459a9a2a0.png)
 
 #### How?
 
 1. 유저가 커널에게 read 작업 요청
 
 2. 데이터가 입력 되었든 되지 않았든, 요청하는 순간 바로 결과가 반환
 
    - 이 때, 입력 데이터가 없으면 입력 데이터가 없다는 결과 메시지(EWOULDBLOCK)를 반환
 
 3. 입력 데이터가 있을 때까지 1, 2번 과정을 반복 (2번에서 결과 메시지를 받은 유저는 다른 작업을 진행)
 
 4. 입력 데이터가 있으면 유저에게 결과가 전달
 
    - 이 경우 I/O 진행 시간과 관계가 없기 때문에(대기 x) 어플리케이션에서 작업을 오랜시간 중지하지 않고도 I/O 작업을 진행할 수 있음
 
### I/O 이벤트 통지 모델

 Non-Blocking에서 제기된 문제를 해결하기 위해 고안된 모델
 
 매번 호출 측에서 데이터가 준비되었는지 확인하는 것은 비효율적이므로 수신 버퍼나 출력 버퍼에서 이벤트를 통지하는 방법
 
 I/O 작업 결과 반환 방식에 따라 동기, 비동기 모델로 구분할 수 있음
 
### Synchronous Model

 I/O 작업이 진행되는 동안 유저 프로세스는 결과를 기다렸다가 이벤트를 직접 처리하는 방식
 
 Notify를 유저 프로세스(호출 측)가 담당하여 주체적으로 진행하며, 커널은 유저 프로세스의 요청에 수동적으로 응답
 
### Asynchronous Model

 I/O 작업이 진행되는 동안 유저 프로세스는 자신의 일을 하다가 **이벤트 핸들러**에 의해 알람이 오면 처리하는 방식
 
 Notify를 커널이 담당하여 주체적으로 진행(콜백을 넘겨주는 형태)하고 유저 프로세스(호출 측)는 수동적인 입장에서 통지가 오면
 그 때 I/O 결과를 반환 받는다.
 
### Non-Blocking I/O 내부에서는 어떻게 처리되나?

 대부분 Non-Blocking 프레임워크들은 무한 루프를 통해 응답데이터가 존재하는지 지속적으로 확인(=Poll)하며 이를 **이벤트 루프**라고 한다.
 
 OS는 이벤트 루프를 생성하기 위해 커널 수준의 API를 제공한다.
 
 이벤트 루프를 통해서 Socket에서 읽을 데이터가 있는지 계속 확인한다.
 
- - -
 
## Non-Blocking, Blocking + Sync, Async

 ![image](https://user-images.githubusercontent.com/32594290/101231815-1575ec00-36f1-11eb-91f0-d90d033b3b90.png)

 위의 정보를 바탕으로 Blocking & Sync, Non-Blocking & Async는 이해가 쉽지만 Non-Blocking & Sync, Blocking & Async는 설명이 필요하다.
 
 - - -
 
### Non-Blocking & Sync

![image](https://user-images.githubusercontent.com/32594290/101231839-36d6d800-36f1-11eb-810b-5e886c23364a.png)

 Non-Blocking은 바로 어플리케이션이 다른 일을 할 수 있도록 제어권을 받으며 Sync는 작업 완료 여부를 유저프로세스측에서 신경쓴다.
 
 그림을 보면 호출을 하고 바로 반환이되어 다른일을 수행하며 이후 작업이 완료되었는지 계속 물어보는 일을 추가로 하는 것이 
 Non-Blocking & Sync 이다.

- - -
 
### Blocking & Async

![image](https://user-images.githubusercontent.com/32594290/101231870-61289580-36f1-11eb-8afd-6e0d1f6d139e.png)

Blocking은 작업이 완료될 때 까지 제어권을 호출측에서 가지고 있고, Async는 자업 완료 여부를 호출된 측에서 신경을 쓴다.
제어권이 없는 상태에서 결과만 기다리는 Blocking & Sync와 별 다른 차이가 없고 이점도 없어서 일부러 이 방식을 사용할 필요는 없다.

다만, 의도하지 않게 NonBlocking-Async를 추구하다 Blocking-Async가 되어버리는 경우가 있다.

Node.js와 MySQL 조합이 바로 그것이다.

Node.js 쪽에서 Callback 지옥을 헤치면서 Async로 전진해와도 결국 DB 작업 호출시에는 MySQL에서 제공하는 드라이버를 호출하게 되는데,
이 드라이버가 Blocking 방식이다. 

따라서 Blocking-Async는 별다른 장점이 없어 일부러 사용할 필요는 없으나 Non-Blocking & Async 방식을 이용하는데 그 과정 중 하나라도
Blocking으로 동작하는 것이 있다면 의도하지 않게 Blocking & Async로 동작할 수 있다.
 
 
## Ref.
 
 http://homoefficio.github.io/2017/02/19/Blocking-NonBlocking-Synchronous-Asynchronous/
 
 
