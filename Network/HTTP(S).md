## HTTP

 웹 서버와 클라이언트 간 문서를 교환하기 위한 통신 규약
 
 웹에서만 사용하는 프로토콜. TCP/IP 기반으로 서버와 클라이언트간 요청과 응답을 전송
 
### HTTP 특징

 **TCP 기반 통신 방식**
 
 **비연결 지향**
 
  - 브라우저를 통해 사용자의 요청으로 서버와 접속하여 요청에 대한 응답의 데이터를 전송한 후 연결을 종료
  
  - 간단하여 자원이 적게 든다.
  
  - 연결이 지속적이지 않기 때문에 사용자와 연결을 종료한 후 추가적인 요청 시 어떤 사용자의 요청인지 모른다
  
  - 해결 방법: 쿠키, 세션, 히든 폼 필드 등...
  
 **단 방향성**
 
  - 사용자의 요청 한 개에 대해 한 개의 응답을 하는 방식 (서버가 먼저 응답하지 않음)
  
 HTTP는 Keep-alive로 지속 연결이 가능하지만 기본적으로 close이며 Request를 해야만 Response를 받을 수 있는 구조임.
 
 
### HTTP 문제점

#### HTTP는 평문 통신이기 때문에 도청이 가능하다

 TCP/IP는 도청 가능한 네트워크이다.
 
  - TCP/IP구조의 통신은 전부 통신 경로 상에서 엿볼수 있다. 패킷을 수집하는 것만으로 도청을 할 수 있는 것. 평문으로 통신할 경우 메시지의
  의미를 파악할 수 있기 때문에 암호화가 필요하다.
  
 보완 방법
   
  - 통신 자체를 암호화: **SSL(secure socket layer)** or **TLS(transport layer security)** 라는 프로토콜을 조합함으로써 HTTP의 통신 내용을
    암호화 한다. SSL을 조합한 HTTP를 **HTTPS(HTTP Secure)** or HTTP over SSL 이라고 한다.
    
  - 콘텐츠를 암호화: 말 그대로 HTTP를 사용해서 운반하는 내용인 콘텐츠만 암호화 하는 것. 암호화해서 전송하면 받은 측에서는 그 암호를 해독하여
    출력하는 장치가 필요하다.
    
#### 통신 상대를 확인하지 않기 때문에 위장이 가능하다.

 HTTP에 의한 통신에는 상대가 누구인지 확인하는 처리가 없다. 따라서 누구든지 Request를 보낼 수 있다.
 
 IP 주소나 Port 등에서 그 웹서버에 대한 액세스 제한이 없는 경우 Request가 오면 상대가 누구든지 Response를 반환한다. 이러한 특징이 다양한
 문제점을 유발한다.
 
  - Request를 보낸 곳의 웹 서버가 원래 의도한 response를 보내야 하는 웹 서버인지 확인 불가
  
  - Response를 반환한 곳의 클라이언트가 원래 의도한 Request를 보낸 클라이언트인지 확인 불가
  
  - 통신하고 있는 상대가 접근이 허가된 상대인지 확인 불가
  
  - 어디에서 누가 Request를 했는지 확인 불가
  
  - 의미 없는 Request도 수신한다 ⇒ 이것은 DoS 공격을 방지할 수 없음
  
 보완 방법
 
  - SSL로 상대를 확인 ⇒ SSL은 상대를 확인하는 수단으로 **증명서**를 제공하고 있음
  
  - 증명서는 신뢰할 수 있는 제 3자기관에 의해 발행되는 것이기 때문에 서버나 클라이언트가 실재하는 사실을 증명한다. 이 증명서를 이용함으로써
  통신 상대가 내가 통신하고자 하는 서버임을 나타내며 이용자는 개인 정보 유출 등의 위험성이 줄어든다.
  
  - 클라이언트는 이 증명서로 본인 확인을 하고, 웹 사이트 인증에도 이용할 수 있다.

#### 완전성을 증명할 수 없어 변조가 가능하다.
 
 완전성이란 **정보의 정확성**을 의미한다. 서버나 클라이언트에서 수신한 내용이 송신측에서 보낸 내용과 일치한다는 것을 보장할 수가 없다.
 
 Request나 Response가 발신된 후 상대가 수신하는 사이 누군가에 의해 변조되더라도 이 사실을 알수가 없기 때문이다. 이와 같이 공격자가 도중에 
 Request나 Response를 빼앗아 변조하는 공격을 **중간자 공격 (Main-in-the-Middle)** 이라고 한다.
 
 보완 방법
  
  - **MD5**, **SHA-1** 등 해시 값을 확인하는 방법과 디지털 서명을 확인하는 방법이 존재하지만 확실히 확인할 수 있는 것은 아니다.
  
  - 확실한 방지를 위해서는 **HTTPS**를 사용해야 한다. SSL에는 인증이나 암호화, 다이제스트 기능을 제공하고 있다.

