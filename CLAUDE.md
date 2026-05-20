# CLAUDE.md

Behavioral guidelines to reduce common LLM coding mistakes. Merge with project-specific instructions as needed.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

## 5. Directory-Specific Actions

### 5.1. Backend API (`management_api`)
Target Root: `management_api/src/python/elasticsearch/`

Strictly adhere to the following layered architecture boundaries:
- **Endpoints:** FastAPI routes go to `application/endpoints/`.
- **Services:** Business logic goes to `application/services/`.
- **Schemas:** Pydantic DTOs for request/response validation go to `application/schemas/`.
- **Repository:** Data access (Elasticsearch/DB clients) goes to `application/repository/`.
- **Config:** Environment settings and configurations go to `config/`.
- **Common:** Shared constants, exceptions, and helpers go to `common/`.

The Rule: If a change spans layers, implement in order: Schema → Repository → Service → Endpoint.

### 5.2. Frontend UI (`management_ui`)
Target Root: `management_ui/`

Strictly adhere to the Next.js 15 App Router directory structure:
- **Routes / Views:** App pages and layouts go to `app/`.
- **Components:** Shared UI components go to `components/ui/`, feature-specific components to `components/[feature]/`.
- **API Clients & Helpers:** API/fetch wrappers go to `lib/`.
- **Hooks:** Custom React hooks go to `hooks/`.
- **Types:** TypeScript contracts go to `types/` (must be synchronized with backend Pydantic schemas).

---

## 6. Execution Protocol

No "blind" coding. Define jobs and track progress explicitly.
1. **Verify Path:** Confirm whether the target submodule is `management_api` or `management_ui`, and always run terminal commands within the respective subdirectory.
2. **Job Definition & Progress:** Before implementing, define the required "Jobs" and report status using: `[WAIT]`, `[RUN]`, `[DONE]`.
   - Format: `### Planned Jobs` followed by a numbered list of tasks.
3. **Standardized Plan:** For each Job, explicitly state:
   - `[Target File] → [Action] → [Verification Method]`
4. **Configuration & Environment Handling:**
   - Never hardcode credentials. Use environment variables managed via Pydantic Settings in `management_api/src/python/elasticsearch/config/`.
   - Use custom exceptions from `management_api/src/python/elasticsearch/common/`.
5. **Validation:**
   - Backend: run `uv run pytest` inside `management_api/`.
   - Frontend: run `npm run lint` inside `management_ui/`.

---

## 7. Testing Strategy (Backend)

Follow a tiered approach to ensure reliability without bloating test execution time.

### 7.1. Unit Tests (Isolated)
- **Repository/Adapter:** Mock external clients (e.g., Elasticsearch, Motor/MongoDB). Test only wrapper logic and exception handling.
- **Service:** Mock all Repositories. Test business logic, data transformation, and edge cases.
- **Location:** `management_api/test/python/elasticsearch/application/` (within `repository/` and `services/`).

### 7.2. Test Implementation Protocol
1. **Fixtures First:** Define reusable Pydantic schemas and mock objects in `conftest.py` where appropriate.
2. **Surgical Assertions:** Check specific response fields and custom exceptions from `common/`.
3. **No Side Effects:** Ensure tests do not persist data to production indices or write persistent local databases.

---

## 8. Programming Languages & Coding Constraints

- **TypeScript Standard (Frontend):**
  - Inside the `management_ui/` submodule, **ALL** newly created files, helper scripts, components, configurations, page files, or utility tools MUST be implemented in TypeScript (`.ts`, `.tsx`).
  - The use of JavaScript (`.js`, `.jsx`, `.mjs`) for new code is strictly prohibited.
  - Any remaining helper scripts or legacy JavaScript configuration scripts must be migrated to TypeScript when they are modified.

---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.