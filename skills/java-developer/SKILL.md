---
name: java-core
description: Expert Java developer skill for core Java language and JVM platform. Focuses on Java 17+ features (records, sealed classes, pattern matching, var), concurrency, performance, memory management, and low‑level JVM development. Use when writing Java applications, optimizing performance, working with threads, or building libraries without heavy frameworks.
license: Apache-2.0
compatibility: Designed for Java 17+ projects with Maven or Gradle. Works in any Java development environment.
metadata:
  language: Java 17+
  paradigm: Object-Oriented, Concurrent, Functional (modern)
  version: "1.0"
---

When the user asks you to write Java code or discuss Java concepts, follow these guidelines carefully.

## CRITICAL RULES – NEVER VIOLATE THESE

**🚫 ABSOLUTELY FORBIDDEN:**
1. **Never use raw types** – always provide generic type parameters.
2. **Never ignore checked exceptions** – catch or declare them appropriately.
3. **Never use `Thread.stop()` or `Thread.suspend()`** – these are deprecated and unsafe.
4. **Never synchronize on `String` literals or `Boolean`** – they are interned and can cause deadlocks.
5. **Never use `System.gc()` explicitly** – it’s a hint to the JVM, not a reliable memory management tool.
6. **Never use `finalize()`** – it’s deprecated; use `Cleaner` or try‑with‑resources.
7. **Never hardcode configuration values** – use environment variables, properties files, or external configuration.
8. **Never block in event loops** – use asynchronous APIs or dedicated threads.

**✅ ALWAYS DO:**
1. **Prefer immutability** – use `record`, `final` fields, and unmodifiable collections.
2. **Use modern Java features** – records, sealed classes, pattern matching, `var` (when appropriate), text blocks.
3. **Handle resources with try‑with‑resources** – for `Closeable` objects.
4. **Use `java.util.concurrent` utilities** instead of low‑level `wait/notify`.
5. **Log with a proper logging framework** (SLF4J, java.util.logging) – avoid `System.out`.
6. **Write tests with JUnit 5** – test edge cases, concurrency, and performance where needed.
7. **Document public APIs with Javadoc** – include `@param`, `@return`, `@throws`.
8. **Use meaningful variable names** – even in short‑lived lambdas.

---

## WHEN GENERATING CODE

**FIRST: Identify the context**
- Is this a performance‑critical section? → Prefer primitive types, avoid object allocation.
- Is this a concurrent part? → Use thread‑safe data structures, consider virtual threads (Java 21+).
- Is this a library/API? → Design for immutability, provide builders or factory methods.

**For every code example you provide:**
1. Check: Does it use `var` where type is obvious? → If yes, keep it; if not, consider explicit type.
2. Check: Is the code thread‑safe? → If multiple threads access mutable state, ensure synchronization or use concurrent structures.
3. Check: Are resources closed? → Use try‑with‑resources.
4. Check: Are exceptions handled properly? → Don’t swallow exceptions, log them.
5. Check: Is the code using modern Java idioms? → Prefer records for data carriers, pattern matching for instanceof, switch expressions.

**Default code structure for a data‑carrier class:**
```java
// ✅ CORRECT – Use record for immutable data
public record Point(int x, int y) {
    // Compact constructor for validation
    public Point {
        if (x < 0 || y < 0) {
            throw new IllegalArgumentException("Coordinates must be non‑negative");
        }
    }
}
