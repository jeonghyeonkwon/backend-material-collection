# DB, SQL 관련

## DB LOCK

- 데이터 베이스의 접근 권한을 막는 것
- 동시에 접근하려고 할 때 일관성이 깨진다.
  - 1개 수량이 남은 물건을 2명의 소비자가 동시에 조회하면 1개 수량이기 때문에 구매 가능하게 된다.
  - 3000원 계좌에 동시에 조회하면 3000원이고 2명의 사용자가 500원, 1000원 각각 입금하게 되면 4500원이 되지 않는다
- 벤더 사 마다 조금씩 Lock 종류와 전략들이 다르다

### Optimistic(낙관적인) Lock

- 기본적으로 충돌이 발생하지 않을 것이라 보고 우선 락을 걸지 않는다.
- 동시에 조회하면 둘 다 1인 version에 커밋할 시 +1로 verion을 올려 준다. 그때 뒤에 커밋한 것에 에러를 터트린다. 버전이 달라졌으므로
- db에서 하는 것이 아닌 application에서 거는 락

### Pessimistic(비관적인) Lock

- 미리 충돌날 것이라 보고 조회 할 때부터 db에 락을 검
- 한 명이 가지고 오면 다른 한 사람은 대기 해야 됨
- 데드락의 위험성
- 종류
  - Shared Lock : 읽을 수는 있지만 UPDATE, DELETE 방지
  - Exclusive Lock : 읽기, 수정, 삭제 모두 방지

### 데드락

- 락으로 인해 발생
- 트랜잭션 1이 A를 락 B를 조회, 트랜잭션 2가 B를 락 A를 조회 하면 발생(A와 B는 각각 1개의 행)

### 참고 자료

