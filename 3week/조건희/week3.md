# 3주차
## Spring Cloud MicroServices
***
### Software Architecture
The History of IT System
- 1960 ~ 1980 : Fraglie, Cowboys
- Mainfarme, H/W 가 중심이 되던 시기
- 1990 ~ 2000 : Robust, Distributed
- Changes (분산)
- 2010 ~ : Resilient/Anti-Fragile, Cloud Native
- Flow of Value의 지속적인 개선
   
### Antifragile의 4가지 특징
- Auto scaling
- Microservices
- Chaos engineering
- 변동, 예견된 불확실성, 예견되지 않는 불확실성, 카오스 불확실성에 대응.
- Continuos deployments
- CI/CD 지속적 통합, 배포.
***   

### Cloud Native Architecture
- 확장 가능한 아키텍처
- 시스템의 수평적 확정에 유연
- 확장된 서버로 시스템의 부하 분산, 가용성 보장.
- 시스템 또는 서비스 애플리케이션 단위의 패키지 (컨테이너 기반 패키지)
- 모니터링
- 탄력적 아키텍처
- 서비스 생성 - 통합 - 배포, 비즈니스 환경 변화에 대응 시간 단축
- 분활된 서비스 구조
- 무상태 통신 프로토콜
- 서비스의 추가와 삭제 자동으로 감지
- 변경된 서비스 요청에 따라 사용자 요청 처리
- 장애 격리 (Fault isolation)
- 특정 서비스에 오류가 발생해도 다른 서비스에 영향을 주지 않음.
***
  
### Cloud Native Application
- Cloud Native Architecture에 의해 설계되고 구현되는 어플리케이션.
- Cloud Native Application의 구현 형태

- MicroService로 구현
- CI/CD
- DevOps
- Containers

- 특징
- CI/CD
    - 지속적 통합, CI (Continuous Integration)
    - 통합 서버, 소스 관리(SCM), 빌드 도구, 테스트 도구
- 지속적 배포
    - Continuous Delivery
        - 패키지화 된 결과물을 실행 환경에 수동 반영
    - Continuous Deployment
        - 패키지화 된 결과물을 실행 환경에 자동 반영
- Pipe line
    - 카나리 배포와 블루그린 배포
    - 95% 사용자 -> 이전 버전 서비스
    - 5% 사용자 -> 새 버전 서비스를 이용하게 함 혹은
    - 이전 버전 사용자의 트래픽을 이전 버전과 거의 동일안 새 버전으로 점진적으로 이동.
    
- DevOps
    - Development와 Operation의 통합을 뜻함.
    - 전체 개발 일정이 완료될 때 까지 지속적으로 고객의 요구사항 반영 및 테스트.
- Container 가상화
    - Cloud Native Architecture의 핵심
    - local -> cloud로 이전하여 적은 비용으로 탄력적 시스템 구성.
    - 기존의 H/W 가상화 혹은 서버 가상화에 비해 적은 리소스를 사용.
***
### 12 Factors
- Cloud Native Application을 개발하거나 운영할 때 고려해야 할 항목. 
    
1. Code Base
    - 버전 제어 목적. 형상 관리.
2. Dependency Isolation(종속성)
    - 각 마이크로 서비스는 종속성을 가지고 패키징 됨. 전체 시스템에 영향을 주지 않는 상태에서 변경 및 수정을 할 수 있어야 함.
3. Configurations (구성 정보)
    - 시스템 외부 구성관리 도구에서 작업들을 제어.
4. Linkable Backing Services (서비스 지원)
    - 보조 서비스를 추가로 지원
5. Stages Of Creation
    - 빌드와 릴리즈, 실행 환경을 각각 분리하는 것
    - 고유한 아이디로 코드를 가지고, 이전 상태로 롤백이 가능해야 함.
6. Stateless
    - 각각의 서비스는 실행 중 다른 서비스와 분리되고 독릭적이여야 함.
7. Port Binding
    - 자체 포트에서 노출되는 인터페이스 및 기능이 자체에 포함되어 있어야 함.
8. Concurrency(동시성)
    - 많은 수의 서비스를 동일한 프로세스로 복사되어 확장.
9. Disposability
    - 서비스 인스턴스 자체가 삭제가 가능하고, 정상적 종료가 가능해야 함.
10. Development & Production Parity
    - 개발 단계와 프로덕션 단계를 구분할 수 있어야 함.
11. Logs
    - 어플리케이션이 실행되지 않는 상태여도 로그는 작동해야 함.
12. Admin Processes For Eventual Processes
    

최근에는 12 Factors에 3가지를 더 더해서 발표를 함.    

- Api First
- Telemetry
- Authentication and Authorization

