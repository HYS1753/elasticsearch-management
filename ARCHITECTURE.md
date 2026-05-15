# Architecture & System Design (Root)

This workspace is a **unified harness** containing two distinct submodules that work together to form the Elasticsearch Management service.

This document serves as a high-level router. To understand the specific constraints, directory structures, and technologies used within a submodule, you **MUST** read the `ARCHITECTURE.md` file located inside that respective submodule.

## 1. System Submodules

### [Backend: `management_api`](./management_api/ARCHITECTURE.md)
The Python-based API service handling all business logic, data persistence, and Elasticsearch interactions.
👉 **[Read the API Architecture Constraints](./management_api/ARCHITECTURE.md)**

### [Frontend: `management_ui`](./management_ui/ARCHITECTURE.md)
The Next.js-based user interface for operators to interact with the management services.
👉 **[Read the UI Architecture Constraints](./management_ui/ARCHITECTURE.md)**

## 2. Macro Architecture Boundaries

At the root level, agents and developers must respect the following strict boundaries:

- **Strict Separation of Concerns:**
  - The UI (`management_ui`) is strictly for presentation and user interaction. It must NEVER contain direct database connections or heavy data processing logic.
  - The API (`management_api`) is the sole owner of business logic and data state.

- **Independent Execution Contexts:**
  - The two submodules manage their own dependencies independently.
  - **Never** attempt to run `npm` or `uv` commands at the root `/elasticsearch-management` level.
  - Always `cd` into the appropriate submodule before executing linting, testing, or server start commands.

- **Type & Schema Synchronization:**
  - The API defines the canonical data contracts (via Pydantic). 
  - If an API response schema changes, the corresponding TypeScript interfaces/types in `management_ui/types/` MUST be updated to reflect those changes.
