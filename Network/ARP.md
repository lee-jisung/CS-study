# ARP (Address Resolution Protocol)

네트워크 상에서 IP 주소를 물리적 네트워크 주소(MAC)로 대응(bind)시키기 위해서 사용되는 프로토콜이다.

ARP를 사용하는 이유는 로컬 네트워크 (LAN, Local Area Network)에서 단말과 단말 간 통신을 위해서 IP 주소 뿐만 아니라 MAC주소를 
이용하게 되는데, IP를 MAC Address로 매칭하여 목적지 IP의 MAC Address를 제대로 찾아가기 위함이다.

왜 IP 주소를 MAC 주소로 매칭해야할까? 그 이유를 알기 위해 LAN과 MAC주소를 먼저 알아보자


## LAN (Local Area Network)

 근거리 통신망, 로컬 영역 네트워크, 구내 정보 통신망은 네트워크 매체를 이용하여 집, 사무실, 학교 등의 건물과 같은 가까운 지역을 한데 묶는
 컴퓨터 네트워크이다. - 위키 -
 
 즉, ARP Request가 미치는 영역이 곧 LAN이라고 볼 수 있다. ARP Packet이 전달되기만 하면 LAN이라고 보는 것이다. 이것은 곧 같은 IP 대역을
 공유하는 LAN에서 단말간 통신을 하기 위해서는 IP 주소를 사용하여 목적지를 지정하지만 실제는 MAC 주소를 이용해 목적지를 찾으며 이때 ARP가
 사용된다는 것을 의미한다.
 
 ![image](https://user-images.githubusercontent.com/32594290/101234552-b4a3df00-3703-11eb-8d6f-2e6a92118368.png)

위의 구성에서 PC0가 PC1과 통신하기 위해서 목적지를 192.168.1.2로 설정하지만, 실제 목적지는 PC1의 MAC주소로 설정한 후 전송된다.


## MAC

 데이터 링크 계층에서 통신을 위한 네트워크 인터페이스에 할당된 고유 식별자로 Network Interface Card(NIC)를 가진 단말이라면
 공장에서 출고될 때 부여되고 평생 사용하는 고유한 주소이다. 
 
 네트워크 장비, PC 등은 모두 MAC 주소를 갖는다. 좀 더 자세히 말하면 네트워크, PC가 갖는 NIC 마다 MAC주소를 갖게 된다. 그리고 LAN에서는
 IP 주소를 MAC주소에 매칭하여 통신한다.
 
 #### 왜 MAC 주소가 필요한가?
 
 IP주소는 끊임없이 변화한다. MAC 주소 체계가 없는 상황을 가정하고 IP 주소만 부여되었다면 PC0 사용자가 자신의 IP를 192.168.1.2로 바꾼다면
 PC0와 PC1 모두 192.168.1.2의 IP를 갖게 될 것이고 원래의 IP 192.168.1.2의 주인이 누구인지 알 수가 없게된다. 이 때문에 MAC주소를 사용한다고
 볼 수 있다.
 
 거꾸로 IP 주소 없이 MAC 주소만을 사용하여 Routing을 하게 된다면 각 고유한 주소를 routing table에 일일이 설정했다간 Router가 다운되고 말것이다.
 

## ARP

 단말간 통신에서 IP를 이용하여 출발지, 목적지를 설정하지만 LAN에서 실제 데이터 이동은 MAC 주소를 이용하여 이루어진다.
 이를 위해 필요한 것이 ARP이며 IP 주소와 MAC 주소를 1:1 매칭하여 LAN에서 목적지를 제대로 찾아갈 수 있또록 돕는다.
 
 IP주소와 MAC주소를 1:1대응하여 테이블로 정리하고, 이를 참조하여 목적지 IP에 맞는 목적지 MAC주소로 전달하는데 이것을 ARP Table이라 부른다.
 
### ARP Table 생성 과정

1. PC 0는 PC 2의 IP 주소를 알고 있는 상황이며 같은 LAN에 속하는 것을 알고 있다. 이에 PC 2의 MAC 주소를 알기 위해 ARP Request를 Broadcast 한다.

2. PC 0는 Broadcast(FF:FF:FF:FF:FF:FF)인 ARP Request를 날리고 PC 1, PC 2, PC 3에 전달된다. 그리고 ARP Request의 목표인 PC 2가 이에
반응하여 ARP Response(PC 2의 MAC 주소)를 보낸다. 다른 PC1, PC3 역시 PC 0의 Broadcast 받아 PC 0의 정보를 적는다.

3. PC 0(192.168.1.1)은 PC 2가 보낸 ARP Response를 받고 ARP Table에 PC2의 IP와 MAC 주소를 적는다. 그리고 데이터를 보내려 목적지 IP를
192.168.1.3으로 지정하면 자연스레 ARP Table을 보고 PC 2의 MAC 주소를 목표로 전달한다.











