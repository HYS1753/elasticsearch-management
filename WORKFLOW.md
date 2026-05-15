# Standard Operating Workflow (WORKFLOW.md)

This file defines the expected process for implementing features or making changes across the `elasticsearch-management` submodules. Agents must adhere to this flow when modifying the codebase.

## 1. Full-Stack Feature Workflow
When tasked with creating or modifying a feature spanning both UI and API:

1. **Analyze & Plan:** 
   - Understand the user requirements.
   - Design the API schema/endpoint before touching the UI.
   - Plan the UI component structure and data flow.
   
2. **Implement Backend (`management_api`):** 
   - Navigate to the `management_api` directory (`cd management_api`).
   - Implement the necessary routes, services, schemas, and data access logic.
   - Write tests and ensure they pass.

3. **Implement Frontend (`management_ui`):** 
   - Navigate to the `management_ui` directory (`cd management_ui`).
   - Update API clients and types to match the newly implemented backend endpoints.
   - Build or update the React components and Next.js pages.
   
4. **Integration & Verification:** 
   - Verify that the UI correctly communicates with the API.
   - Ensure there are no cross-origin resource sharing (CORS) or type mismatch errors.

## 2. Definition of "Done"
A task is considered "Done" when:
- [ ] Both `management_api` and `management_ui` codebases compile/run without syntax or type errors.
- [ ] Appropriate unit/integration tests have been written and passed for the backend.
- [ ] The architectural boundaries in `ARCHITECTURE.md` have been strictly respected (e.g., no business logic leaked into UI).
- [ ] A walkthrough artifact or direct summary is provided to the user detailing the changes made in both submodules, including screenshots or API responses if applicable.

## 3. Review Process
- Present complex API design changes or major UI layout changes to the user for approval before deep implementation.
- Explicitly call out any breaking changes to existing APIs that might affect the UI or other consumers.
