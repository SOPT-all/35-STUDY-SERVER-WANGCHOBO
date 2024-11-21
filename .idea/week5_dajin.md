# 🤣합동세미나🤣

### 합동세미나를 진행하며 어려웠던 점

1. **설계를 잘하자**
    - 시간이 부족할 것 같다는 이유로 설계를 꼼꼼히 하지 못하고 넘어가면서, 이후 구현 과정에서 많은 문제가 발생했습니다. 설계에 더욱 집중했다면 더 단기간에 끝낼 수 있었을 것이라는 아쉬움이 남습니다.
2. **소통**
    - 합동세미나에서 생각보다 소통해야 할 부분이 많음을 느꼈습니다. 특히 디자이너 선생님들과는 처음 협업해 보며, 어떻게 물어보아야 실례가 되지 않을지 고민이 많았습니다.
    - 클라이언트분들과의 소통에서는 조율해야 할 점이 상당히 많음을 느꼈고, 실제 클라이언트 개발자가 원하는 API에 대해 많은 이야기를 들을 수 있어서 유익했습니다.
3. 개발
    - record 처음 해봐서 조금 헷갈렸습니다. 그래도 사용해보니 편리한 느낌은 받았습니다.
        - record : Java 컴파일러가 자동으로 생성자, toString(), equals(), hashCode() 메서드를 만들어주므로 별도로 작성할 필요 없이 데이터 클래스의 일반적인 동작을 제공한다.
        - 불변성 : record의 모든 필드는 암시적으로 fianl로 선언된다. 필드 값을 변경할 수 없고, 객체를 불변하게 유지하는데 적합하다.
        - 간결성 : 데이터를 저장하는 용도의 클래스를 작성할 때 필요한 코드가 크게 줄어든다.
        - Vaildation과의 통합 : @Validated 또는 javax.validation과 함께 사용 가능

        ```java
        public record UserRequest(
            @NotBlank String username,
            @Email String email
        ) {
        }
        
        @PostMapping("/users")
        public ResponseEntity<String> createUser(@Valid @RequestBody UserRequest userRequest) {
            return ResponseEntity.ok("User created");
        }
        ```

        - 주의 점
            - 변경 가능한 데이터를 처리해야 할 경우 적합하지 않다.

              → 클래스를 사용하거나 @Build 패턴을 도입해야 한다.

            - JPA는 엔티티의 변경 가능성을 전제로 동작하므로 record는 엔티티로 사용하지 않는다. (DTO)
            - record는 단순한 데이터 구조를 위한 것이며 복잡한 비지니스 로직이 포함된 클래스에는 적합하지 않다.