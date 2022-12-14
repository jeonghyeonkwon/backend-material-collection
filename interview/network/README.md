# CS 전공 지식 - 네트워크

## OSI 7계층

### 7계층 - 애플리케이션 계층

- 프로그램
- 데이터 단위 : Data
- 응용 프로세스 간의 정보 교환, 전자 메일, 파일 전송 등의 서비스를 제공
- 프로토콜
  - TCP에 사용되는 프로토콜
    - telnet
    - FTP
      - 파일을 전송
    - HTTP
      - 웹서비스 통신의 기초
    - POP
    - SMTP
      - 메일 프로토콜
  - UDP에 사용되는 프로토콜
    - DHCP
    - SNMP
    - DNS
      - 도메인 이름과 IP주소를 매핑해주는 서버

### 6계층 - 표현 계층

- 프로그램
- 데이터 단위 : Data
- 코드 변환, 구문 검색, 데이터 압축 및 암호화

### 5계층 - 세션 계층

- 프로그램
- 데이터 단위 : Data
- 세션 수립, 유지, 종료
- ex) 예를 들어 100MB를 A에서 B로 전송중 45MB에서 연결이 끊김 그러면 45MB부터 다시 시작하여 연결함

### 4계층 - 전송 계층

- 양종단(단말기) 간의 신뢰성 있는 정보 전달
- 데이터 단위 : Header(TCP or UDP)/Data -> Segment(TCP) or Datagram(UDP)
- 프로토콜
  - TCP
    - 내가 보낸 데이터가 손실이 되었는지 확인하고 데이터 순서를 보장
    - 신뢰적
  - UDP
    - 데이터를 보내면 책임지지 않음
    - 신뢰도는 떨어지지만 속도는 좋음
    - 스트리밍에서 사용

### 3계층 - 네트워크 계층

- 비연결형 전달만 하는 역할
- 데이터 단위 : Header/Segment -> 패킷
- 장비
  - 라우터
    - 라우팅을 하는 역할
    - 경로 최적화
- 프로토콜
  - IP
    - IP 주소를 가지고 전송만 함
  - ICMP
    - 라우터를 통과하면서 실패를 하면 실패에 대한 것을 전달
  - ARP
    - 물리계층(1~2계층)으로 IP -> MAC Address로 변환하는 것
  - RARP
    - 물리계층(1~2계층)에서 MAC Address -> IP로 변환하는 것

### 2계층 - 데이터링크 계층

- 인접한 노드간의 신뢰성 있는 정보 전달
- 데이터 단위 : Header/패킷 -> 프레임
- 장비
  - 브릿지
    - 두개의 근거리 통신망(LAN)을 접속할 수 있도록 통신망 연결 장
    - 포트와 포트 사이 다리 역할
  - 스위치

### 1계층 - 물리 계층

- 비트스트림으로 쏘는 역할
- 장치
  - 허브
  - 리피터
    - 약해진 신호(와이파이 등)을 증폭시켜주는 장치

## 출처 및 자료

- [[10분 테코톡] 👍 파즈의 OSI 7 Layer](https://www.youtube.com/watch?v=Fl_PSiIwtEo)
