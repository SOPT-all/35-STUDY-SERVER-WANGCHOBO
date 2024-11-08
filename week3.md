
## 미미나에서 익힌 개념 + 개인적으로 정리 해보고 싶던 개념

### 1. Transaction (트랜잭션)

트랜잭션은 여러 작업을 하나로 묶어 **"한 번에 모두 성공하거나 모두 실패하는 것"**을 보장하는 개념이다
 - 대표적으로,, 은행에서 돈을 이체하는 예시가 가장 일반적
 - 은행에서 돈을 이체할 때 출금과 입금이 동시에 성공하거나, 둘 다 실패하도록 해야함 (그렇지 않으면 한 쪽에서는 돈이 빠져나가고 다른 쪽에서는 돈이 안 들어오는 문제가 생길 수 있기 때문....) => 트랜잭션은 이런 문제가 발생하지 않도록 해주는 장치

- **주요 특징**: 트랜잭션은 **ACID**(원자성, 일관성, 고립성, 지속성)라는 4가지 성질을 지켜야 한다
  - **원자성**: 모든 작업이 완전히 수행되거나, 아무것도 수행되지 않아야 함.
  - **일관성**: 트랜잭션 실행 전과 후의 데이터 상태가 일관적이어야 함.
  - **고립성**: 다른 트랜잭션에 영향을 주지 않아야 함.
  - **지속성**: 완료된 트랜잭션의 결과는 영구히 반영됨.

---

### 2. HikariCP

데이터베이스 연결 풀 라이브러리

서버에서 데이터베이스에 연결할 때는 시간이 걸리기 때문에, 자주 연결하고 끊는 것은 비효율적..
-> HikariCP는 여러 개의 데이터베이스 연결을 미리 만들어 두고, 필요할 때마다 빠르게 연결을 빌려주는 방식으로 연결 속도를 높이고 서버 자원을 절약하게 도와준다

- **장점**: HikariCP는 **연결 성능이 뛰어나고 메모리를 적게 사용**해서 서버 자원을 효율적으로 관리하는 데 유리함
특히, 대규모 트래픽이 몰리는 서버 환경에서 안정적으로 동작

---

### 3. Master-Slave 구조

Master-Slave 구조는 데이터베이스 서버를 **주(Master) 서버와 부(Slave) 서버**로 나누어 사용하는 방식을 의미함

- **Master 서버**: 데이터를 **쓰고(INSERT, UPDATE, DELETE 등)**, 변경 작업을 담당
- **Slave 서버**: Master 서버의 데이터를 **복제하여 읽기(SELECT) 전용 작업**을 담당

이 구조를 사용하면 읽기와 쓰기 작업을 나눠서 할 수 있기 때문에 **서버 부하를 줄이고, 읽기 작업 속도를 높일 수 있는** 장점이 있다
-> 데이터가 많은 애플리케이션(예: 쇼핑몰)에서 자주 사용된다고 함!

---

# DBC

- **JDBC(Java DataBase Connectivity)**: 데이터베이스에 연결 및 작업을 하기 위한 자바 표준 인터페이스
- 자바는 DBMS(Oracle, MySQL, MongoDB 등)의 종류에 상관 없이 하나의 JDBC API를 이용해서 데이터베이스 작업을 처리
- JDBC는 DB에 접근해서
    - CRUD를 쉽고 효율적이게 할 수 있게 하고
    - 고성능에서의 세련된 메소드를 제공하며 쉽게 프로그래밍 할 수 있게 도와준다

### 장점

- 각 DB 회사들은 자신들의 DB에 맞는 드라이버를 제공한다. 이러한 JDBC 드라이버를 사용하면 JDBC 를 지원하기만 한다면 모든 DB에 접근하는 것이 가능하다.
- SQL 을 지원함으로써 모든 JDBC 데이터소스에 SQL을 통하여 접속할 수 있도록 해준다.
- 대부분의 개발자들이 잘 아는 친숙한 Access 기술로 별도의 학습 없이 적용해 개발이 가능하다.

### 단점

- 안정적이고 유연하지만 로우 레벨 기술로 인식되기도 한다.
- 간단한 SQL 문을 실행하는 데도 코드 중복이 반복적으로 일어나게 된다.
- Connection 과 같은 공유 리소스를 제대로 릴리즈 해주지 않으면 시스템 자원이 바닥나는 버그가 발생할 수도 있다.

---

# JPA

**JPA(Java Persistence API)**는 현재 **ORM(Object Relational Mapping)**의 기술 표준
  - ORM을 사용하기 위한 인터페이스를 모아둔 것

