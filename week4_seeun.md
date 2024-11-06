## 왕초보스터디 week4
## week2 - 과제 코드 리뷰

### 1. 유효성 검증 - @Valid
#### 유효성 검증이란?
데이터 필드가 정의한 조건에 대해 올바른 값인지 검증하는 것.

1. 라이브러리 추가
```
dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-validation'
}
```
2. 제약조건 설정
* 대표적으로 자주 사용되는 제약조건 어노테이션
  - @NotNull - Null 불가
  - @Size(min=, max=) - 문자열, 배열 등의 최소, 최대 크기 검증
  - @Min/Max(숫자) - 필드의 최솟값/최댓값을 벗어나는지 검증
  - @Email - 이메일 형식만 가능
  - etc
  - https://reflectoring.io/bean-validation-with-spring-boot/

3. @Valid 어노테이션 지정

검증할 대상의 앞에 @RequestBody와 함께 @Valid 어노테이션을 붙여준다.

4. 예외 처리
   지정한 제약조건에 위배되는 데이터가 들어오는 경우, 스프링부트는 **MethodArgumentNotValidException**을 반환한다.

### 2. 스프링부트에서의 예외 처리
#### Excecption(예외)란?

프로그램 코드에 의해서 수습될 수 있는 다소 미약한 오류
개발자가 예외처리(try - catch문)를 해준다면 예외가 발생하더라도 프로그램의 비정상적인 종료를 막을 수도 있음

Exception 클래스를 Checked Exception(컴파일 예외)와 Unchecked Exception(런타임 예외)로 나눌 수 있다.

- Checked Exception: 컴파일러가 예외처리를 확인하는 예외들
- Unchecked Exception: 컴파일러가 예외처리를 확인하지 않는 예외들

둘의 핵심적인 차이는 “**반드시 예외처리를 해야 하는가”** 이다

- Checked Exception: 체크 시점이 컴파일 단계이기 때문에, 별도의 예외 처리를 하지 않는다면, 컴파일 자체가 되지 않느다. 따라서, 반드시 예외 처리를 해야 한다.
- Unchecked Exception: 상대적으로 미약한 예외로 취급되어 명시적인 예외 처리를 하지 않아도 된다.
    - 일반적으로 프로그램 로직의 에러이기 때문에, 예외 처리보다는 예외 발생하지 않도록 코드를 수정하는 것이 바람직

#### 예외 처리 구문(try - catch)

```
try {
	//예외 발생 가능성 있는 문장
} catch (Exception1 e1) {
	//Exception1이 발생했을 경우, 처리를 위한 문장
} catch (Exception2 e2) {
	//Exception2이 발생했을 경우, 처리를 위한 문장
} finally {
	//예외 발생 여부와 관계없이 항상 실행되는 문장
}
```

#### throw 키워드

throw 키워드를 통해 개발자가 고의로 예외를 발생시킬 수 있다.

- new 연산자로 발생시키고자 하는 예외 클래스 객체 생성
- throw 키워드로 예외 발생시킴

#### 스프링부트의 예외 처리 방식

예외가 발생하면, 예외를 복구해서 정상적으로 처리하기보다는 어떤 문제가 발생했는지 상황을 전달하는 경우가 많다.

이렇게 오류 메시지를 전달하려면, 각 layer에서 발생한 예외를 endpoint level인 `controller`로 전달해야 한다.

- @RestControllerAdvide와 @ExceptionHandler를 통해 모든 컨테이너의 예외 처리
    - 애플리케이션 내 모든 @Controller에서 발생하는 예외를 처리할 수 있게 함
- @ExceptionHandler를 통해 특정 컨트롤러의 예외를 처리
    - @ExceptionHandler를 메서드에 선언하고 특정 예외 클래스를 지정해주면 해당 예외가 발생했을 때 메서드에 정의한 로직으로 처리함

크게 2가지 방식이 있다.

> 즉, @RestControllerAdvide를 통해 전역적으로 예외처리를 할 수 있다.

