## HTTPS vs HTTP

  HTTPS는 Hypertext Trasfer Protocol의 약자다. 즉 Hypertext인 HTML을 전송하기 위한 통신규약을 의미한다. HTTPS에서 S는 Over Secure Socket Layer의 약자로 Secure라는 말을 통해서 알 수 있듯이 보안이 강화된 HTTP라는 것을 짐작할 수 있다. HTTP는 암호화되지 않은 방법으로 데이터를 전송하기 때문에 서버와 클라이언트가 주고 받는 메시지를 제3자가 조작하거나 감청할 수 있습니다. 이를 보안한 것이 HTTPS입니다. 로그인할 때 아이디와 비밀번호가 HTTPS프로토콜로 전송이 되기 때문에 그 정보는 제3자로부터 안전할 수 있습니다. 그래서 서비스를 운영하는 입장에서도 HTTPS가 중요하지만 서비스를 이용하는 입장에서도 HTTPS가 중요합니다.
  
## HTTPS와 SSL

  HTTPS와 SSL를 같은 의미로 이해하고 있는 경우가 많은데 맞기도하고 틀리기도 합니다. 인터넷과 웹같은 관계라 말해도 무방합니다. 웹이 인터넷 위에서 돌아가는 거서럼 HTTPS도 SSL 프로토콜 위에서 돌아가는 프로토콜입니다.

  ![image](https://user-images.githubusercontent.com/79847020/167242319-6d4fb4c7-5676-476b-8dee-22fd0f669db7.png)
  
  HTTP가 제일 위에 있고 그 밑에 SSL/TLS라는 것이 있습니다. 그 밑에 TCP/IP가 존재합니다. 네트워크 통신할 때는 계층적으로 레이어라는 것이 구성되어 있어서 SSL 위에 HTTP, FTP, Telnet이 있습니다. SSL은 포괄적인 것이고 SSL이라는 통신 방법 위에서 동작하는 서비스 중 하나가 HTTP이고 HTTP가 SSL 위에서 동작하게 되면 HTTPS가 되는 것이다라고 생각하시면 됩니다. 
  
## SSL과 TLS

  같은 말이다. 네스케이프에 의해서 SSL이 발명되었고 점차 폭넓게 사용되다가 표준화 기구인 IETF의 관리로 변경되면서 TLS라는 이름으로 바뀌었다. TLS1.0은 SSL3.0을 계승한다. 하지만 TLS라는 이름보다 SSL이라는 이름이 훨씬 많이 사용되고 있다.
  
## SSL 디지털 인증서

  SSL 인증서는 클라이언트와 서버간의 통신을 제3자가 보증해주는 전자화된 문서다. 클라이언트가 서버에 접속한 직후에 서버는 클라이언트에게 이 인증서 정보를 전달한다. 클라이언트는 이 인증서 정보가 신뢰할 수 있는 것인지를 검증 한 후에 다음 절차를 수행하게 된다. SSL과 SSL 디지털 인증서를 이용했을 때의 이점은 아래와 같다.
  
  1. 통신 내용이 공격자에게 노출되는 것을 막을 수 있다.
  2. 클라이언트가 접속하려는 서버가 신뢰할 수 있는 서버인지를 판단할 수 있다.
  3. 통신 내용의 악의적인 변경을 방지할 수 있다.

## SSL에서 사용하는 암호화의 종류

  통신 내용이 공격자에게 노출 되는 것을 막기 위해서는 무엇을 해야할까요. 암호화가 필요합니다. SSL의 핵심은 암호화다. SSL은 보안과 성능상의 이유로 두가지 암호화 기법을 혼용해서 사용하고 있습니다. 공격자에게 통신 내용이 노출되더라도 공격자가 통신 내용을 해석할 수 없게하고, 목적지에 정상적으로 도달한 경우에는 해석할 수 있도록 하는 것이 암호화입니다. 
  
  내용을 제3자가 알아보지 못하도록 바꾸는 것을 `암호화`, 암호화 된 내용을 다시 원상태로 되돌리는 것을 `복호화`라고 합니다. 그리고 암호화와 복호화를 하기 위해 사용되는 데이터를 `키`라고 합니다. `키`를 가지고 있어야만 암호화와 복호화를 할 수 있습니다. 키가 없다면 내용을 알 수 없도록 하는 것이 암호화의 기본 메커니즘 입니다. 이런 방식의 암호화를 `대칭키`라고 합니다. 대칭키라는 것은 암호화를 할 때와 복호화를 할 때 같은 키를 사용한다는 것을 의미합니다. 
  
## 대칭키

  openssl 프로그램을 사용해서 실습을 해보겠습니다. http://slproweb.com/products/Win32OpenSSL.html 사이트에서 Win64 OpenSSL v1.1.1o를 설치합니다.  
  
  ```
  C:\study\work\https>echo 'this is the plain text' > plaintext.txt;

  C:\study\work\https>dir
  C 드라이브의 볼륨에는 이름이 없습니다.
   볼륨 일련 번호: 1AEF-2526

  C:\study\work\https 디렉터리

  2022-05-07  오후 07:57    <DIR>          .
  2022-05-07  오후 07:57    <DIR>          ..
  2022-05-07  오후 07:57                28 plaintext.txt
                 1개 파일                  28 바이트
                 2개 디렉터리  668,420,591,616 바이트 남음

  C:\study\work\https>openssl enc -e -des3 -salt -in plaintext.txt -out ciphertext.bin;
  enter des-ede3-cbc encryption password:
  Verifying - enter des-ede3-cbc encryption password:
  *** WARNING : deprecated key derivation used.
  Using -iter or -pbkdf2 would be better.
  ```                 
                 
  * openssl enc -e -des3 : des3 방식으로 암호화 함 (des3는 대칭키 암호화 방법 중 하나, SSL에서 사용하는 암호화 기법)
  * -in plaintext.txt -out ciphertext.bin : plaintext.txt 파일을 암호화 한 결과를 ciphertext.bin 파일에 저장함

  ciphertext  
  ```
  Salted__d?럶???	?F?d?9?볾N괩?퉋E?r
  ```
  
  ciphertext.bin는 des-ede3-cbc encryption password로 입력한 Key를 가지고 있지 않은 사람이라면 복호화 할 수 없습니다. 이 방식은 자기가 가지고 있는 Key를 통해서 암호화할 때 사용하는 방법 입니다. 개인이 사용할 때 해당 방법은 유용하게 사용할 수 있습니다. 그런데 ciphertext.bin을 멀리 있는 사람에게 전송을 하는 경우 경유지가 많아지고 ciphertext.bin이 노출될 가능성이 커집니다. 암호화가 되어있기 때문에 누군가가 가로채도 해독하는 것이 어렵습니다. 문제는 암호를 멀리 있는 사람이 복호화하려면 지정했던 Key를 알아야 된다는 겁니다. 멀리있는 사람에게 Key값을 전송해야 된다는 의미이기 때문에 `Key값이 유출될 경우 누구든 복호화할 수 있다는 문제점`이 있습니다. 이것이 대칭키 방식의 가장 치명적인 단점입니다. 그래서 대칭키 암호화 방식을 보완한 공개키 암호화 방식이 도입됩니다. 참고로 SSL은 대칭키 방식과 공개키 방식이 혼합되서 사용됩니다.
  
## 공개키

  공개키 방식과 대칭키 방식의 큰 차이점은 공개키 방식은 Key값이 2개가 있다는 것입니다. 대칭키의 경우 암호화 Key와
  
  
  
  
  
  
  
 
  
  
  

  



  

  
  
  
  
  
  


  
  
  
  
    


 

