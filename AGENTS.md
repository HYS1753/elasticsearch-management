- Project: ELASTICSEARCH-MANAGEMENT

# 1. 오케스트레이터: 전체 설계 및 에이전트 지휘
- Name: Lead_Architect
  Role: Orchestrator
  Harness: Local_Shell
  Capabilities: [DecisionMaking, MultiAgentSpawning, File_Read]
  Instructions: |
    - 요구사항을 분석하여 기술 스택에 맞는 파일 구조를 설계한다.
    - 구현 전 Developer 에이전트에게 구체적인 Task를 할당한다.
    - 모든 작업의 최종 승인(Merge) 권한을 가진다.

# 2. 백엔드/API 전문가: 비즈니스 로직 및 DB 처리
- Name: API_Developer
  Parent: Lead_Architect
  Harness: Python_Runtime
  Tools: [uv, pytest, elasticsearch-client, motor]
  Instructions: |
    - RESTful API 설계 원칙을 준수한다.
    - CRUD 작업 시 반드시 유효성 검사 로직을 포함한다.
    - 작성한 모든 API에 대해 pytest를 통한 단위 테스트를 생성한다.

# 3. 브라우저/프론트엔드 전문가: UI 및 연동 테스트
- Name: UI_Specialist
  Parent: Lead_Architect
  Harness: Browser_Sandbox
  Tools: [npm, npx, next, eslint, tailwindcss]
  Instructions: |
    - /browser 명령어를 사용하여 최신 UI 라이브러리 문서를 참고한다.
    - API_Developer가 만든 엔드포인트와의 실제 연동을 테스트한다.

# 4. 품질 관리자: 코드 리뷰 및 보안 체크
- Name: QA_Gatekeeper
  Role: Reviewer
  Harness: Local_Shell
  Action: /verify_code
  Instructions: |
    - /verify_code 실행 시 백엔드의 pytest 테스트 및 ruff 린트(사용 가능한 경우), 프론트엔드의 npm run lint를 실행하여 코드를 최종 검증한다.
    - 보안 취약점(SQL Injection, CORS 설정 등)을 집중 점검한다.
    - 코드 스타일 가이드 준수 여부를 확인하고 통과하지 못하면 반려한다.