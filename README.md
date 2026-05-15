# Elasticsearch Management (AI Harness Workspace)

이 저장소는 Elasticsearch 클러스터의 컴포넌트 템플릿(Component Templates)과 인덱스 템플릿(Index Templates)을 관리하기 위한 프로젝트입니다. 

특히 이 프로젝트는 **AI Agent Harness Engineering** 방법론을 적용하여 설계되었습니다. Antigravity를 비롯하여 Cursor, GitHub Copilot 등 **어떤 AI 에이전트가 이 프로젝트에 투입되더라도** 즉시 프로젝트의 컨텍스트, 제약 사항, 작업 흐름을 파악하고 일관된 고품질의 코드를 생성할 수 있도록 디렉터리와 가이드 문서가 구조화되어 있습니다.

---

## 🧭 AI Harness 구조 안내 (Directory & File Structure)

AI 에이전트(혹은 인간 개발자)가 이 저장소에서 작업할 때 참조해야 하는 "진실의 공급원(Single Source of Truth)" 문서들과 숨김 지식 저장소(`.ai/`)의 역할을 설명합니다.

### 1. Root-Level Guidelines (핵심 가이드라인)
작업을 시작하기 전, 또는 새로운 아키텍처를 설계할 때 반드시 확인해야 하는 최상위 문서들입니다.

* **[AGENTS.md](./AGENTS.md)**: **제약 사항 및 절대 규칙 (Constraints)**
  * **무엇을 할 때 보나요?**: 코드를 수정하거나 새로운 기능을 제안하기 전에.
  * **어떤 내용인가요?**: 프로젝트 내에서 절대 해서는 안 될 행동(예: 버전 호환성 체크 없이 매핑 변경, 기존 필드 삭제 금지)과 에러 발생 시의 대처 방법(Error Recovery Loop)이 명시되어 있습니다.
  
* **[ARCHITECTURE.md](./ARCHITECTURE.md)**: **시스템 설계 및 경계 (Inform)**
  * **무엇을 할 때 보나요?**: 새로운 템플릿 구조를 설계하거나 기존 템플릿을 리팩토링할 때.
  * **어떤 내용인가요?**: 중복을 피하기 위한 DRY 원칙, 컴포넌트 템플릿과 인덱스 템플릿의 디렉터리 분리 철학 등 전반적인 아키텍처 기준을 제공합니다.

* **[WORKFLOW.md](./WORKFLOW.md)**: **표준 작업 프로세스 (Verify & Workflow)**
  * **무엇을 할 때 보나요?**: 새로운 작업을 시작하고 끝마칠 때.
  * **어떤 내용인가요?**: 템플릿을 생성하고, 검증하고, 문서화(`README.md`)하는 일련의 순서와 작업 완료(Done)의 기준을 정의합니다.

* **[agent.yaml](./agent.yaml)**: **에이전트 페르소나 및 역할 정의**
  * **무엇을 할 때 보나요?**: AI 에이전트의 역할과 권한을 파악하거나 수정할 때.
  * **어떤 내용인가요?**: 에이전트가 `LeadTemplateEngineer` 또는 `DocumentationSpecialist`의 역할을 수행하도록 돕는 메타 설정 파일입니다.

---

### 2. `.agent/` (AI Agent 전용 지식 및 스킬 저장소)
과거 `.antigravity/` 등의 특정 툴에 종속되었던 구조를 벗어나, 모든 범용 AI 에이전트가 참조할 수 있도록 만들어진 디렉터리입니다.

* **[.agent/map.md](./.agent/map.md)**: 프로젝트 라우팅 인덱스. 에이전트가 방대한 코드베이스를 탐색하기 전, 어떤 디렉터리에 어떤 파일이 있는지 빠르게 파악하는 지도 역할을 합니다.
* **`.agent/knowledge/`**: 모듈별 컨텍스트 저장소. 
  * 작업 컨텍스트가 길어지거나 복잡한 도메인 지식(예: `logging_knowledge.md`)이 필요할 때, 에이전트가 장기 기억(Long-term memory)을 이곳에 Markdown 형태로 저장하고 읽습니다.
* **`.agent/skills/`**: 워크스페이스 커스텀 스킬.
  * 예: `elasticsearch_guidelines.md`에는 JSON 페이로드 작성법, `text` vs `keyword` 매핑 사용 규칙 등 Elasticsearch 관련 실무 지침(Skill)이 담겨 있습니다.

---

## 🛠 어떻게 작업해야 하나요? (How to work here)

**인간 개발자(User)를 위한 가이드:**
1. AI 에이전트(예: Antigravity)에게 작업을 지시할 때, 추가적인 프롬프트를 길게 쓸 필요가 없습니다.
2. 단순히 **"새로운 Nginx 로그용 인덱스 템플릿을 만들어줘. 단, `WORKFLOW.md`와 `ARCHITECTURE.md`를 반드시 준수해."** 라고만 지시하세요.
3. 에이전트는 스스로 하네스(Harness) 문서들을 읽고, 금지 사항(`AGENTS.md`)을 피해 안전하게 코드를 생성합니다.

**AI 에이전트를 위한 가이드 (For AI Agents):**
1. **READ FIRST**: Before making any tool calls or writing code, read `AGENTS.md` and `ARCHITECTURE.md`.
2. **FOLLOW WORKFLOW**: Always adhere to the step-by-step process defined in `WORKFLOW.md`.
3. **USE SKILLS**: When generating Elasticsearch mapping JSON, consult `.agent/skills/elasticsearch_guidelines.md` for best practices.