### 3. DTO - record 타입
#### DTO란?
- Data Transfer Object의 약자로, 데이터를 전달하기 위한 객체이다.
- DTO는 로직을 가지지 않는 순수한 데이터 객체이다.(getter & setter만 가진 클래스)
- 여러 layer 사이에서 데이터를 주고 받을 때 사용할 수 있다.
- DTO의 필요성
    - 엔티티 내부 구현 캡슐화
    - api마다 넘겨주는 데이터가 다를 수 있기 때문에, 엔티티를 반환값으로 사용하면 유지보수가 힘들다.
    - 클라이언트에 전달해야 할 데이터의 크기 조절 가능(엔티티 반환 시, 불필요한 데이터까지 전송될 수 있다)

→ DTO는 class 타입 뿐만 아니라 record 타입으로도 만들 수 있다.

#### Record란?
- 데이터만을 포함하는 불변한 객체를 간단하게 정의할 수 있게 도와주는 데이터 클래스
    - 필드에 따로 final을 선언하지 않아도 필드 값 변경 시 컴파일 오류 발생
- 기존 Java에서 DTO를 정의하기 위해서는 생성자, getter, equals(), hashcode(), toString() 등이 필요했으나, Record는 이러한 불편함을 해소하고 간결한 문법으로 데이터를 표현할 수 있도록 도와준다.(작성하지 않아도 됨)
    - 기본적으로 getter를 제공하며, getX()가 아닌 x()로 네이밍 처리된다
    - equals, hashcode, toString 메서드가 자동으로 제공되며, 서로 다른 record가 필드 값이 같으면 같은 객체로 인식한다
- 상속을 받을 수도, 할 수도 없다.
    - 상속 불가능의 이점
        - 불변성 보장 - 객체의 상태를 변경하는 서브 클래스를 방지해 데이터의 일관성과 안정성을 높인다.
        - 간결성과 명료성 - 클래스 계층 구조가 단순해지고, 이해하기 쉬워 유지보수가 쉽다
        - 추상화의 명확성 - record는 명확하게 데이터 홀더(data holder) 역할을 한다.

#### DTO를 Record 타입으로 사용한 이유
1. 간결성
    - 보일러플레이트 코드를 줄일 수 있다
        - 보일러플레이트 코드: 특정 기능을 구현하기 위해 반복적으로 작성해야 하는 코드 블록
2. 명확한 의도
    - 해당 클래스가 데이터 전용임을 명확히 나타낼 수 있다
3. 불변성
    - Record는 데이터의 불변성을 보장해 오류 발생을 최소화하고 코드의 안정성을 향상시킨다.

## 4. 엔티티 값 update

- save()와 더티 체킹

### [1] save() 함수

reposigory에 id값이 존재하는지 확인(findById)하고, 해당 id의 엔티티의 content를 변경(setContent)하고, save() 함수를 통해 업데이트(save)

```
// id에 해당하는 DiaryEntity가 존재하는지 확인
DiaryEntity diaryEntity = diaryRepository.findById(id)
        .orElseThrow(()-> new NoSuchElementException("존재하지 않는 일기 id입니다."));
diaryEntity.setContent(content);
diaryRepository.save(diaryEntity);
```

> 하지만, 이 경우에는 모든 값을 변경하지 않으면, 변경한 값을 제외한 속성값에는 null값이 입력되어 업데이트되는 오류가 발생할 수 있다.

즉, id, email, password, role, username로 5개의 속성이 있는 엔티티에서 password만 변경한다면, 나머지 4개의 속성에는 null값이 입력되어 문제가 생긴다.
→ 모든 속성값을 변경해 주어야 오류가 생기지 않는다.


### [2] @Transactional과 Dirty Checking

- Dirty란 상태의 변화
- 즉, Dirty Checking이란 상태 변경 검사

JPA에서는 트랜잭션이 끝나는 시점에 변화가 있는 모든 엔티티 객체를 DB에 자동으로 반영해준다. 그렇기 때문에, 값을 변경하고 save하지 않더라도 DB에 반영되는 것이다.

