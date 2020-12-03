# UDP

### UDP란?

 - 데이터를 데이터그램 단위로 처리하는 프로토콜
 
 - 비연결형, 신뢰성 없는 전송 프로토콜
 
 - 데이터그램 단위로 쪼개면서 전송해야 하기 때문에 Transport layer에서 사용하는 프로토콜
 

### TCP와 UDP는 왜 만들어졌나?

 - IP의 역할은 Host to Host (장치 ⇒ 장치)만을 지원한다. *"장치 ⇒ 장치"* 이동은 IP로 해결 되지만, 하나의 장비 안에서 수많은 프로그램들이 
 통신하는 경우 IP만으로는 한계가 있다. 이러한 이유로 **Port**가 나오게 되었다.
 
 - 또한, IP에서 오류가 발생한다면 ICMP에서 알려준다. 하지만 ICMP는 알려주기만 할 뿐 대처를 못하기 때문에 IP보다 윗단계에서 처리를 해줘야 한다.
 이러한 이유로 **TCP & UDP**가 나오게 되었다.
 
   - ICMP (Internet Control Message Protocol): 네트워크 컴퓨터 위에서 돌아가는 OS에서 오류 메세지를 전송 받는데 주로 쓰임.
   
### TCP와 UDP가 오류를 어떻게 해결하는가?

 #### TCP
 
   - 데이터 분실, 중복, 순서의 뒤바뀜 등을 자동으로 보정해주어 송수신 데이터의 정확한 전달을 가능하도록 돕는다.
   
 #### UDP
 
   - IP가 제공하는 정도의 수준만 제공하는 IP 상위 계층의 프로토콜
   
   - TCP와 다르게 에러가 날 수 있으며, 재전송이나 순서가 뒤바뀌는 경우가 발생한다. 이 경우 어플리케이션에서 처리해야하는 번거로움이 존재한다.
   
### UDP는 왜 사용하나?

 UDP의 결정적인 장점은 데이터의 **신속성**이다. TCP보다 데이터 처리가 빠르며 주로 **실시간** 방송과 온라인 게임에서 사용된다. 이러한 이유로
 네트워크 환경이 안 좋을 때 끊기는 현상이 발생하기도 한다.
 
### DNS에서 UDP를 사용하는 이유?
 
 DNS는 Application Layer이고, 모든 Application layer protocol은 TCP or UDP 중 하나의 Transport layer protocol을 사용해야 하며
 DNS는 UDP를 사용한다. (DNS port = no.35)
  
 #### 1. DNS의 Request 크기가 작다.
   
  UDP Request에 충분히 담기는 크기이며, DNS query는 Single UDP Request와 Server로부터의 single UDP reply로 구성되어 있다.
   
 #### 2. 3 way handshaking으로 연결을 유지할 필요가 없다. (오버헤드 발생)
  
  TCP를 사용하게 되면 데이터를 송신할 때 까지 세션 확립을 위한 처리를 하고, 송신할 데이터가 수신되었는지 점검하는 과정이 필요하다.
  이 때문에 Protocol overhead가 UDP에 비해서 커지게 된다.
   
 #### 3. Request에 대한 손실은 Application layer에서 제어가 가능하다.
  
  Time out을 추가하거나 Resend의 작업을 수행하면 된다.
   
 But, TCP를 사용하는 경우가 존재함. DNS Request 크기가 512(UDP 제한)을 넘길 때 TCP를 사용한다. 또한, Zone Transfer를 사용해야 하는 경우
 TCP를 사용해야 한다 (Zone Transfer: DNS 서버 간 요청을 주고 받을 때 사용하는 Transfer)
 

- - -

## UDP Header 
   
   ![image](https://user-images.githubusercontent.com/32594290/101026661-849cf480-35ba-11eb-8a67-763b61972cd8.png)

 - Source Port: 시작 포트
 
 - Destination Port: 도착 포트
 
 - Length: 길이
 
 - Checksum: 오류 검출
 
   - 중복 검사의 한 형태로, 오류 정정을 통해 공간이나 시간 속에서 송신된 자료의 무결성을 보호하는 단순한 방법
   
  - 위와 같이 UDP Header가 간단하기 때문에 TCP보다 가볍고 송신 속도가 빠르게 작동된다. 그러나 확인 응답을 못하기 때문에 TCP보다 신뢰도가 떨어진다.
   
   
 ## UDP Example
 
  - RTP(Real Time Protocol)
   
    - RTP에서는 재전송을 하면 안됨. ex, 전화 상에서 "여", "보", "세", "요" 라는 4개의 데이터를 전송했을 때, "세"를 받지 못해 재전송을 하게된다면
    "여보요세"가 될것이다. 차라리 "여보X요"로 전달하는 것이 나은 상황이기 때문에 재전송을 하지 않는다.
    
  - Multicast 
   
    - "1 : N"으로 통신하는 방식에서 한 사람이 데이터를 받지 못했다고 재전송을 요청한다고 가정했을 때, 제대로 받은 다른 사람들도 해당 데이터를
   다시 받아서 처리해야하는 문제점들이 발생하게 된다. 따라서 UDP를 사용해 이러한 문제를 없앤다.
   
