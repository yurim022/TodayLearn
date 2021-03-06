 ## netstat 이란?
 
 네트워크 연결상태, 라우팅 테이블, 인터페이스 상태 등을 보여주는 명령어
 
 
 
 ## netstat 옵션
  - n : ip,port 번호
  - a : 모든 네트워크 상태
  - t : TCP 프로토콜만 보여준다
  - U : UDP 프로토콜만 보여준다
  - P : 해당 포트를 사용하는 프로그램과 PID를 보여준다.
  - r : 라우팅 테이블 출력
  - s : 프로토콜별 ( ip, icmp, tcp, udp 등) 통계
  - c : 1초 단위로 연속적으로 출력



##  TCP state

- LISTEN : 서버가 접속 요청을 기다리는 상태
- SYN-SENT : 클라이언트 어플리케이션이 원격 호스트에 연결을 요청한 상태
- SYN_RECEIVED : 서버가 원격 클라이언트로부터 접속을 요구받아 클라이언트에게 응답 후, 클라이언트로부터 확인 메세지를 받지 못한 상태
- ESTABLISHED : 3 way-handshaking 이 완료된 후 서로 연결된 상태
- FIN-WAIT1, CLOSE-WAIT, FIN-WAIT2 : 서버에서 연결을 종료하기 위해 클라이언트에게 종결을 요청하고 회신을 받아 종료하는 과정
- CLOSING : 확인 메시지가 전송 도중 분실된 상태
- TIME-WAIT : 연결은 종료되었지만 분실되었을지 모를 느린 세그먼트를 위해 당분간 소켓을 열어두고 있는 상태
- CLOSED : 완전히 종료

#### netstat -an 결과

![image](https://user-images.githubusercontent.com/45115557/134955481-86594632-d922-41f7-bc64-d5765fce90de.png)
- 맨 위에거는 임의의 요청을 LISTEN 하고 있는 상태,
- 새로운 연결이 있으면 새로운 소켓이 만들어짐 (각 IP:PORT ~ IP: PORT) 가 소켓 단위
