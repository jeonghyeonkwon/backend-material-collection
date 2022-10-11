# Redis 관련 정리

- HashMap의 key-value 형태
- 메모리에 더 많이 접근하고 덜 자주 바뀌는 데이터를 저장(in memory database - cache)

## 도커로 실행

```bash

docker run --name redis -d -p 6379:6379 redis

docker exec -it redis /bin/sh

# cli 접속
redis-cli

## cli 외부 접속
redis-cli -h 호스트_명 -p 포트


```

### cli CRUD

```bash
# 값 저장
set 키 벨류
set 키1 벨류1 키2 벨류2

# 키로 값 찾기
get 키


# 키들 찾기
keys *
keys *키* # 앞 뒤로 포함된 글자의 키 들

# 여러개의 key들의 값 찾기 (mget)
mget 키1 키2

# 제한 시간동안 키-값 유지 (setex)
setex 키 초 값

# setex로 생성한 키들 제한시간 남은 시간 확인 (ttl, pttl)

ttl 키 # 초단위 (없을 시 (integer) -2)
pttl 키 # 밀리세컨드 (없을 시 (integer) -2)

# 키 삭제 (del)
del 키
# 키가 존재해서 삭제 완료하면 (integer) 1
# 키가 존재하지 않아서 실패하면 (integer) 0

# 모든 key 삭제
flushall

# 키 이름 변경
rename 키_이름 바꿀_키이름 # 있을 경우 덮어 씀
renamenx 키_이름 바꿀_키이름 # 있을 경우 (integer) 0 으로 실패


```
