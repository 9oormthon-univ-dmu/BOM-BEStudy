# MicroService란
클라우드 네이티브 아키텍처, 클라우드 네이티브 어플리케이션의 핵심   
전체 서비스를 구축하고 있는 개별적인 모듈이나 기능을 독립적으로 개발하고, 배포하고 운영할 수 있도록 세분화된 서비스   

## Cloud Native Architecture
- 확장 가능한 아키텍처
    - 시스템의 수평적 확장에 유연
    - 확장된 서버로 시스템의 부하 분산, 가용성 보장
    - 시스템 또는 서비스 애플리케이션 단위의 패키지 (컨테이너 기반 패키지)
    - 모니터링

Scale-up 시스템의 성능 스펙을 높이는 작업   
Scale-out 서버 인스턴스 양을 늘려 분산

- 탄력적 아키텍처
    - 서비스 생성 - 통합 - 배포, 비즈니스 환경 변화에 대응 시간 단축
    - 분활된 서비스 구조
    - 무상태 통신 프로토콜
    - 서비스의 추가와 삭제 자동감지
    - 변경된 서비스 요청에 따라 사용자 요청 처리 (동적 처리)

- 장애 격리 (Fault isolation)
    - 특정 서비스에 오류가 발생해도 다른 서비스에 영향을 주지 않음

## Cloud Native Application

1. MicroService로 개발
2. CI/CD
3. DevOps
4. Contaniner 가상화

### CI/CD 
- 지속적인 통합 CI (Continuous Integration)
    - 통합서버, 소스관리, 빌드 도구, 테스트 도구
    - ex) Jenkins, Team CI, Travis CI

- 지속적 배포
    - Continuous Delivery   
        소스 저장소 -> 실행 환경 | 수동 배포
    - Continuous Deployment
        소스 저장소 -> 실행 환경 | 자동 배포
    - Pipe line

- 카나리 배포와 블루그린 배포
    - 카나리 배포   
    특정 유저에게 신버전을 배포하여 테스트 후 전체를 배포

    - 블루그린 배포   
    신버전을 배포하고 사용자를 일제히 전환

### DevOps
Development + Operations 개발 조직과 운영 조직의 통합을 의미  
이러한 통합으로 고객의 요구사항을 빠르게 반영하고 만족도 높은 결과물을 제시하는것에 목적을 두고 있다.

### Container 가상화
기존 로컬 환경에서 운영하고 유지하던 시스템을 클라우드 환경으로 이전하여 적은 비용으로 탄력성있는 시스템을 구축   

기존 하드웨어, 서버 가상화에 비하여 적은 리소스를 사용하여 가상화 서비스 구축가능

## 12 Factors
Heroku라는 플랫폼에서 제안한 클라우드 어플리케이션을 개발하거나 서비스를 운영할 때 고려해야할 항목들

1. Base Code
2. Dependency Isolation
3. Configurations 
4. Linkable Backing Services
5. Stages Of Creation
6. Stateless Processes
7. Port Binding
8. Concurrency
9. Disposability
10. Development & Production Parity
11. Logs
12. Admin Processes For Eventual

### 12 Factors + 3
Heroku가 제시한 12 Factors에 3가지 항목을 더해 피보탈이라는 클라우드 회사가 발표

1. API Frist
2. Telemetry
3. Authentication and Authorization
