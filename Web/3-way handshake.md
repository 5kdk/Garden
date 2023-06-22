# **3-way handshake**

3-way handshake은 네트워크 통신에서 TCP/IP 프로토콜을 사용하여 연결을 설정하는 방법이다.

TCP (Transmission Control Protocol)는 인터넷에서 신뢰성 있는 데이터 전송을 위해 사용되는 프로토콜이다.

<br>

3-way handshake은 클라이언트와 서버 간에 세션을 설정하기 위해 다음과 같은 세 단계를 거친다.

1. **클라이언트에서 서버로 SYN 보내기 (SYN Sent)**

   - 클라이언트가 서버에 연결을 요청하기 위해 SYN(Synchronize) 패킷을 보냄.
   - 이 패킷은 클라이언트가 서버와 통신을 시작하려는 것을 알리는 역할을 한다.
   - 클라이언트는 SYN 패킷을 보낸 후 SYN_SENT 상태로 전환된다.

2. **서버에서 클라이언트로 SYN-ACK 보내기 (SYN Received)**

   - 서버는 클라이언트로부터 받은 SYN 패킷에 응답하기 위해 SYN-ACK(Synchronize-Acknowledgment) 패킷을 보낸다.
   - 이 패킷은 서버가 클라이언트의 요청을 수락하고 통신을 시작할 준비가 되었다는 것을 알리는 역할을 한다.
   - 서버는 SYN-ACK 패킷을 보낸 후 SYN_RECEIVED 상태로 전환된다.

3. **클라이언트에서 서버로 ACK 보내기 (Established)**

   - 클라이언트는 서버로부터 받은 SYN-ACK 패킷에 대한 응답으로 ACK(Acknowledgment) 패킷을 보낸다.
   - 이 패킷은 클라이언트가 서버의 응답을 받았으며, 통신이 성공적으로 설정되었음을 알리는 역할을 한다.
   - 서버는 ACK 패킷을 받은 후 ESTABLISHED 상태로 전환되며, 클라이언트도 ESTABLISHED 상태로 전환된다.
   - 이제 클라이언트와 서버는 데이터를 주고받을 수 있는 상태가 된다.

이와 같은 3-way handshaking 과정을 통해 클라이언트와 서버는 신뢰성 있는 연결을 설정하고 데이터를 안정적으로 교환할 수 있게 된다.
