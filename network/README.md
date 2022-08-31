# 네트워크 관련

## 스위칭

### 서킷 스위칭

- A에서 B로 2진수로 다이렉트로 바로 보낸다
- 출발지와 목적지만 있다.
- 전화와 같은 실시간 통신에만 사용한다.
- 단점
  - 상대방이 많아지면 선이 늘어난다.

### 패킷 스위칭

- 패킷 단위로 쪼개서 보내고 헤더에 출발지와 목적지가 포함되어 있다
- 중간 라우터들을 거쳐서 목적지 까지 도착

## IP 관련

- 공인 IP
  - 공유기
- 사설 IP

  - 공인 IP 1개 내부에 사용하는 여러개의 사설 IP -> 공인IP(1) : 사설IP(N)

- DHCP 할당
  - 남는 IP를 부여해준다.(남은 것을 부여 하므로 IP가 유동적으로 변한다.)

### IPv4

- 32비트의 길이의 식별자로 8비트의 4부분(8비트, 8비트, 8비트, 8비트)으로 10진수로 표시
- 0.0.0.0 부터 255.255.255.255 까지 약 42억 9천개

### IPv6

- 128 비트의 길이의 식별자로 16비트씩 8부분(16비트,16비트, ..., 16비트)의 16진수로 표시
- IPv4로는 인터넷 사용자수가 급증하면서 대체제로 등장
- 사물 인터넷에 사용

### 출처

- [https://swdevelopment.tistory.com/45](https://swdevelopment.tistory.com/45)
- [https://m.blog.naver.com/hostinggodo/220589113088](https://m.blog.naver.com/hostinggodo/220589113088)
- [강좌 - 개발자를 위한 AWS DevOps 입문 [CI/CD 무중단 배포]](https://easyupclass.e-itwill.com/course/course_view.jsp?id=74&rtype=0&ch=course)
