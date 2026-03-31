---
name: jooq-repository
description: Build JOOQ repositories using Repository Chain Pattern with JoinModule reuse and no query duplication
---

# JOOQ Repository Skill

## Concept: Repository Chain Pattern

Each JOOQ repository manages:
1. Its own main entity
2. JoinModule for parent reuse

Chain:
JooqUserRepository
↑
JooqServiceInfoRepository
↑
JooqPendingRepository
↑
JooqRecordRepository

## Rules

- NEVER duplicate joins
- ALWAYS reuse JoinModule
- ALWAYS compose queries via chain

## Mandatory workflow

1. Use MCP to find existing repositories
2. Locate JoinModules
3. Reuse instead of reimplement

## Anti-Patterns

- Copy-paste queries
- Manual joins when module exists
- Breaking chain
