# 스프링, 스프링 부트 관련

## application-\*.yml 관련

-Dspring.profiles.active=\* (ex : 개발 서버이면 -Dspring.profiles.active=dev)

```yml
spring:
    datasource:
        url: jdbc:mariadb://${RDS_NAME}:${RDS_PORT}/${RDS_DB_NAME}
or

spring:
    datasource:
        url: jdbc:mariadb://${rds.hostname}:${rds.port}/${rds.db.name}

#이렇게 설정 가능
```
