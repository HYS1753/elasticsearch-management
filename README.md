# Elasticsearch Management (AI Harness Workspace)

이 저장소는 관리 도구 서비스(`management_api`)와 프론트엔드 서비스(`management_ui`)를 통합하여 관리하는 **AI Agent Harness Engineering** 워크스페이스입니다.

이 프로젝트는 Antigravity, Cursor, GitHub Copilot 등 **어떤 AI 에이전트가 투입되더라도** 즉시 프로젝트의 컨텍스트, 제약 사항, 작업 흐름을 파악하고 풀스택 서비스에 대해 일관된 고품질의 코드를 생성할 수 있도록 구조화되어 있습니다.

---

## 🧭 프로젝트 구조 (Submodules & Architecture)

이 저장소는 두 개의 주요 서브모듈로 구성되어 있습니다.

* **`management_api/`**: 관리 도구용 백엔드 서비스 API (Python 기반)
* **`management_ui/`**: 실제 서비스를 동작하는 프론트엔드 UI (Next.js 기반)

## 📘 AI Harness 가이드 문서 (Single Source of Truth)

AI 에이전트(혹은 인간 개발자)가 이 저장소에서 작업할 때 참조해야 하는 핵심 가이드라인들입니다. 작업 전에 반드시 확인하세요.

* **[AGENTS.md](./AGENTS.md)**: **제약 사항 및 절대 규칙 (Constraints)**
  * 프로젝트 내에서 절대 해서는 안 될 행동과 코드 작성 규칙이 정의되어 있습니다. 코드를 수정하기 전 반드시 읽어야 합니다.

* **[ARCHITECTURE.md](./ARCHITECTURE.md)**: **시스템 설계 및 경계 (Inform)**
  * `management_api`와 `management_ui` 간의 역할 분리, 아키텍처 기준을 제공합니다.

* **[WORKFLOW.md](./WORKFLOW.md)**: **표준 작업 프로세스 (Verify & Workflow)**
  * 프론트엔드와 백엔드 간의 작업을 어떻게 기획하고 구현하며 검증해야 하는지에 대한 일련의 작업 순서와 완료 기준을 정의합니다.

* **[agent.yaml](./agent.yaml)**: **에이전트 페르소나 및 역할 정의**
  * 에이전트가 풀스택 엔지니어의 역할을 수행할 수 있도록 돕는 메타 설정 파일입니다.

---

## 🛠 어떻게 작업해야 하나요? (How to work here)

**인간 개발자(User)를 위한 가이드:**
1. AI 에이전트(예: Antigravity)에게 작업을 지시할 때, 추가적인 프롬프트를 길게 쓸 필요가 없습니다.
2. 단순히 **"UI에서 새로운 관리자 페이지를 추가하고 API와 연동해줘. 단, `WORKFLOW.md`와 `ARCHITECTURE.md`를 반드시 준수해."** 라고만 지시하세요.
3. 에이전트는 스스로 하네스(Harness) 문서들을 읽고, 규칙(`AGENTS.md`)을 준수하여 양쪽 서브모듈에 걸쳐 안전하게 코드를 생성합니다.

**AI 에이전트를 위한 가이드 (For AI Agents):**
1. **READ FIRST**: Before making any tool calls or writing code, read `AGENTS.md` and `ARCHITECTURE.md`.
2. **FOLLOW WORKFLOW**: Always adhere to the step-by-step process defined in `WORKFLOW.md`.
3. **UNDERSTAND SUBMODULES**: Remember that this workspace contains two distinct projects (`management_api` and `management_ui`). Switch contexts and directories appropriately when executing terminal commands or modifying files.
