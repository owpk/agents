---
name: jdt-bridge
description: >-
  Provides deep Java code understanding using Eclipse JDT. Use this skill for
  Java code navigation, finding references, viewing type hierarchies, running
  tests, and checking compilation errors in Java projects. JDT Bridge exposes
  Eclipse's semantic index to give AI agents the same code understanding as a
  human developer in the IDE.

  Always use for: "who calls this method", "find all implementations of interface",
  "go to definition" for library classes, checking compilation errors, running
  Java tests, understanding class structure without reading source.
---

# JDT Bridge

Eclipse JDT Bridge exposes Eclipse's deep semantic index of Java code via the `jdt` CLI. This gives AI coding agents the same understanding of Java codebases that a human developer gets in the IDE.

## Installation

```bash
npm install -g @kaluchi/jdtbridge
```

Or run directly without installing:

```bash
npx @kaluchi/jdtbridge <command>
```

## Core Commands

### Find References

```bash
jdt references "com.example.dao.OrderRepository#save(Order)"
# or short form
jdt refs "com.example.dao.OrderRepository#save(Order)"
```

Returns only actual call sites (unlike grep which returns 200+ hits including comments and string literals). Shows file and line numbers.

### View Source Definition

```bash
jdt source org.springframework.transaction.support.TransactionTemplate#execute
# or short form
jdt src org.springframework.transaction.support.TransactionTemplate#execute
```

Read Spring/Hibernate/JDK source code by name. Without JDT this is impossible — library sources live inside JARs.

### Check Compilation Errors

```bash
jdt errors --project my-app-server
# or short form
jdt err --project my-app-server
```

Sub-second response compared to Maven's 30-90 seconds. Uses Eclipse's incremental compiler.

### Find Implementations

```bash
jdt implementors "com.example.core.Repository#save(Order)"
# or short form
jdt impl "com.example.core.Repository#save(Order)"
```

Returns only actual implementations by resolving type hierarchy. Unlike grep which returns every class with that method name.

### Type Hierarchy / Overview

```bash
jdt type-info com.example.web.OrderController
# or short form
jdt ti com.example.web.OrderController
```

Structured overview of a class: fields, method signatures, supertypes, line numbers — without reading raw source.

### Run Tests

```bash
jdt test run com.example.service.OrderServiceTest -f
```

Run tests with real-time progress streaming. Failures shown immediately.

### Check Status

```bash
jdt status
```

Shows git branches, open editors, errors, running launches, test results for all projects.

## Additional Commands

### Find Types

```bash
jdt find "Order*" --source-only
jdt find "com.example.dao"  # find by package
```

Find types by name, wildcard, or package.

### Subtypes

```bash
jdt subtypes "com.example.core.Repository"
```

All subtypes/implementors of a type.

### Full Hierarchy

```bash
jdt hierarchy "com.example.web.OrderController"
```

Full hierarchy: supertypes + interfaces + subtypes.

### Build Project

```bash
jdt build --project my-app-server
jdt build --project my-app-server --clean  # clean build
```

Incremental or clean build (sub-second vs Maven's 30-90 seconds).

### Test Status

```bash
jdt test status <session> -f --all
jdt test sessions
```

Show test progress/results or list test sessions.

### Refactoring

```bash
jdt organize-imports <file>
jdt format <file>
jdt rename "com.example.OldName" "NewName"
jdt move "com.example.OldClass" "target.package"
```

### Launch Applications

```bash
jdt launch list
jdt launch configs
jdt launch run <config> -f
jdt launch debug <config> -f
jdt launch stop <name>
```

### Workspace Info

```bash
jdt projects
jdt project-info <project-name>
jdt editors
jdt git
```

### Open in Editor

```bash
jdt open "com.example.web.OrderController"
```

Open class directly in Eclipse editor.

## Common Use Cases

| Task | Command |
|------|---------|
| Who calls this method? | `jdt refs "ClassName#methodName"` |
| Find implementations | `jdt impl "InterfaceName#methodName"` |
| Find subtypes | `jdt subt "com.example.ParentClass"` |
| Full hierarchy | `jdt hier "com.example.ClassName"` |
| Read library source | `jdt src org.springframework...ClassName#method` |
| Check compile errors | `jdt err --project <project-name>` |
| Class overview | `jdt ti fully.qualified.ClassName` |
| Run tests | `jdt test run <test-class> -f` |
| Test status | `jdt test status <session> -f` |
| Build project | `jdt build --project <name>` |
| Find types | `jdt find "Order*"` |
| Open in editor | `jdt open "fully.qualified.ClassName"` |
| List projects | `jdt projects` |
| Git status | `jdt git` |
| Project status | `jdt status` |

## Input Format

**FQMN (Fully Qualified Method Name) formats:**
```
pkg.Class#method              any overload
pkg.Class#method()            zero-arg overload
pkg.Class#method(String)      specific signature
pkg.Class.method(String)      Eclipse Copy Qualified Name style
```

Examples:
```
jdt refs com.example.dao.UserDaoImpl#getStaff
jdt refs "com.example.dao.UserDaoImpl#save(Order)"
jdt refs com.example.dao.UserDaoImpl --field staffCache
```

- Use fully qualified class names: `com.example.dao.OrderRepository`
- Method format: `ClassName#methodName` or `ClassName#methodName(Type)`
- Project names should match Eclipse project names in the workspace

## Error Handling

If a command fails:
1. Run `jdt setup --check` to verify Eclipse plugin is installed
2. Check that the project is open in Eclipse/JDT workspace
3. Verify class name is fully qualified (FQMN format: `package.ClassName#methodName`)
4. For `jdt src`, ensure the library is on the classpath
5. For `jdt err` / `jdt build`, ensure the project exists and is built
6. For `jdt test run`, ensure JUnit/TestNG is configured

### Setup Required

```bash
jdt setup --check   # Check if plugin is installed
jdt setup           # Install Eclipse plugin
jdt setup --remove  # Remove plugin
```

## When to Use

- Java code navigation tasks
- Finding method references and implementations
- Reading JDK/Spring source during debugging
- Quick compilation checks (faster than Maven)
- Running Java tests with progress
- Understanding unfamiliar Java classes