[https://www.youtube.com/watch?v=w6sFR3ZM64c&t=306s](https://www.youtube.com/watch?v=w6sFR3ZM64c&t=306s)

## DB 격리 수준

- 오라클은 기본 READ-COMMITTED MY-SQL은 REPEATABLE-READ

* Read Uncomitted

  - 사용 하지 않음
  - 커밋 전에 값만 수정해도 다른 트랜잭션이 조회하면 바뀐 값이 출력된다.

* Read Committed

  - 커밋된 데이터만 읽기
    - 커밋된 값과 트랜잭션 진행 중인 값을 따로 보관
  - 커밋된 데이터만 덮어쓰기
    - 행 단위 잠금 사용
      - 같은 데이터를 수정한 트랜잭션이 끝날때 까지 대기

* Repeatable Read
  - 트랜잭션 동안 같은 데이터를 읽게 함
    - 읽는 시점에 특정 버전에 해당하는 데이터만 읽음

- Serializable

  - 인덱스 잠금이나 조건 기반 잠금 등 사용
    - 업데이트가 일어나고 커밋되기 전까지 다른 update에 대한 것은 다 거부

## 격리수준에 따른 문제

### 커밋 되지 않는 데이터 읽기 ( Dirty Read )

- 기존 재고의 물품 1개에서 A 유저가 물품을 1개 더 추가 하고 갯수에 대한 것도 2로 수정할 때 그 사이에 B 유저가 조회하면 물품은 2개가 조회 되고 갯수에 대한 것은 1개로 조회 되는 것을 볼수 있다

### 커밋 되지 않는 데이터 덮어쓰기 ( Dirty Write )

- 유저 A가 item1, item2의 소유자를 A로 순서대로 변경 사이에 유저 B가 item1, item2의 소유자를 B로 변경하면 item1의 소유자는 B item2의 소유자는 A가 된다.

### 읽는 동안 데이터 변경 (Read Skew)

- A, B 둘 다 포인트 10인 상황에서 유저1이 A와 B의 포인트 조회를 순서대로 실행 시 유저 1가 A, B 포인트를 +1 씩 올린다면 유저 1이 조회한 A와 B의 포인트는 각각 10 11이 된다

### 변경 유실 ( Lost Update )

- 유저1이 글을 조회하고 조회수 0 -> 1로 업데이트 하고 커밋 하기 전에 유저2가 같은 글을 조회하면 조회수는 0이고 조회수를 0 -> 업데이트 되기 때문에 2가 되지 않는 현상

#### 변경 유실에 대한 해결법

- 원자적 연산 방법 : UPDATE table SET readCnt = readCnt + 1 WHERE id = 1; 처럼 사용함 DB가 원자적 연산 지원하는지 확인 필요

- 명시적 잠금 : 조회하는 쿼리가 있을 시 다른 사람이 사용 중인 수정 쿼리를 끝날때 까지 대기 했다가 조회 허용하는 것

- CAS : 버전 값으로 수정하는 방식 유저 1이 조회 업데이트를 하는 사이 유저 2가 조회 업데이트를 하면 버전이 둘 다 1이다 하지만 둘 중 빠른 것이 버전을 2로 올리고 커밋 한다면 나머지 하나는 버전이 1이기 때문에 업데이트가 되지 않는다.

### 출처

- [https://www.youtube.com/watch?v=poyjLx-LOEU&t=4s](https://www.youtube.com/watch?v=poyjLx-LOEU&t=4s)

## JPA @Transactional 관련

- 스프링 AOP를 통해 구현되어 있다.

### 트랜잭션 속성

#### propagation

- 트랙잭션 시작이나 기존 트랜잭션에 참여하는 방법을 결정

- 부모 메소드와 자식 메소드에 각각 트랙잭션을 어떻게 처리 할 것인가? 를 결정

- REQUIRED

  - 기본 전파 속성
  - 트랙잭션이 있다면 참여하고 없다면 새로 참여
  - 부모 메소드나 자식 메소드 둘 중 하나만 잘못되면 다 같이 롤백

- REQUIRES_NEW

  - 항상 새로운 트랙잭션 실행
  - 진행 중인 트랙잭션이 있다면 그 트랜잭션 보류 후 끝나면 실행
  - 새로운 트랙잭션이기 때문에 부모 자식 메서드 간의 예외로 인한 롤백 관계는 없다.
  - 즉 부모가 예외로 롤백이면 부모만 롤백 자식이 예외로 롤백이면 자식만

- SUPPORTS

  - 부모 메소드에 트랜잭션이 있으면 자식 메소드도 그 안에 포함
  - 없다면 자식은 트랜잭션 없이 실행

- NESTED

  - 부모 트랜잭션이 있다면 자식은 중첩 트랜잭션 생성
  - 부모가 롤백이나 커밋되면 자식은 영향을 받음
  - 중첩으로 실행되기 때문에 자식이 롤백 커밋되도 부모는 영향 안받음

- NEVER, NOT_SUPPORTED
  - 트랜잭션을 사용안함
  - NEVER : 부모 메소드에서 트랜잭션이 있다면 예외 발생
  - NOT_SUPPORTED : 부모 메소드에 트랜잭션이 있다면 잠시 보류

* MANDATORY
  - 부모 메소드에 트랜잭션이 있다면 자식 메소드는 트랜잭션 포함
  - 부모 메소드에 트랜잭션이 없다면 예외 터트림

#### isolation

- DB의 격리 수준을 제어하는 옵션
- READ_UNCOMMITTED, READ_COMMITTED, REPEATABLE_READ, SERIALIZABLE

#### read-only

- 트랜잭션 내에서 데이터를 조작하려는 시도를 막는다

#### timeout

- 트랜잭션을 수행하는 시간을 제어할 수 있다. 기본은 제한시간이 없다

#### rollback-for

- 기본적으로 RuntimeException시 롤백
- 타깃이 되는 Exception이 터질시 롤백시키고 싶을때 사용

#### no-rollback-for

- 타깃이 되는 Exception이 터져도 롤백 시키고 싶지 않을때 사용

### 출처

- [https://oingdaddy.tistory.com/28](https://oingdaddy.tistory.com/28)

- [https://www.youtube.com/watch?v=cc4M-GS9DoY&t=601s](https://www.youtube.com/watch?v=cc4M-GS9DoY&t=601s)

## JPA 동시성 문제 해결

### synchronized를 이용해서 문제 해결

#### 장점

- 스레드를 1개 사용하므로 동시성 문제를 해결

#### 단점

- synchronized를 사용하므로 해당 메소드에 @Transactional를 붙이지 못한다

  - @Transactional을 굳이 붙여야 된다면 synchronized로 걸어준 뒤 @Transactional로 처리

  ```java
    public class SynchronizedService {

      @Autowired
      private TransactionalService ts;

      public synchronized void request(){
        ts.request();
      }
    }

    @Component
    public class TransactionalService {

      @Transactional
      public void request(){
        //...
      }

    }
  ```

- 만약 서버의 갯수가 N개 늘어난다면 한 서버당 1개라도 N개의 스레드가 생기므로 사용 안한다.

### 출처 및 자료

- [인프런 - 재고시스템으로 알아보는 동시성이슈 해결방법 강좌](https://www.inflearn.com/course/%EB%8F%99%EC%8B%9C%EC%84%B1%EC%9D%B4%EC%8A%88-%EC%9E%AC%EA%B3%A0%EC%8B%9C%EC%8A%A4%ED%85%9C/dashboard)
- [인프런 - 재고시스템으로 알아보는 동시성이슈 해결방법 강좌 transactional 답변](https://www.inflearn.com/questions/619166)
