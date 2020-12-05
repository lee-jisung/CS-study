# DHCP (Dynamic Host Configuration Protocol)

클라이언트에게 네트워크 할당이나 IP 주소 할당을 자동화 하는 역할 수행

클라이언트가 DHCP 서버에 요청을 보내면 해당 서버는 클라이언트에게 서버가 가지고 있는 IP Pool에서 사용 가능한 IP를 할당

DHCP는 보통 라우터에 같이 탑재되지만, 별도로 서버에도 설치가 가능

DHCP 기능 3가지 - IP 임대, IP 갱신, IP 반환

## 임대 (Lease)

 DHCP가 클라이언트에게 특정 기간 동안 IP를 빌려주는 것
 
 임대 기간은 보통 8일 정도이며 임대 기간이 끝나면 DHCP의 IP Pool로 반환한다.
 
 유동인구가 많은 도심지나 시내의 경우 이 기간을 짧게 (2~3시간) 설정하며 IP가 빠른 회전률을 갖도록 한다.
 
### DHCP의 IP 할당 순서

#### 1. DHCP Discover

  - IP를 가지지 않는 클라이언트는 MAC 주소를 기반으로 IP를 할당받는다. IP를 할당받기 위해 로컬 네트워크의 Discover packet을 Broadcast한다.
  Discover packet이 broadcast되는 경우, 해당 호스트가 속한 네트워크의 모든 호스트에게 패킷이 전달되지만, DHCP 서버를 제외한 모든 호스트는
  패킷을 파기하고 DHCP에 Discover packet이 정상적으로 도착한 경우 다음 단계로 넘어간다.
  
#### 2. DHCP Offer

  - 현재 DHCP 서버는 클라이언트로부터 Discover packet을 수신한 상태이며, 그 후 offer packet을 broadcast한다. MAC 주소에는 Discover packet을
  발행한 호스트의 MAC주소를 담아 송신한다.
  
#### 3. DHCP Request

  - 클라이언트가 DHCP 서버로부터 Offer packet을 받고, 같은 네트워크 안에 DHCP 서버가 존재한다고 판단한다. 클라이언트는 DHCP로부터 IP를 할당
  받기 위해 request packet을 송신하며 DHCP는 클라이언트가 보낸 request를 받는다.
  
#### 4. DHCP ACK

  - DHCP가 클라이언트가 보낸 Request를 수신하면 IP pool에서 할당할 수 있는 IP를 찾는다. 보통 IP 주소는 192.168.0.2 ~ 192.168.0.254까지
  임대 Pool에 넣어둔다. 이 경우 일반적으로 IP 할당은 가장 앞쪽에 비어 있는 IP부터 할당한다. 할당한 IP를 찾으면 IP 주소를 packet에 담아
  Response를 보내고 클라이언트가 정상적으로 패킷을 수신하면 IP가 할당된다.
  
  ![image](https://user-images.githubusercontent.com/32594290/101234397-46125180-3702-11eb-946e-2260be26bc34.png)

  
위의 DHCP의 IP 할당 과정을 "DORA"라고 한다.


### 하나의 네트워크에 DHCP 서버가 여러대인 경우

  DHCP 4단계에 걸쳐 IP를 할당하는 이유는 위와 같은 경우 때문이다. 하나의 네트워크에 여러대의 DHCP가 존재하는 경우 네트워크 내의 모든 DHCP
  서버에 Discover packet이 송신된다. 패킷을 받은 DHCP 서버는 해당 패킷에 등록된 MAC주소로 응답하고, 클라이언트느 다수의 offer packet을 받게 된다.
  하지만 클라이언트는 하나의 offer packet에만 응답하기 때문에 한대의 DHCP 서버로부터 하나의 IP만 할당 받는다.
  
### 고정 IP를 사용하는 경우
  
  DHCP는 해당 서버에서 임대 가능한 IP Pool 범위 내에서 IP를 할당하기 때문에 사용중인 IP인지 아닌지에 대한 정확한 정보를 알지 못한다.
  만약 고정 IP로 사용중인 IP를 클라이언트에게 할당해준다면 IP가 충돌하는 현상이 일어난다.
  
  이런 경우 IP 주소 Pool로 옮기거나 DHCP 예약 기능을 통해 해당 IP의 임대기간을 -1로 설정하면 무한대에 가까운 임대 기간동안 IP를 이용할 수 있따.
  
- - -

## 갱신 (Renewal)

 IP 갱신의 경우 위의 "DORA"과정 중에서 RA에 해당하는 부분만 재실행 된다. IP가 정상적으로 임대된 경우 IP 임대 갱신은 총 2번 발생한다.
 
 첫 번째 임대 갱신은 임대 시간이 50% 지났을 때, 두 번째 임대 갱신은 87.5% 지났을 때이다. 
 
 왜 임대 갱신을 2번 하는가? DHCP의 임대 갱신이 두번인 이유는 임대 갱신 동안 네트워크의 불안정이나 DHCP 서버의 불안정으로 인해 제대로
 갱신이 이루어지지 않을 경우를 대비하기 위함이다.
 
- - -

## 반환 (Release)

 IP 임대기간이 끝나게 되면 IP를 DHCP 서버에 반환한다. 
 
 
![image](https://user-images.githubusercontent.com/32594290/101234354-e3b95100-3701-11eb-8081-62196d5e2c53.png)

 위의 그림은 임대, 갱신, 반환이 이루어지는 모습이다.
 
 
- - -

## DHCP Relay Agent

 일반적으로 Boradcast packet은 라우터(게이트웨이)밖의 영역으로 전달되지 못한다. 가정에 존재하는 PC의 경우 공유기로부터 사설 IP를 할당 받아
 사용하기 때문에 DHCP 서버가 라우터 밖에 존재하는 경우가 없으나, 기업의 경우 DHCP가 라우터 밖에 존재하는 경우가 많다.
 
 이 경우 packet이 라우터 바깥 영역으로 전달될 수 있도록 Unicast로 변환하여 전송해야 한다. DHCP Relay Agent가 이러한 역할을 해준다.
 
 ![image](https://user-images.githubusercontent.com/32594290/101234422-70fca580-3702-11eb-8e70-304437705da1.png)


## DDNS: Dynamic DNS

  ![image](https://user-images.githubusercontent.com/32594290/101234452-b6b96e00-3702-11eb-8a33-a9e3c610ad20.png)

 DHCP는 어느 클라이언트에 어떤 IP를 할당했는지 추적이 까다로워 클라이언트를 특정하기 곤란하다. 그래서 Dynamic DNS로 DHCP 서버와 DNS 서버를 
 연계시켜 클라이언트에 할당한 IP 주소와 호스트 이름을 DNS에 등록하고 호스트 이름으로 Access Log를 기록할 수 있게 한다.
 
 또한, 최근 클라우드 환경의 보급으로 서버의 생성과 삭제가 빈번하게 일어난다. 그래서 고정 IP 주소 사용이 일반적이던 서버에서도
 DHCP로 설정하는 일이 잦아졌다. DHCP 서버와 DNS 서버의 연계로 IP 주소가 바뀌어도 같은 호스트 이름으로 계속 Access 할 수 있게 된다.
 
 
 
 
 
 
 
 
 
