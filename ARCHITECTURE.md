# Architecture & System Design (ARCHITECTURE.md)

This document outlines the architectural boundaries and design standards for the `elasticsearch-management` repository. Agents must use this to understand project context.

## 1. System Overview
This repository manages the configuration state of an Elasticsearch cluster. It primarily consists of:
- **Component Templates:** Reusable blocks of mappings, settings, or aliases.
- **Index Templates:** Definitions that apply component templates to specific index patterns.

## 2. Structural Standards
### Directory Layout
- `/component_templates/`: Contains reusable building blocks (e.g., standard analyzers, common metadata fields).
- `/index_templates/`: Contains the actual templates that map to `index_patterns`. These should compose `component_templates` rather than repeating mappings inline.

### JSON Formatting
- All templates MUST be valid JSON.
- Indentation should be 2 spaces.
- Keep mapping definitions modular.

## 3. Design Philosophy
- **DRY (Don't Repeat Yourself):** If multiple index templates share the same mappings (e.g., `timestamp`, `trace_id`), those mappings must be extracted into a `component_template`.
- **Dynamic Mapping Strictness:** Default to `dynamic: "strict"` or `"false"` to prevent mapping explosions, unless otherwise specified by the user.
