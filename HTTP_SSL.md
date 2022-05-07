## HTTPS vs HTTP

  HTTPS는 Hypertext Trasfer Protocol의 약자다. 즉 Hypertext인 HTML을 전송하기 위한 통신규약을 의미한다. HTTPS에서 S는 Over Secure Socket Layer의 약자로 Secure라는 말을 통해서 알 수 있듯이 보안이 강화된 HTTP라는 것을 짐작할 수 있다. HTTP는 암호화되지 않은 방법으로 데이터를 전송하기 때문에 서버와 클라이언트가 주고 받는 메시지를 제3자가 조작하거나 감청할 수 있습니다. 이를 보안한 것이 HTTPS입니다. 로그인할 때 아이디와 비밀번호가 HTTPS프로토콜로 전송이 되기 때문에 그 정보는 제3자로부터 안전할 수 있습니다. 그래서 서비스를 운영하는 입장에서도 HTTPS가 중요하지만 서비스를 이용하는 입장에서도 HTTPS가 중요합니다.
  
## HTTPS와 SSL

  HTTPS와 SSL를 같은 의미로 이해하고 있는 경우가 많은데 맞기도하고 틀리기도 합니다. 인터넷과 웹같은 관계라 말해도 무방합니다. 웹이 인터넷 위에서 돌아가는 거서럼 HTTPS도 SSL 프로토콜 위에서 돌아가는 프로토콜입니다.

  ![image](https://user-images.githubusercontent.com/79847020/167242319-6d4fb4c7-5676-476b-8dee-22fd0f669db7.png)
  
  HTTP가 제일 위에 있고 그 밑에 SSL/TLS라는 것이 있습니다. 그 밑에 TCP/IP가 존재합니다. 네트워크 통신할 때는 계층적으로 레이어라는 것이 구성되어 있어서 SSL 위에 HTTP, FTP, Telnet이 있습니다. SSL은 포괄적인 것이고 SSL이라는 통신 방법 위에서 동작하는 서비스 중 하나가 HTTP이고 HTTP가 SSL 위에서 동작하게 되면 HTTPS가 되는 것이다라고 생각하시면 됩니다. 
  
## SSL과 TLS

  같은 말이다. 네스케이프에 의해서 SSL이 발명되었고 점차 폭넓게 사용되다가 표준화 기구인 IETF의 관리로 변경되면서 TLS라는 이름으로 바뀌었다. TLS1.0은 SSL3.0을 계승한다. 하지만 TLS라는 이름보다 SSL이라는 이름이 훨씬 많이 사용되고 있다.
  
  
  
  
  
  
    


 


