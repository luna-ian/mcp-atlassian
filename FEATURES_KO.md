# MCP Atlassian 주요 기능 요약

아틀라시안 제품(Jira, Confluence)을 연동하기 위한 MCP 서버의 핵심 기능을 큰 범주로 정리합니다. Kotlin 변환 작업 시 참고용으로 활용할 수 있습니다.

1. **지라(Jira) 클라이언트 및 연산 모듈**
   - `src/mcp_atlassian/jira/` 위치
   - 인증/세션 설정과 에픽, 이슈, 보드, 스프린트 등 다양한 API 연동 로직을 포함합니다.
   - 프로토콜 정의와 믹스인 형태의 구현을 통해 세부 기능을 분리합니다.

2. **컨플루언스(Confluence) 클라이언트 및 연산 모듈**
   - `src/mcp_atlassian/confluence/` 위치
   - 페이지, 스페이스, 라벨, 검색 등 Confluence 관련 API와 도구를 제공합니다.
   - Jira 모듈과 마찬가지로 믹스인 방식으로 세분화되어 있습니다.

3. **모델 계층**
   - `src/mcp_atlassian/models/` 경로
   - Jira와 Confluence API 응답을 표현하는 Pydantic 모델이 정의되어 있습니다.
   - `ApiModel` 기반 클래스로 공통 변환 로직을 제공합니다.

4. **FastMCP 서버 구현**
   - `src/mcp_atlassian/servers/` 디렉터리
   - Starlette/FastAPI 기반 서버로, Jira/Confluence용 MCP 툴을 등록하여 실행합니다.
   - `main.py`가 진입점 역할을 하며, 툴 필터링과 라이프사이클을 관리합니다.

5. **텍스트 전처리 모듈**
   - `src/mcp_atlassian/preprocessing/` 하위
   - Markdown과 Jira/Confluence 마크업 간 변환 등 텍스트 처리 기능을 담당합니다.

6. **공통 유틸리티**
   - `src/mcp_atlassian/utils/` 폴더
   - 환경 변수 관리, OAuth 설정, SSL 처리, 로그 마스킹 등 재사용 가능한 기능을 모아둡니다.

7. **예외 정의 및 기본 설정**
   - `src/mcp_atlassian/exceptions.py`에서 인증 실패 등 공통 예외를 정의합니다.
   - 루트 레벨 설정 파일과 스크립트(`scripts/`)로 개발 및 배포를 돕습니다.
