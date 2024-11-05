# 9주차 : Spring Cloud MSA

### 섹션5 : User Microservice

### User Microservice 기능

- 신규 회원 등록
- 회원 로그인
- 상세 정보 확인
- 회원 정보 수정/삭제
- 상품 주문
- 주문 내역 확인

### User Microservice의 API 엔드포인트

### 프로젝트 생성

- 필요 의존성
Spring Boot DevTools
Lombok
Spring Web
Eureka Discovery Client

- Health Check를 위한 컨트롤러 생성
```java
@RestController
@RequestMapping("/")
public class UserController {
    @GetMapping("/health_check")
    public String status() {
        return "It's working in User Service";
    }
}
```


- yml 설정
```yaml
greeting:
  message: Welcome to the Simple E-commerce.
```


- Controller에서 message 불러오기
    1. Environment (객체)

        ```java
        @RestController
        @RequestMapping("/")
        public class UserController {
            private Environment env;

            @Autowired
            public UserController(Environment env) {
                this.env = env;
            }

            @GetMapping("/health_check")
            public String status() {
                return "It's working in User Service";
            }

            @GetMapping("/welcome")
            public String welcome() {
        				return env.getProperty("greeting.message");
        				// Welcome to the Simple E-commerce.
            }

        }
        ```

    2. @Value

        ```java
        // Greeting.java
        @Component
        @Data
        public class Greeting {
            @Value("${greeting.message}") // .yml에서 직접 가져옴
            private String message;
        }
        ```

        ```java
        @RestController
        @RequestMapping("/")
        public class UserController {
            @Autowired
            private Greeting greeting;

            @GetMapping("/health_check")
            public String status() {
                return "It's working in User Service";
            }

            @GetMapping("/welcome")
            public String welcome() {
                return greeting.getMessage();
            }

        }
        ```


- H2 DB 연동
    1. h2 의존성 추가

        ```xml
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <version>1.3.176</version> <!-- DB 자동 생성 지원을 위해 구버전 사용 -->
            <scope>runtime</scope>
        </dependency>
        ```

    2. H2 yml 설정

        ```yaml
        spring:
          h2:
            console:
              enabled: true
              settings:
                web-allow-others: true
              path: /h2-console
        ```
