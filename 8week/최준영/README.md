# Spring Cloud MSA

## Gateway와 Eureka 연동

Gateway로 요청을 보내게 되면 Eureka에서 서비스 위치를 찾아 포워딩하는 방식

### Gateway 설정 변경
```yml
spring:
  cloud:
    gateway:
      routes:
        - id: first-service
          uri: lb://MY-FIRST-SERVICE
        - id: second-service
          uri: lb://MY-SECOND-SERVICE
```
기존 ip와 포트번호로 routes를 설정한것을 eureka에 등록된 이름을 참조하여   
"lb://(Discovery 등록된 이름)"으로 변경한다.

### 마이크로 서비스 설정 변경
```yml
eureka:
  client:
    register-with-eureka: true
    fetch-registry: true
```
Eureka에 등록될수있게 설정을 바꿔준다.

## Random Port 사용하기

### 서비스 설정 변경
```yml
server:
  port: 0
```
server의 포트를 0으로 지정하게되면 랜덤한 포트로 실행된다.

### Eureka 서버의 설정을 추가한다.
```yml
eureka:
  instance:
    instance-id: ${spring.application.name}:${spring.application.instance_id:${random.value}}
```

마이크로 서비스가 여러개 등록될때 고유한 식별값을 가지게하여 충돌을 방지한다.