- - -

## HTTPS

 HTTP에 암호화한 인증, 완전성 보호를 더한 것이다. HTTPS는 SSL의 껍질을 덮어 쓴 HTTP라고 할 수 있다. 
 
 즉, HTTPS는 새로운 Application layer의 Protocol이 아니다. HTTP 통신하는 소켓 부분을 SSL or TLS 로 대체하는 것이다.
 
 HTTP는 원래 TCP와 직접 통신했지만 HTTPS에서의 HTTP는 SSL과 통신하고, **SSL이 TCP와 통신** 하게 된다. 
 
 SSL을 사용한 HTTPS는 암호화와 증명서, 안정성 보호를 이용할 수 있게 된다.
 
 HTTPS의 SSL에서는 공통키 암호화 방식과 공개키 암호화 방식을 혼합한 하이브리드 암호 시스템을 사용한다. 공통키를 공개키 암호화 방식으로 교환한 다음에
 그 다음 통신은 공통키 암호를 사용하는 방식이다.
 
### 모든 웹 페이지에서 HTTPS를 사용하지 않는 이유

 평문 통신에 비해 암호화 통신은 CPU, 메모리 등 리소스가 많이 필요하다. 통신할 때 마다 암호화를 하면 많은 리소스를 소비하기 때문에 
 서버 한 대당 처리할 수 있는 Request의 수가 줄어들게 된다. 따라서 민감한 정보를 다룰 때만 HTTPS에 의한 암호화 통신을 사용한다.
 
 **cf) HTTP 2.0이 발전되면서 HTTPS가 HTTP보다 빠르다는 내용 
   
   https://tech.ssut.me/https-is-faster-than-http/
 

## SSL

 SSL 프로토콜은 Netscape 사에서 웹 서버와 브라우저 사이의 보안을 위해 만들었다. CA (Certificate Authority)라 불리는 Third party로부터
 서버와 클라이언트의 인증을 하는데 사용된다.
 
 #### Application 서버를 운영하는 기업은 CA를 통해 인증서를 만든다.
 
  - Application 서버를 운영하는 기업은 HTTPS 적용을 위해서 공개키와 개인키를 만든다.
  
  - 그리고 신뢰할 수 있는 CA 기업을 선택하고 인증서 생성을 요청한다.
  
  - CA는 서버의 공개키, 암호화 방법 등의 정보를 담은 인증서를 생성하고 해당 CA의 개인키로 암호화하여 서버에 제공한다.
  
  - 클라이언트가 SSL로 암호화된 페이지 ('https://~~')를 요청시 서버는 인증서를 전송한다.
  
 #### 클라이언트와 서버의 통신 흐름 과정
 
  - 클라이언트가 SSL로 암호화된 페이지를 요청
  
  - 서버는 클라이언트에게 인증서를 전송
  
  - 클라이언트는 인증서가 신용이 있는 CA로 부터 서명된 것인지 판단 ⇒ 브라우저내의 CA 리스트와 해당 CA 공개키를 이용하여 인증서의 복호화가
  가능하다면 접속한 사이트가 CA에 의해 검토되었음을 의미 ⇒ 서버가 신용이 있다고 판단 (공개키가 데이터를 제공한 사람의 신원을 보장해주는 것이므로 이러한 것을 **전자 서명** 이라고 함)
  
  - 클라이언트는 CA의 공개키를 이용해 인증서를 복호화 하고 서버의 공개키를 획득
  
  - 클라이언트는 서버의 공개키를 이용해 랜덤 대칭 암호화 키, 데이터 등을 암호화 하여 서버로 전송
  
  - 서버는 자신의 개인키로 복호화하고 랜덤 대칭 암호화키, 데이터 등을 획득
  
  - 서버는 랜덤 대칭 암호화키를 이용하여 클라이언트의 요청에 대한 응답을 암호화 하여 전송
  
  - 클라이언트는 랜덤 대칭 암호화키를 이용해 복호화 하여 응답 데이터를 이용
  
 #### 인증서에 포함된 내용
 
  - 서버측 공개키 (Public key)
  
  - 공개키 암호화 방법
  
  - 인증서를 사용한 웹 서버의 URL
  
  - 인증서를 발행한 기관(CA) 이름
  
  ![image](https://user-images.githubusercontent.com/32594290/101058013-09463d80-35d0-11eb-815c-ac5ab4cec6c2.png)
