# NGINX

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