JPA는 애플리케이션과 JDBC 사이에서 동작한다. JPA 내부에서 JDBC API를 사용하여 SQL을 호출하여 DB와 통신한다.

## JPA를 사용하는 이유

### 1. 생산성

JPA를 사용하면 자바 컬렉션에 저장하듯이 JPA 에게 저장할 객체를 전달하면 된다.

반복적인 코드를 개발자가 직접 작성하지 않아도 되며, 데이터베이스 설계 중심을 객체 설계 중심으로 변경할 수 있다.

### 2. 유지보수

필드를 하나만 추가해도 관련된 SQL과 JDBC 코드를 전부 수행해야 했지만 JPA는 이를 대신 처리해주기 때문에 개발자가 유지보수해야하는 코드가 줄어든다.

### 3. 성능

애플리케이션과 데이터베이스 사이에서 성능 최적화 기회를 제공한다.

같은 트랜잭션안에서는 같은 엔티티를 반환하기 때문에 데이터베이스와의 통신 횟수를 줄일 수 있다. 또한, 트랜잭션을 commit하기 전까지 메모리에 쌓고 한번에 SQL을 전송한다.

---

### SpringBoot에서는 왜 Entity를 DTO로 변환시켜 사용할까?

- **엔티티(Entity)**:
  - 데이터베이스의 테이블과 직접 매핑되는 객체
  - 비즈니스 로직을 포함할 수 있음!
  - 데이터베이스와의 상호작용을 위해 설계되며, 엔티티의 변경은 데이터베이스의 스키마에 영향을 미칠 수 있다.

- **DTO(Data Transfer Object)**:
  - 계층간 데이터 교환을 위한 객체
  - 로직을 포함하지 않고, 순수한 데이터만 가지고 있다.
  - 특정 엔티티의 일부 데이터나, 여러 엔티티의 조합된 데이터를 전달하는데 사용할 수 있음

- **엔티티를 직접 사용하면?**
  - 클라에게 불필요한 정보까지 노출될 수 있음 (보안에 민감한 정보까지도..)
  - 엔티티 변경이 API 스펙에 직접 영향을 미친다 → 유지보수 어려워지고, 확장성 제한됨
  - 성능 이슈 (필요한 정보만을 선택적으로 제공 X → 성능 최적화가 어려워짐)



### 예시 코드

```java
// 엔티티 클래스
@Entity
public class User {
    @Id
    private Long id;
    private String username;
    private String password; // 클라이언트에게 노출되면 안 되는 정보
    
    // Getters, Setters, Constructors ...
}

// DTO 클래스
public class UserDTO {
    private Long id;
    private String username; // 필요한 정보만 포함
    
    // Getters, Setters, Constructors ...
}

/// 변환 로직
public UserDTO convertToDTO(User user) {
    return new UserDTO(user.getId(), user.getUsername());
}
```
---


# Dirty Checking (더티 체킹)

JPA에서 트랜잭션 내에서 엔티티가 변경되면 자동으로 데이터베이스에 반영되도록 하는 기능.

- **작동 방식**:
  - 트랜잭션이 시작되고 엔티티가 조회된 상태에서 필드를 변경하면 JPA는 이를 감지하고, 트랜잭션 종료 시점에 변경 사항을 자동으로 데이터베이스에 반영함.
  - JPA는 조회한 엔티티의 초기 상태를 보관하고, 트랜잭션 종료 시 현재 상태와 비교하여 변경된 필드가 있으면 SQL UPDATE 명령을 실행함.
- **효과**: `save()` 호출 없이도 트랜잭션 종료 시점에 변경 사항이 자동으로 커밋되며, 코드가 간결해지고 엔티티의 변경이 데이터베이스와 자동 동기화된다!

## Dirty Checking (더티 체킹) 사용 여부에 따른 코드 비교

### 더티 체킹을 사용하지 않은 코드

```java
public void updateDiaryTitle(Long diaryId, DiaryRequest diaryRequest) {
    Diary diary = diaryRepository.findById(diaryId)
        .orElseThrow(() -> new EntityNotFoundException("Diary not found"));

    // 제목이 존재하고 30자를 넘지 않으면 수정
    if (diaryRequest.getTitle() != null && diaryRequest.getTitle().length() <= 30) {
        diary.setTitle(diaryRequest.getTitle());
        diaryRepository.save(diary); // save()를 호출하여 변경 사항을 명시적으로 저장
    }
}
```

### 더티 체킹을 사용한 코드

