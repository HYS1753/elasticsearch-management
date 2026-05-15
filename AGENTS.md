# AGENT.md

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

Target Root: src/python/retrieval_model/

Strictly adhere to the following layer boundaries:
•	Schema: DTOs go to application/schema/ (sub-folders: requests/, responses/).
•	Service: Business logic & model inference go to application/service/.
•	Endpoint: FastAPI routes go to application/endpoint/.
•	Infrastructure: Embedding & DB logic go to application/repository/embedding/.
•	Config: App settings in config/fastapis/, model paths in config/models/.

The Rule: If a change spans layers, implement in order: Schema → Service → Endpoint.

## 6. Execution Protocol

No "blind" coding. Define jobs and track progress explicitly.
1.	Verify Path: Confirm you are working within src/python/retrieval_model/.
2.	Job Definition & Progress: Before implementing, define the required "Jobs" and report status using: [WAIT], [RUN], [DONE].
    - Format: ### Planned Jobs followed by a numbered list of tasks.
3.	Standardized Plan: For each Job, explicitly state:
    - [Target File] → [Action] → [Verification Method]
4.	Model & Config Handling:
    - Never hardcode local model paths. Use config/models/.
    - Use custom exceptions from config/exceptions/.
    - Use centralized logging via config/loggings/.
5.	Validation: Run pytest or ruff if available. Update config/settings/ if new env vars are added.

Success criteria: The model acts as a surgical architect, placing every piece of code in its pre-defined home without manual redirection.

## 7. Testing Strategy

Follow a tiered approach to ensure reliability without bloating test execution time.

### 7.1. Unit Tests (Isolated)
- **Repository/Adapter:** Mock external clients (e.g., DB drivers, API clients). Test only the wrapper logic and exception handling.
- **Service:** Mock all Repositories. Test business logic, data transformation, and edge cases.
- **Location:** `tests/unit/[layer_name]/`

### 7.2. Integration Tests (Connected)
- **Scope:** Test `Endpoint` to `Repository` flow.
- **Policy:** Use `TestClient`. Replace heavy ML models with static vector mocks using `pytest-mock`.
- **Location:** `tests/integration/`

### 7.3. Test Implementation Protocol
1. **Fixtures First:** Define reusable Pydantic schemas and mock objects in `conftest.py`.
2. **Surgical Assertions:** Check specific response fields and custom exceptions from `config/exceptions/`.
3. **No Side Effects:** Ensure tests do not persist data to production `models/` or `logs/`.

---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.