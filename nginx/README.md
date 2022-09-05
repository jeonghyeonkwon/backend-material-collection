# NGINX

## 기능

- 메모리 사용량을 줄여준다.
- 동시 요청 처리를 빨리해준다.
- 로드밸런싱 가능
  - 무중단 배포도 가능
- 캐싱 서버로도 가능(설정 한다면)
  - [설정 방법](https://www.joinc.co.kr/w/man/12/nginx/static)
  * [설정시 주의사항](https://jojoldu.tistory.com/60)
  * 클라우드 프론트(AWS 서비스)로 대체 가능

## 아마존 리눅스에서

### 설치

- sudo amazon-linux-stras install nginx1

### 서비스 시작

- sudo service nginx start

### nginx 설정 파일 경로

- /etc/nginx/nginx.conf

### 내용 부분

```bash

    server{
        ...

        location / {

            proxy_pass http://localhost:열어줄 포트
            ... 그 이외의 설정
        }

    }

```

### nginx 서버 재 시작

-sudo service nginx restart
