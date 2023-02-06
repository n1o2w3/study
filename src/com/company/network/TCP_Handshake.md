## TCP 3-way Handshake

---
TCP는 장치들 사이에 논리적인 접속을 성립(establish)하기 위하여
three-way handshake를 사용한다.

TCP/IP 프로토콜을 이용해서 통신을 하는 응용프로그램이 데이터를 전송하기 전에 먼저 **정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션을 수립하는 과정**을 의미한다.

Client > Server : TCP SYN
Server > Client : TCP SYN ACK
Client > Server : TCP ACK

이러한 절차는 TCP 접속을 성공적으로 성립하기 위하여 반드시 필요하다.

### TCP의 3-way Handshake 역할

양쪽 모두 **데이터를 전송할 준비가 되었다는 것을 보장**하고, 실제로 데이터 전달이 시작하기전에 한쪽이 **다른 쪽이 준비되었다는 것을 알 수 있도록 한다**.

양쪽 모두 상대편에 대한 초기 순차일련번호를 얻을 수 있도록 한다.

![](https://velog.velcdn.com/images/kimnow/post/2be638e0-5588-48fa-a42f-039ffefd69a9/image.png)

#### 3-way Handshaking 과정

1. A클라이언트는 B서버에 접속을 요청하는 **SYN 패킷**을 보낸다.
   이 때 A클라이언트는 SYN을 보내고 SYN/ACK 응답을 기다리는 SYN_SENT 상태가 됨.
2. B서버는 SYN요청을 받고 A클라이언트에게 요청을 수락한다는 ACK와 SYN flag가 설정된 패킷을 발송하고, A가 다시 ACK으로 응답하기를 기다린다. 이 때 B서버는 SYN_RECEIVED 상태가 됨.
3. A클라이언트는 B서버에거 ACK를 보내고 이후로부터는 연결이 이루어지고 데이터가 오가게 되는 것이다, 이 때의 B서버 상태는 ESTABLSHED이다.

위와 같은 방식으로 통신하는 것이 신뢰성 있는 연결을 맺어 준다는 TCP의 3 Way handshake 방식이다.

<br>

## 4-way Handshaking

---

3-Way handshake는 TCP의 연결을 초기화 할 때 사용한다면, 4-Way handshake는 세션을 종료하기 위해 수행되는 절차이다.

![](https://velog.velcdn.com/images/kimnow/post/2725ca54-5dc1-42e8-a14f-9315a6e45cc2/image.png)


### 4-way Handshaking 과정
1. 클라이언트가 연결을 종료하겠다는 FIN플래그를 전송
2. 서버는 일단 확인메세지를 보내고 자신의 통신이 끝날 때 까지 기다리는데 이 상태가 CLOSE_WAIT 상태
3. 서버가 통신이 끝났으면 연결이 종료되었다고 클라이언트에게 FIN플래그를 전송
4. 클라이언트는 확인 했다는 메세지를 보냄.

만약 Server에서 FIN을 전송하기 전에 전송한 패킷이 **Routing 지연이나 패킷 유실로 인한 재전송 등**으로 인해 FIN패킷보다 늦게 도착하는 상황이 발생한다면

Client에서 세션을 종료시킨 후 뒤늦게 도착하는 패킷이 있다면 이 패킷은 Drop되고 데이터는 유실 될 것이다.
이러한 현상을 대비하여 Client는 Server로부터 FIN을 수신하게 되더라도 일정시간(디폴트 240초)동안 세션을 남겨놓고 **잉여 패킷을 기다리는 과정**을 거치게 되는데, 이 과정을 **TIME_WAIT**이라고 함