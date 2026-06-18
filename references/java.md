# Java Project Analysis Rules

## Signature Detection
- `pom.xml` (Maven) or `build.gradle` / `build.gradle.kts` (Gradle) exists

## Scan Steps

### 1. Read Build File
**Maven (pom.xml)**:
- `groupId` + `artifactId` + `version` → Project identity
- `<parent>` → Spring Boot starter parent (if `spring-boot-starter-parent`)
- Dependencies → Key frameworks:
  - `spring-boot-starter-web` → Spring Boot web app
  - `spring-boot-starter-data-jpa` → JPA/Hibernate
  - `spring-boot-starter-security` → Spring Security
  - `spring-boot-starter-test` → Testing
  - `spring-cloud-starter-*` → Spring Cloud microservice
  - `lombok` → Lombok boilerplate reduction
  - `jackson-*` → JSON serialization
  - `mapstruct` → Object mapping
  - `hibernate-validator` → Bean validation
  - `junit-jupiter` → JUnit 5 testing
  - `mockito-*` → Mocking framework
- `<packaging>` → `jar` (app) or `war` (traditional web) or `pom` (parent/multi-module)
- `<modules>` → Multi-module sub-projects

**Gradle (build.gradle)**:
- Plugins: `java`, `org.springframework.boot`, `io.spring.dependency-management`
- Dependencies block → same identification as above
- `java.toolchain` → JDK version

### 2. Determine Build Tool
| Tool | Key File | Wrapper Script |
|---|---|---|
| Maven | `pom.xml` | `mvnw` / `mvnw.cmd` |
| Gradle | `build.gradle` | `gradlew` / `gradlew.bat` |

### 3. Map Directory Structure
**Standard Maven/Gradle layout:**
| Directory | Common Role |
|---|---|
| `src/main/java/<package>/` | Main source code |
| `src/main/resources/` | Properties, templates, static files |
| `src/test/java/<package>/` | Test code |
| `src/test/resources/` | Test fixtures |

**Common package patterns:**
| Package | Pattern |
|---|---|
| `*.controller` or `*.api` | REST endpoints |
| `*.service` | Business logic |
| `*.repository` or `*.dao` | Data access |
| `*.model` or `*.entity` or `*.domain` | Domain objects |
| `*.dto` | Data transfer objects |
| `*.config` | Configuration classes |
| `*.exception` | Custom exceptions |

### 4. Find Entry Points
```bash
grep "public static void main" in src/
```
```bash
grep "@SpringBootApplication" in src/
```
Most common: An `Application.java` or `Main.java` annotated with `@SpringBootApplication`.

### 5. Read Configuration
- `src/main/resources/application.properties` or `application.yml`
- Key Spring Boot properties: `server.port`, `spring.datasource.url`, `spring.profiles.active`
- Multiple profiles? → `application-{profile}.yml`

### 6. Identify Architecture
- **Layered**: Controller → Service → Repository (most common in Spring Boot)
- **Hexagonal/Ports & Adapters**: `*.port` / `*.adapter` packages
- **DDD**: `*.domain` + `*.infrastructure` + `*.application` packages
- **Microservice**: Dockerfile + service discovery dependencies (Eureka, Consul)
- **Reactive**: `WebFlux`, `R2DBC` dependencies

### 7. Build & Test Commands
**Maven:**
- Build: `./mvnw clean package` (skip tests: `-DskipTests`)
- Test: `./mvnw test`
- Run: `./mvnw spring-boot:run` or `java -jar target/<name>.jar`

**Gradle:**
- Build: `./gradlew build` (skip tests: `-x test`)
- Test: `./gradlew test`
- Run: `./gradlew bootRun` (Spring Boot) or `./gradlew run`
