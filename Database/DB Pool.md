## Database Pool

### Connection Pool

 클라이언트의 요청에 따라 각 어플리케이션의 스레드에서 DB에 접근하기 위해서는 Connection이 필요하다.
 
 Connection pool은 이런 Connection을 여러 개 생성해 두고 저장해 놓은 **공간(캐시)**,
 또는 이 공간의 Connection을 필요할 때 꺼내 쓰고 반환하는 **기법**을 말한다.
 
   ![image](https://user-images.githubusercontent.com/32594290/102299500-65c53780-3f96-11eb-8ff4-db23ae647409.png)
 

### DB에 접근하는 단계

 - 웹 컨테이너가 실행되면서 DB와 연결된 Connection 객체들을 미리 생성하여 Pool에 저장
 
 - DB에 요청 시 Pool에서 Connection 객체를 가져와 DB에 접근
 
 - 처리가 끝나면 다시 Pool에 반환
 
  ![image](https://user-images.githubusercontent.com/32594290/102301021-60b5b780-3f99-11eb-9aa9-16b3ae1b4aba.png)

### Connection이 부족하다면?

 모든 요청이 DB에 접근하고 있고 남은 Connection이 없다면, 해당 클라이언트는 대기 상태로 전환시키고 Pool에 Connection이 반환되면
 대기 상태에 있는 클라이언트에게 순차적으로 제공
 
### 사용 이유

 - 매 연결 마다 Connectino 객체를 생성하고 소멸시키는 비용을 줄일 수 있음
 
 - 미리 생성된 Connection 객체를 사용하기 때문에 DB 접근 시간이 단축됨
 
 - DB에 접근하는 Connection의 수를 제한하여 메모리와 DB에 걸리는 부하를 조정할 수 있음
 
### Thread Pool

 Connection pool과 비슷한 개념
 
 매 요청마다 요청을 처리할 Thread를 생성하는 것이 아니라 미리 생성한 pool 내의 Thread를 소멸시키지 않고
 재사용 하여 효율적으로 자원을 활용하는 방법
 
### Thread Pool & Connectino Pool

 WAS에서 Thread Pool과 Connection Pool내의 Thread와 Connection의 수는 직접적으로 메모리와 관련이 있기 때문에, 많이 사용하면 할 수록
 메모리를 많이 점유하게 된다. 그렇다고 반대로 메모리를 위해 적게 지정한다면, 서버에서 많은 요청을 처리하지 못하고 대기할 수 밖에 없다.
 
 보통 WAS의 Thread 수가 Connection 수 보다 많은 것이 좋은데, 그 이유는 모든 요청이 DB에 접근하는 작업이 아니기 때문이다.
 
 
 