→ 이때 변화가 있다의 기준은 ‘최초 조회 상태’이다.

Dirty Checking은 Transaction 안에서 엔티티의 변경이 일어나면, 변경 내용을 자동으로 DB에 반영하는 JPA 특징이다.

JPA에서는 엔티티를 조회하면 해당 엔티티의 조회 상태 그대로 스냅샷을 만들어놓고, 트랜잭션이 끝나는 시점(트랜잭션 커밋)에 이 스냅샵과 비교해서 다른 점이 있으면 update query를 DB로 전달한다.

### Dirty Checking 조건

→ 상태 변경 검사의 대상은 `영속성 컨텍스트` 가 관리하는 엔티티에만 적용된다.

- 영속 상태의 엔티티

→ Transaction이 커밋되기 전까지 영속성 컨텍스트는 변경사항을 추적하기만 하고, DB에 반영하지는 않는다. 따라서, Transaction이 커밋될 때 엔티티의 변경 상태를 DB에 반영한다.

- @Transactional 어노테이션으로 한 트랜잭션으로 묶어주고 변경해야 적용된다.

#### 영속성 컨텍스트란?

- 엔티티를 영구 저장하는 환경

  → 눈에 보이지 않는 논리적 개념

  → 애플리케이션과 DB 사이 객체를 보관하는 가상의 DB로 생각

  → EntityManager를 통해 Entity를 영속성 컨텍스트에 보관 및 관리, 제거


#### 엔티티의 생명주기

(1) 비영속

- 객체 생성
    - DB에 반영되기 전 처음 생성된 엔티티
- 엔티티가 영속성 컨텍스트에 없는 상태(즉, 영속성 컨텍스트와 관계x)

(2) 영속

- 엔티티가 영속성 컨텍스트에 저장
- 즉, 영속성 컨텍스트가 엔티티를 관리

(3) 준영속

- 엔티티가 영속성 컨텍스트에 저장되었다가 분리된 상태
    - 영속 상태 → 준영속 상태(상태 변화)
- 영속성 컨텍스트가 엔티티 관리 x
    - 영속성 컨텍스트가 제공하는 기능 사용 x

(4) 삭제

- 영속성 컨텍스트에 있는 엔티티를 삭제
- 이때 DB에서도 삭제
    - 주의) 트랜잭션이 끝나거나 EntityManager를 flush하여 삭제 쿼리가 나가야 DB에서 삭제됨


더티 체킹을 통해 자동으로 생성된 update query는 기본적으로 모든 속성을 업데이트한다.

그 이유는,

- 모든 필드를 update query로 만들면, 수정 쿼리가 항상 동일하게 생성되므로, 쿼리 재사용 가능
- 또한, DB는 동일한 query를 전달받으면 한 번 파싱된 쿼리 재사용 가능

그러나, 필드가 많은 경우에는 모든 필드를 update하는 것이 부담이 될 수 있다.

따라서, 변경된 필드만 updatd하고 싶다면 엔티티 최상단에 `@DynamicUpdate` 어노테이션을 선언해주면 된다.

```
@Transactional
    public void updateDiary(final long id, final DiaryUpdateRequest diaryUpdateRequest) {
        // id에 해당하는 DiaryEntity가 존재하는지 확인
        DiaryEntity diaryEntity = findDiaryById(id);
        diaryEntity.setContent(diaryUpdateRequest.content());
    }
```

> dirty checking은 save를 대체하는 것이 아니라, update를 대체해주는 것이다.
영속성 컨텍스트에 포함된 엔티티들의 변경을 감지하는 것이다.
처음 만들어진 엔티티들은 아직 영속성 컨텍스트 대상에 포함되지 않는다.
>
> 1. 트랜잭션 범위 내에서 save하거나(즉, save로 값이 DB에 저장)
> 2. 트랜잭션 범위 내에서 저장된 엔티티를 조회해온 경우(이미 DB에 저장된 값을 꺼내서 변경)
>
> 에만 해당된다.