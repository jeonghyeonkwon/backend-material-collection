# 스프링 데이터 JPA 관련

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

### PessimisticLock(비관적 락) 이용해서 문제 해결

- 조회 할 때부터 다른 트랜잭션이 못 건들게 락

```java
public interface ItemRepository extends JpaRepository<Item,Long>{

  @Lock(value = LockModeType.PESSIMISTIC_WRITE)
  @Query("SELECT i FROM Item i WHERE i.id = :id")
  Item findItemById(Long id);

}
```

#### 비관적 락 LockModeType 속성

| 타입                        | 내용                               |
| --------------------------- | ---------------------------------- |
| PESSIMISTIC_READ            | 비관적 읽기에 락을 검              |
| PESSIMISTIC_WRITE           | 비관적 쓰기에 락을 검              |
| PESSIMISTIC_FORCE_INCREMENT | 비관적 락 + 버전정보를 강제로 증가 |

#### PESSIMISTIC_FORCE_INCREMENT

- 게시글(1) - 첨부 파일(N) 양방향 관계에서 첨부 파일 추가로 인해 게시글의 데이터 수정은 없다. 그래서 version의 변화가 없다. 첨부파일의 변경으로 인해 게시글의 version을 업그레이드 시켜야 된다면 사용

### OPTIMISTICLOCK(낙관적 락) 이용해서 문제 해결

- 엔티티에 버전 필드를 선언하고 버전에 따라 동시성 문제 해결

Entity

```java
@Entity
public class Item{
  @Id
  private Long id;
  ...

  @Version
  private Long version;
  //Long, Integer, Short, Timestamp 가능
  ...

}

```

ItemRepository

```java
public interface ItemRepository extends JpaRepository<Item,Long>{

  @Lock(value = LockModeType.OPTIMISTIC)
  @Query("SELECT i FROM Item i WHERE i.id = :id")
  Item findItemById(Long id);

}


```

기존 서비스 클래스 ItemService이고 itemRepository를 호출할 수 있게 알아서 작성

그리고 버전 정보 차이로 에러가 터지면 다시 요청 할 수 있도록 Facade 클래스 작성

ItemFacade

```java
@Service
private ItemFacade{
  @Autowired
  private ItemService itemService;

  public void request(Long id) throws InterruptedException{
     while(true){
      try{
          itemService.request(id);
          break;
      }catch(Exception e){
        Thread.sleep(50);
      }
    }
  }
}
```

#### 낙관적 락 LockModeType 속성

| 타입                       | 내용                       |
| -------------------------- | -------------------------- |
| OPTIMISTIC                 | 낙관적 락 사용             |
| OPTIMISTIC_FORCE_INCREMENT | 낙관적 락 + 버전 강제 증가 |

### NameLock 이용 문제 해결

- 별도의 곳에 Lock을 검

LockRepository

```java

interface LockRepository extends JpaRepository<Item, Long>{
    @Query(value = "SELECT get_lock(:key, 3000)", nativeQuery = true)
    void getLock(String key);

    @Query(value = "SELECT release_lock(:key)", nativeQuery = true)
    void releaseLock(String key);
}

```

```java

@Service
public class NamedLockItemFacade{

  @Autowired
  private LockRepository lockRepository;

  @Autowired
  private ItemService itemService;

  @Transactional
  public void resuest(Long id, Dto dto){
    try{
      lockRepository.getLock(id.toString());
      itemService.request(id,dto);
    }finally{
      lockRepository.releaseLock(id.toString());
    }
  }
}
```

ItemService

```java
...

@Transactional(propagation = Propagation.REQUIRES_NEW)
public void request(Long id, Dto dto){
    ...
}
```

### 출처 및 자료

- 도서 - 자바 ORM 표준 JPA 프로그래밍
- [인프런 강좌 - 재고시스템으로 알아보는 동시성이슈 해결방법 강좌](https://www.inflearn.com/course/%EB%8F%99%EC%8B%9C%EC%84%B1%EC%9D%B4%EC%8A%88-%EC%9E%AC%EA%B3%A0%EC%8B%9C%EC%8A%A4%ED%85%9C/dashboard)
- [인프런 질문 - 재고시스템으로 알아보는 동시성이슈 해결방법 강좌 transactional 내용](https://www.inflearn.com/questions/619166)
