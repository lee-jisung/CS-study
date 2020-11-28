## 3 Way Handshake - 연결 성립

 TCP는 정확한 전송을 보장해야 함. 따라서 통신하기 앞서, 논리적인 접속을 성립하기 위해 3 way handshake 과정을 진행
 
 ![image](https://user-images.githubusercontent.com/32594290/100517541-820f5900-31ce-11eb-926c-ea3a56a769f1.png)
 
 1. Client가 server에게 SYN(x)를 보냄
 
 2. Server가 SYN(x)를 받고, client에게 SYN(y) / ACK(x+1)을 보냄
  
 3. Client는 server로부터 SYN(y) / ACK(x+1)을 받고, ACK(y+1)을 보냄
 
 4. 3번의 통신이 완료되면 연결이 성립.
 
- - - 

## 4 Way Handshake - 연결 해제 (통신이 끝난 후 해제 과정)

![image](https://user-images.githubusercontent.com/32594290/100517532-7459d380-31ce-11eb-9295-a58f09d71d9e.png)

 1. Client는 server에게 연결 종료를 위한 FIN을 보냄
 
 2. Server는 FIN을 받고, Client에게 ACK을 보냄. 이 때, Server는 모든 데이터를 보내기 위해 **Time out** 상태가 됨.
 
 3. Server가 데이터를 모두 보냈다면, 연결이 종료되었다는 FIN flag를 client에게 보냄
 
 4. Client는 FIN을 받고 확인했다는 ACK을 서버에게 보냄. (아직 서버로부터 받지 못한 데이터가 있을 수 있기 때문에 **TIME_WAIT**을 통해 기다림)
 
   - Server는 ACK을 받은 이후 소켓을 닫음 (closed)
   
   - Client는 **TIME_WAIT**이 끝난 후 닫음
   
이렇게 4번의 통신 과정이 완료되면 연결 해제
