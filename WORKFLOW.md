# Standard Operating Workflow (WORKFLOW.md)

This file defines the expected process for managing templates in the `elasticsearch-management` project. Agents must adhere to this flow when creating or modifying configurations.

## 1. Template Creation Workflow
When tasked with creating a new index configuration:
1. **Analyze Requirements:** Understand the fields, data types, and index patterns needed.
2. **Identify Components:** Check if existing `component_templates` can be reused. If a new reusable block is needed, create it in `/component_templates/`.
3. **Assemble Index Template:** Create the final template in `/index_templates/`, composing the necessary component templates.
4. **Document:** Write a beginner-friendly explanation and API payload example in the README.

## 2. Definition of "Done"
A task is considered "Done" when:
- [ ] The JSON files are syntactically valid.
- [ ] The templates adhere to the DRY principles outlined in `ARCHITECTURE.md`.
- [ ] The relevant `README.md` documentation has been updated.
- [ ] A walkthrough artifact or direct summary is provided to the user with the expected API response or `curl` commands.

## 3. Review Process
- Before final approval, present the JSON structure to the user using markdown `diff` or full code blocks.
- Explicitly call out any potentially breaking changes (e.g., changing a field from `text` to `keyword`).
