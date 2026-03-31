# AGENTS.md

## Project
Spring Boot backend service.

## Structure
- context/ → project knowledge (minimal, non-structural)
- skills/ → reusable engineering workflows
- agents/ → specialized roles
- tasks/ → repeatable tasks
- commands/ → quick actions
- mcp/ → code intelligence

## Routing

| Task | Load |
|------|------|
| implement API | skills/rest + skills/code-navigation |
| database work | skills/jpa + skills/code-navigation |
| kafka | skills/kafka + skills/code-navigation |
| jooq | skills/jooq-repository + skills/code-navigation |
| bug fixing | tasks/fix-bug.md + skills/code-navigation |

## Code Intelligence (MANDATORY)

When working with code:

ALWAYS use MCP tools:

- find symbols
- find usages
- trace call graph
- analyze dependencies

DO NOT:

- guess structure
- scan random files blindly
- rely only on text search

Preferred workflow:

1. Find symbol via MCP
2. Analyze relationships
3. Load minimal required files
4. Apply changes

## External Knowledge (Context7 MCP)

Use Context7 when working with:

- libraries (Spring, JOOQ, Kafka, etc.)
- frameworks
- APIs
- configuration
- annotations and DSLs

Purpose:
- get up-to-date documentation
- avoid hallucinated APIs
- ensure correct usage

## When to use Context7

ALWAYS use Context7 if:

- you are unsure about API usage
- working with non-trivial library features
- writing configuration
- using annotations or DSL (JOOQ, Spring)

## When NOT to use Context7

- pure business logic
- project-specific code
- simple Java constructs

## Combined Workflow (CRITICAL)

When solving tasks:

1. Use CodeGraph → understand project structure
2. Use Context7 → understand external APIs
3. Combine both → implement solution

DO NOT:
- guess library APIs
- rely on memory for frameworks

## Rules

- Never guess architecture
- Prefer reuse over new implementations
- Follow existing patterns strictly
