
# API Gateway 서비스란

API Gateway는 클라이언트가 직접 마이크로서비스에 접근하는 대신, 
설정된 라우팅에 따라 요청을 적절한 엔드포인트로 전달하고 응답을 받아오는 역할을 하는 프록시. 
이를 통해 시스템 내부 구조를 외부에 숨기고, 클라이언트 요청에 맞춰 데이터를 가공해 응답할 수 있는 장점이 있다. 
각 마이크로서비스를 개별적으로 호출하는 대신 게이트웨이를 통해 통합 관리할 수 있기 때문에, 새로운 마이크로서비스가 추가되거나 
기존 서비스의 요청 경로나 파라미터가 변경되었을 때 유연하게 대응할 수 있다.

API Gateway 사용 목적:

- 인증 및 권한 관리
- 서비스 간 통합 검색
- 응답 데이터 캐싱
- 회로 차단기 및 QoS 정책 적용
- 요청 속도 제한
- 로드 밸런싱
- 로그 기록, 추적, 상관 관계 관리
- 헤더 및 쿼리 파라미터 변환
- P 허용 목록 관리

## Netflix Ribbon

Netflix Ribbon은 스프링 클라우드에서 사용하는 클라이언트 사이드 로드 밸런서로, 마이크로서비스 간의 요청을 분산시킴. 다만 비동기 처리를 지원하지 않는 제한이 있음.

## Netflix Zuul

Zuul은 API Gateway 서비스로 사용되었지만, 현재는 Spring Cloud Gateway로 대체.

## Spring Cloud Gateway

Spring Cloud Gateway는 API Gateway 역할을 수행하며, 다양한 필터를 통해 요청을 사전 또는 사후에 처리할 수 있다.

## FILTER
Predicate: 특정 조건에 따라 요청을 분기 처리.
Pre Filter: 요청을 처리하기 전에 실행되는 필터.
Post Filter: 응답을 처리한 후 실행되는 필터.
필터는 Java 코드 또는 yml 설정 파일을 통해 등록할 수 있다.

```java
@Configuration
public class FilterConfig {

    @Bean
    public RouteLocator gatewayRoutes(RouteLocatorBuilder builder) {
        return builder.routes()
                .route(r -> r.path("/first-service/**")
                        .filters(f -> f.addRequestHeader("first-request", "first-request-header")
                                      .addResponseHeader("first-response", "first-response-header"))
                        .uri("http://localhost:8081"))
                .route(r -> r.path("/second-service/**")
                        .filters(f -> f.addRequestHeader("second-request", "second-request-header")
                                      .addResponseHeader("second-response", "second-response-header"))
                        .uri("http://localhost:8082"))
                .build();
    }
}

```
```yaml
spring:
  application:
    name: apigateway-service
  cloud:
    gateway:
      routes:
        - id: first-service
          uri: http://localhost:8081/
          predicates:
            - Path=/first-service/**
          filters:
            - AddRequestHeader=first-request, first-requests-header2
            - AddResponseHeader=first-response, first-response-header2
        - id: second-service
          uri: http://localhost:8082/
          predicates:
            - Path=/second-service/**
          filters:
            - AddRequestHeader=second-request, second-requests-header2
            - AddResponseHeader=second-response, second-response-header2

```
요약
API Gateway를 사용하면 여러 마이크로서비스가 있을 때, 
클라이언트는 각 서비스의 실제 주소나 포트를 알 필요 없이 게이트웨이를 통해 이름으로만 요청을 보낼 수 있다. 
이 과정에서 필터를 통해 요청이나 응답 헤더에 필요한 값을 추가할 수 있으며, 이를 통해 AWS의 로드 밸런서와 유사한 기능을 제공한다. 
msa간의 어떤 통신방법으로 통신을 하는지 알게 되었다.




