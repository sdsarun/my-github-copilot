---
agent: build-error-resolver
description: Fix Java and Spring/Quarkus build errors — Maven/Gradle, compilation errors, annotation processing, and dependency issues
---

# Java Build Fix

Analyze and fix the Java build failure shown below.

## Step 1 — Collect Error Output

**Maven:**

```bash
mvn compile -e 2>&1 | tail -60
mvn test -e 2>&1 | tail -60
```

**Gradle:**

```bash
./gradlew compileJava --stacktrace 2>&1 | tail -80
./gradlew test 2>&1 | tail -60
```

Paste the full compiler error output including line numbers.

## Step 2 — Categorize Errors

| Error Pattern                                                     | Meaning                                                    |
| ----------------------------------------------------------------- | ---------------------------------------------------------- |
| `cannot find symbol`                                              | Missing import, typo, or method/field does not exist       |
| `incompatible types`                                              | Type mismatch — check generics and casting                 |
| `method X in class Y cannot be applied to given types`            | Wrong argument types — check overload resolution           |
| `class X is public, should be declared in a file named X.java`    | Class name does not match file name                        |
| `package X does not exist`                                        | Missing dependency — add to `pom.xml` or `build.gradle`    |
| `no suitable constructor found`                                   | Constructor signature changed or wrong constructor called  |
| `unreported exception X; must be caught or declared to be thrown` | Add `throws`, use `try/catch`, or handle checked exception |
| `variable X might not have been initialized`                      | Ensure variable is initialized on all code paths           |

## Step 3 — Fix Strategy

**Cannot find symbol**

- Check import statements — add missing `import`
- Verify method/field spelling and case (Java is case-sensitive)
- Check if method is in a parent class — may need to call `super.method()`

**Spring Boot / Quarkus annotation issues**

- Missing `@Component`, `@Service`, `@Repository` for bean discovery
- Missing `@Autowired` / constructor injection
- Missing `@Transactional` on repository or service methods
- CDI scope mismatch (`@RequestScoped` injected into `@ApplicationScoped`)

**Gradle/Maven dependency missing**

```xml
<!-- Maven pom.xml -->
<dependency>
    <groupId>com.example</groupId>
    <artifactId>library</artifactId>
    <version>1.0.0</version>
</dependency>
```

```groovy
// Gradle build.gradle
implementation 'com.example:library:1.0.0'
```

**Annotation processing failures (Lombok, MapStruct)**

- Ensure annotation processor is configured in build file
- Clean and rebuild: `mvn clean compile` or `./gradlew clean compileJava`

## Step 4 — Verify

```bash
mvn compile        # or ./gradlew compileJava
mvn test           # or ./gradlew test
```

## Output

List each fix applied:

```
File: src/main/java/com/example/File.java
Change: [What was changed and why]
```

Confirm with final build output.
