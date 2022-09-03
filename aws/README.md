# AWS 서비스 관련

## VPC (Virtual Private Cloud - 가상 개인용 클라우드)

- VPC1(외부 엑세스 허용), VPC2(외부 엑세스 금지)라 가정
  - VPC2 에 접근하는법
    - VPC1에서만 접근 가능하게 개방
    - 특정 IP만 접근 가능하게 개방

## Elastic Beanstalk(엘라스틱 빈스톡)

- 기본적인 셋팅이 되어 있는 서비스
- 구성( 기본 이고 설정에 따라 달라짐 )
  - 선택한 1가지 (JAVA, Docker, 등)
  - 샘플 코드 동작 - 포트 5000
  - NGinxX (프록시) - 포트 80
  - 외부 IP 요청 거부
  - 같은 보완 그룹으로 묶인 친구의 요청만 받음
    - 로드밸런서만 접근이 가능하다.
  - 로드밸런서

## RDS

- utf 8 설정
  - ALTER DATABASE db명 CHARACTER SET = 'utf8mb4' COLLATE = 'utf8mb4_general_ci';
  - 다른 시간 관련, utf 관련 등은 파라미터 그룹으로...

## ALB (Application Load Balancer)

- 여러개의 인스턴스의 IP 주소를 알고 적절히 호출 함
- IP주소는 있지만 컨트롤 불가능 (주기적으로 변함)
- 해결법 - 고정 IP를 사용해야 된다
  - NGinX를 이용하고 탄력적 아이피로 구성
  - NLB(Network Load Balancer) 사용 + 탄력적 IP 사용

## 롤링

- 앞단에 로드밸런서 있음

1. 한번에 모두

- 모든 인스턴스의 버전을 업데이트 시킴. 그러므로 동시간에 모든 서버 중단

2. 추가 배치

- 기본적 설정이면 1개의 새로운 인스턴스를 생성하고 배포
- 각 1개의 인스턴스씩 버전을 업데이트 시킴
- 무중단 배포 가능
- 단점
  - 1개라도 에러 떨어지면 버전 업데이트 한 것을 모두 이전 버전으로 롤백

3. 변경 불가능(블루/그린)

- 순간적으로 새로운 인스턴스를 생성해서 새로운 버전으로 업데이트 후 성공이면 새로운 인스턴스로 연결 실패면 그대로
- 단점
  - 1개가 아니라 N개의 인스턴스를 생성하므로 자원이 많이 듬

## 출처 및 자료

- [강좌 - 개발자를 위한 AWS DevOps 입문 [CI/CD 무중단 배포]](https://easyupclass.e-itwill.com/course/course_view.jsp?id=74&rtype=0&ch=course)
