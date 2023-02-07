## Stateful

---

server side에 **client와 server의 동작, 상태정보를 저장**하는 형태
세션 상태에 기반하여 Client에 response를 보냄.

TCP는 3-way handshake 과정에서 SYN과 SYNACK을 주고 받으며 **양단간 세션 상태를**
**established한 상태**로 만든다

세션 상태가 established가 되면 client와 server는 데이터를 주고 받을 수 있다.

이렇게 TCP는 **세션 상태**에 따라 server의 응답이 달라지는 **Stateful 프로토콜**이다.

![](https://velog.velcdn.com/images/kimnow/post/2be638e0-5588-48fa-a42f-039ffefd69a9/image.png)

## Stateless

---

server side에 client와 server의 동작, **상태정보를 저장하지 않는 형태**

server의 응답이 **client와의 세션 상태와 동립적**이다.

장점으로는 서버가 client 정보를 저장, 관리 하지 않으므로 Scaling이 자유롭다

UDP는 Client의 세션 상태와 관계없이 요청에 대한 응답만을 수행하고,

server가 client의 정보를 저장하지 않는다.

