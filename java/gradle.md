# Gradle



## Gradle 시작하기



### Basic Project Structure

```
myproject/
├── build.gradle       # Main Gradle build file
├── settings.gradle    # Project name and modules
├── gradlew            # Gradle wrapper script
├── gradle/            # Wrapper config
└── src/
    └── main/java/...  # Your code
```

- Tips for Smooth Management

  - Use gradle.properties to define JVM args, versions, etc.

  - Use version catalog (with libs.versions.toml) for cleaner dependency versioning.

  - Use Kotlin DSL (build.gradle.kts) for better type safety in modern projects.

  - Use Spring Boot DevTools for live reloads in development.



### build.gradle Example (Groovy DSL)

```groovy
plugins {
    id 'org.springframework.boot' version '3.2.5'
    id 'io.spring.dependency-management' version '1.1.4'
    id 'java'
}

group = 'com.example'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '17'

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

test {
    useJUnitPlatform()
}
```

- `implementation` : For main runtime dependencies (recommended)                  
- `compileOnly` : Available at compile time only (e.g., Lombok)                
- `annotationProcessor` : For annotation-based code generation (used with Lombok, MapStruct) 
- `runtimeOnly` : Available at runtime, not at compile time                    
- `testImplementation` : For test-only dependencies (e.g., JUnit, Mockito)            



### Useful Gradle Commands

| **Command**            | **Description**             |
| ---------------------- | --------------------------- |
| ./gradlew build        | Compile, test, and package  |
| ./gradlew bootRun      | Run the Spring app          |
| ./gradlew test         | Run tests                   |
| ./gradlew dependencies | Show dependency tree        |
| ./gradlew clean        | Delete build artifacts      |
| ./gradlew tasks        | Show available Gradle tasks |

> Run these in the project root
>
> Use gradlew instead of gradle to ensure the correct version is used (wrapper-based).



### Managing Dependencies

Gradle uses **transitive dependency management** and a central place (dependencies {} block) for:

- Adding libraries
- Using implementation, testImplementation, etc.
- Overriding versions when needed





## Maven vs Gradle



### 공통점

**Gradle** and **Maven** are both **build automation tools** used primarily in **Java** and **JVM-based projects** (like Kotlin, Groovy). They help developers automate tasks like

- Compiling source code
- Downloading dependencies
- Running tests
- Packaging the code into JAR/WAR files
- Deploying to servers or repositories

#### What They Do

| **Task**                   | **Example**                                  |
| -------------------------- | -------------------------------------------- |
| Dependency management      | Fetch libraries from Maven Central           |
| Build lifecycle automation | compile, test, package, deploy               |
| Plugin support             | Run code quality tools, test coverage, etc.  |
| Multi-project builds       | Useful for enterprise apps with many modules |



### 차이점

| **Feature**           | **Maven**                                | **Gradle**                                        |
| --------------------- | ---------------------------------------- | ------------------------------------------------- |
| **Build file**        | XML (pom.xml)                            | Groovy/Kotlin DSL (build.gradle / .kts)           |
| **Verbosity**         | Verbose, boilerplate-heavy               | Concise and flexible                              |
| **Speed**             | Slower (no incremental build by default) | Faster (supports incremental and parallel builds) |
| **Custom logic**      | Harder to customize                      | Easily extendable via code                        |
| **Learning curve**    | Easier to learn                          | More complex but more powerful                    |
| **Default structure** | Strong conventions (fixed structure)     | Convention-based, but flexible                    |

:bulb: Summary

- **Maven**: Opinionated and stable; good for simple, standardized projects.
- **Gradle**: Modern and flexible; better for complex builds or large projects.



### **Usage Trends**

| **Context**                             | **More Popular Tool**                       |
| --------------------------------------- | ------------------------------------------- |
| **Android development**                 | **Gradle** (officially required)            |
| **Modern Java projects**                | **Gradle** (gaining traction)               |
| **Enterprise Java apps (Spring, etc.)** | **Maven** (still widely used)               |
| **Open-source libraries**               | **Maven** (due to simplicity and stability) |



### Overall: Gradle is gaining ground

- **Gradle** is increasingly preferred in modern projects due to:
  - Better performance (incremental builds, daemon)
  - Flexible build logic
  - Easier scripting with Kotlin DSL
- Many **new Spring Boot projects** are now started with Gradle (especially via Spring Initializr).
- However, **Maven** is still very common in **legacy codebases** and **large enterprises** due to its stability and mature ecosystem.



