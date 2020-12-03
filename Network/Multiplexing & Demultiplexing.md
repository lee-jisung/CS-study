## Multiplexing & Demultiplexing (in trasport layer)


### Multiplexing
 
 - Send할 때 Transport layer에서 각 program마다 다른 port로 보내지만, network layer에서는 모두 같은 Source & Destination IP address이다.
 즉, 보낼 때 data가 어떤 application에서 보낸 것인지 중요하지 않고 하나로 묶어서 network layer로 내려주는 것임.
 
### Demultiplexing

 - Network layer에서 동일한 Destination IP를 통해 도착한 데이터는 Transport layer에서 Port number를 확인하여 이에 해당하는 process의 
 socket으로 보내는 것.
 
 
### Connectionless (UDP) Demultiplexing

 - Host가 UDP segment를 받으면 Destination IP, port number를 확인
 
 - UPD 에서는 Destination Port number만 보고 Process를 찾아주기 때문에 서로 다른 source가 보냈다고 하더라도 같은 socket과 같은 process로 보낸다.
 
 - Source port number를 적는 이유: response를 보내기 위해서 source port number가 필요함 (demultiplexing에 사용하진 않는다)
 
### Connection-oriented (TCP) Demultiplexing

 - Source IP, Source Port, Destination IP, Destination Port 4개 (4-tuples)를 사용하여 connection (socket)을 구분한다.
 
 - TCP는 Process마다 connection을 맺기 때문에 서로 다른 host가 보낸 데이터의 목적지가 같더라도 connection을 구분해줘야 함
 
 - 동일한 Destination IP, Destination Port라도 Source IP와 Source Port를 이용하여 구분
 
 - 즉, 4-tuples를 사용하여 각각의 **독립된 Socket**을 따로 구분해준다.