```java
@Transactional
public void updateDiaryTitle(Long diaryId, DiaryRequest diaryRequest) {
    Diary diary = diaryRepository.findById(diaryId)
        .orElseThrow(() -> new EntityNotFoundException("Diary not found"));

    // 제목이 존재하고 30자를 넘지 않으면 수정
    if (diaryRequest.getTitle() != null && diaryRequest.getTitle().length() <= 30) {
        diary.setTitle(diaryRequest.getTitle());
        // save()를 호출하지 않아도 트랜잭션 종료 시점에 변경 사항이 자동으로 반영됨
    }
}
```

# DB의 역할

데이터베이스 DB를 통해 데이터를 Disk에 온전히, 영구적으로 저장
이때, Spring과의 의존이 최소화되도록 인프라를 구성해야 함
-> 서버가 죽더라도 두 프로세스 모두 죽지 않도록!

DB와 Spring 각각의 프로세스의 역할을 명확히 분리할 것

# DB의 종류
 
1. RDB 관게형 데이터 베이스
   `join`으로 관계를 맺게 되고, 이를 통해 서버에서 데이터를 정합성 있게 조회
   즉, RDB는 데이터 스키마 간에 관계를 맺게 된다

2. 데이터 식별자
   데이터를 '잘' 저장하기 위해서는 데이터가 잘 정리될 수 있도록 데이터의 UIQUE를 보장해 줘야 한다.
   이때, PK라 불리는 기본키 식별자로 데이터의 유니크를 보장한다.

3. 쿼리
   DB와의 소통을 위해 필요한 언어를 뜻한다.
   관계형 데이터 베이스에 데이터의 저장, 조회, 조작에 사용

3-1) DDL: 데이터 정의어

> Create, Alter, Drop

3-2) DML: 데이터 조작어

> Selece, Insert, Update, Delete

# DB와 Server 실제로 연결하기

+) DB 접근 정보가 담긴 파일 yml이 노출되어서는 안 된다
+) auto-ddl DB데이터가 유실되지 않게 조심해야 한다

Client -> Controller에서 DTO로 받은 뒤 Service로 전달 -> Service에서 Entity로 바꾸어 Repository로 전달
JPA, MySQL dependency를 추가하고, resources 패키지 내부에 application.yml 파일을 추가해 환경세팅하기

# @Transactional 이해하기 - 미미나... 내용

- 트랜잭션은 DB 관리 시스템 혹은 유사한 시스템에서의 상호작용 단위로 데이터 정합성을 보장하기 위한 방법
  -> 동시 접근하는 여러 프로그램 간 격리를 제공
  -> 오류로부터의 복구 허용 및 DB 일관성 있게 유지하는 안정적 작업 단위 제공

- Transaction의 특징 <ACID>
  -> 원자성 + 일관성 + 독립성 + 지속성의 약자를 딴 단어
  -> 트랜잭션이 단일 단위로 처리해 완전히 성공 / 실패 로만 처리되도록 함
  -> 트랜잭션의 성공적 완료 시 언제나 동일한 데이터베이스 상태로 유지
  -> 트랜잭션 수행 시 다른 트랜잭션의 그 어떤 작업도 개입하지 못함
  -> 성공적으로 수행된 트랜잭션의 결과는 영원히 반영되어 있어야 함

- 트랜잭션의 과정
  -> 트랜잭션 시작 -> 비즈니스 로직 실행 -> 트랜잭션 커밋...

- 트랜잭션 사용 이유
  -> 데이터베이스 상태의 변화를 위해 수행하는 작업 단위를 의미
  -> Transactional 어노테이션 추가 시 Dirty Checking을 하게 되고, 이터베이스에 commit을 해서 수정된 사항을 save 없이도 반영할 수 있도록 함
  -> JPA에서는 트랜잭션이 끝나는 시점에 변화가 생긴 모든 엔티티들을 데이터베이스에 자동으로 반영
  +) JPA의 영속성 컨텍스트(Persistence Context)가 수행하는 변경 감지(Dirty Checking)
  간단히 말하자면... Drity Checking은 상태 변경 검사를 의미

- 트랜잭션 언제 사용하면 좋을까?
  -> 주로 비즈니스 로직이 담겨있는 서비스 레이어에서 트랜잭션 처리
  -> 데이터 저장을 하는 레포지토리 레이어에서 읽어온 데이터들을 읽기, 수정, 저장 등의 작업을 하는 곳이 서비스 레이어이기 때문!

- @Transactional(readOnly = true)는?
  -> 조회용 메서드임을 선언하는 것
  -> JPA는 해당 트랜잭션 내에서 조회하는 Entity가 조회용임을 인식, 변경 감지를 위한 Snapshot을 따로 보관하지 않음
  -> 메모리 절약되는 성능상의 이점 존재
