# MCP Atlassian Kotlin 마이그레이션 계획

이 문서는 기존 Python 기반 MCP Atlassian 프로젝트를 Kotlin으로 이전하기 위한 큰 틀의 단계를 정리합니다. Kotlin 변환 과정에서 참고용으로 활용하세요.

1. **프로젝트 준비 및 환경 구성**
   - Gradle 기반 Kotlin/JVM 프로젝트를 생성합니다.
   - 주요 의존성은 Ktor(또는 Spring Boot), kotlinx-coroutines, Jackson 등을 고려합니다.
   - 커맨드 라인 스크립트와 설정 파일을 Gradle에 맞게 수정합니다.

2. **패키지 구조 설계**
   - `com.example.project` 기준의 패키지 체계를 도입합니다.
   - `domain` 하위에 context별 `data`, `repository`, `instrument`, `service` 패키지를 배치합니다.
   - 복잡한 비즈니스 규칙을 처리하기 위해 4-Layer 구조를 기본으로 적용합니다.

3. **모델 계층 변환**
   - `src/mcp_atlassian/models/`의 Pydantic 모델을 Kotlin `data class`로 변환합니다.
   - 공통 필드와 변환 로직은 확장 함수와 매퍼 클래스로 분리합니다.

4. **Jira·Confluence 클라이언트 구현**
   - Python의 `jira/`, `confluence/` 모듈 기능을 각각 Ktor HttpClient 기반 클래스로 옮깁니다.
   - 인증 처리, API 호출, 오류 매핑을 Kotlin에서 재구현합니다.
   - 기존 믹스인 구조는 인터페이스와 구성 객체로 표현합니다.

5. **서버 계층 전환**
   - FastAPI 서버를 Ktor 혹은 Spring Boot 애플리케이션으로 교체합니다.
   - 컨트롤러(라우트), 서비스, 인스트루먼트, 리포지터리 레이어를 순차적으로 구현합니다.
   - 기존 MCP 툴 등록 로직을 Kotlin에서 플러그인 형태로 제공합니다.

6. **테스트 이전**
   - Pytest 기반 테스트를 JUnit5와 MockK로 재작성합니다.
   - Integration 테스트는 Testcontainers를 활용해 Jira/Confluence 모의 서버를 사용합니다.

7. **점진적 마이그레이션 전략**
   - 우선 공통 유틸리티와 모델부터 Kotlin으로 포팅해 Python 코드와 병행 구동합니다.
   - 이후 클라이언트 모듈 → 서버 모듈 순으로 단계별 이전을 진행합니다.
   - 기능 단위 전환 후 기존 테스트를 Kotlin 테스트로 교체하며 안정성을 확보합니다.

