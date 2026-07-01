# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **Spring Boot 4.1.0 e-commerce application** built with Gradle (Kotlin DSL), using Java 17. The project integrates with Anthropic's Claude AI via Spring AI for intelligent features.

**Current Status**: Feature development in progress (feature/todo-app branch). Basic domain models (Member, Product) and infrastructure are set up; Order domain implementation planned.

## Technology Stack

- **Framework**: Spring Boot 4.1.0
- **Language**: Java 17
- **Build Tool**: Gradle 8.x (Kotlin DSL)
- **Database**: H2 (in-memory, configured for development)
- **ORM**: Spring Data JPA
- **Web**: Spring MVC
- **AI Integration**: Spring AI 2.0.0 with Anthropic Claude
- **Code Generation**: Lombok
- **Testing**: JUnit 5
- **IDE Support**: IntelliJ IDEA (build.gradle.kts configured for IDE toolchain)

## Build & Test Commands

### Build the project
```bash
./gradlew build
```

### Run the application
```bash
./gradlew bootRun
```

### Run tests
```bash
./gradlew test
```

### Run a specific test class
```bash
./gradlew test --tests ClassName
```

### Run a specific test method
```bash
./gradlew test --tests ClassName.methodName
```

### Clean build
```bash
./gradlew clean build
```

### Check dependencies
```bash
./gradlew dependencies
```

### Build without running tests
```bash
./gradlew build -x test
```

## Project Structure

```
src/main/java/common/claude/commoncluade/
├── CommonCluadeApplication.java          # Spring Boot entry point
├── global/
│   ├── config/                           # Configuration classes (JPA, QueryDSL, Security, etc.)
│   ├── entity/                           # Base entity classes
│   ├── exception/                        # Exception handling and error codes
│   └── common/                           # Common DTOs and utilities
├── member/
│   └── domain/                           # Member entity and related domain models
├── product/
│   └── domain/                           # Product entity and related domain models
└── order/
    └── domain/                           # Order entity (planned)

src/main/resources/
├── application.properties                # Spring configuration
└── (static/, templates/ for future UI)

src/test/java/common/claude/commoncluade/
└── CommonCluadeApplicationTests.java     # Basic application test
```

## Architecture Patterns

### Domain-Driven Design
- Project is organized by domain (member, product, order)
- Each domain contains entity models and business logic
- Separation of concerns: `domain/` contains JPA entities and core business rules

### JPA & Repository Pattern
- Spring Data JPA provides automatic repository implementations
- Entities inherit from `BaseEntity` for common fields (id, createdAt, updatedAt, etc.)
- Repositories define custom queries using `@Query` annotations or method naming conventions

### Configuration
- `JpaConfig`: JPA/Hibernate configuration
- `QueryDslConfig`: QueryDSL setup for type-safe queries (planned for complex queries)
- `SecurityConfig`: Spring Security configuration (planned)
- `GlobalExceptionHandler`: Centralized exception handling

### Error Handling
- Custom `ErrorCode` enum for standardized error responses
- `BusinessException` for domain-specific exceptions
- `GlobalExceptionHandler` provides consistent API error responses via `ApiResponse`

## Development Setup

### Prerequisites
- Java 17 installed and set as JAVA_HOME
- Gradle wrapper (gradlew) handles version management

### IDE Setup
- Open project in IntelliJ IDEA
- Enable Annotation Processing for Lombok (Settings → Build, Execution, Deployment → Compiler → Annotation Processors → Enable annotation processing)
- Gradle should auto-sync on project open

### Database
- H2 database is configured for development (in-memory by default)
- Access H2 console at `http://localhost:8080/h2-console` when app is running
- Connection string: `jdbc:h2:mem:testdb`

### Running Tests
- Tests use `@SpringBootTest` for integration testing
- JUnit 5 with JUnitPlatform configured
- Test database uses H2 in-memory mode by default

## Key Implementation Notes

### Lombok Usage
- All entities use `@Getter`, `@Setter`, `@NoArgsConstructor`, `@AllArgsConstructor`
- Entities may use `@Builder` for fluent construction
- Override `equals` and `hashCode` using `@EqualsAndHashCode` when needed for JPA collections

### JPA Entity Best Practices
- Entities use `@Entity` with table names following convention: `table_name` (snake_case)
- Primary keys use `@Id @GeneratedValue(strategy = GenerationType.IDENTITY)`
- All relationships properly annotated with `@OneToMany`, `@ManyToOne`, `@OneToOne`, etc.
- Implement `@CreationTimestamp` and `@UpdateTimestamp` for audit fields (in `BaseEntity`)

### Spring AI Integration
- Anthropic Claude is configured via Spring AI
- AI beans are registered in application context for dependency injection
- Planned: Chat endpoints for user interactions with Claude

## Testing Strategy

- **Unit Tests**: Service layer logic with mocked repositories
- **Integration Tests**: Full Spring context loaded (use `@SpringBootTest`)
- **Database Tests**: Use H2 in-memory database for fast, isolated test runs
- Use `TestRestTemplate` for REST endpoint testing in integration tests

## Common Development Tasks

### Adding a New Domain
1. Create domain package: `src/main/java/common/claude/commoncluade/{domain}/domain/`
2. Create entity class extending `BaseEntity`
3. Create repository interface extending `JpaRepository<Entity, Long>`
4. Define repositories in `src/main/java/common/claude/commoncluade/{domain}/repository/`
5. Add tests in `src/test/java/common/claude/commoncluade/{domain}/`

### Adding a REST Endpoint
1. Create controller in `{domain}/controller/`
2. Inject repository and/or service via constructor
3. Use `@RestController`, `@RequestMapping`, `@GetMapping`, etc.
4. Return `ApiResponse` wrapper for consistency
5. Add integration test in test tree

### Running Against IntelliJ
- Use "Run" button next to `CommonCluadeApplication.main()` or
- In terminal: `./gradlew bootRun`
- Application starts on `http://localhost:8080` by default

## Git Workflow

- **Main Branch**: Production-ready code
- **Feature Branches**: Feature development (currently on `feature/todo-app`)
- Create PRs for feature branches before merging to main
- Commits follow conventional commit format (chore:, feat:, fix:, etc.)

## Notes for Future Work

- QueryDSL integration for complex domain queries
- REST API endpoints for each domain (GET, POST, PUT, DELETE)
- Service layer for business logic orchestration
- DTO pattern for request/response mapping
- Spring Security integration for authentication/authorization
- Actuator endpoints for monitoring
- Spring AI chat features using Claude API
