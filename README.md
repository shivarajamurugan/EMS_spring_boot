# EMS — Employee Management System (Spring Boot)

## Overview

**EMS** (`com.tekpyramid.boot_sample`) is a Spring Boot REST API for managing employees, departments, leave requests, and authentication. Based on the most recent commit, the project has been enhanced with logging, auditing, caching, file upload, transaction management, Swagger/OpenAPI documentation, JPA entity mappings, and batch processing.

- **Repository**: `EMS_spring_boot`
- **Base package**: `com.tekpyramid.boot_sample`
- **Author**: shivarajamurugan

## Features

Based on the latest commit (`feat: enhance EMS with logging, auditing, caching, file upload, transactions, Swagger, JPA mappings and batch processing`):

- 🔐 **Authentication** — `AuthController` for login/auth flows
- 👥 **Employee Management** — CRUD operations across multiple API versions (`EmployeeController`, `EmployeeV2Controller`, `EmployeeControllerV3`)
- 🏢 **Department Management** — `DepartmentController`
- 🌴 **Leave Management** — request, approve/reject, and track leave balances (`LeaveController`, `LeaveRequestDto`, `LeaveResponseDto`, `LeaveBalanceResponseDto`, `LeaveMapper`)
- ⚡ **Caching** — Redis-backed caching via `RedisConfig` (JSON serialization, `@EnableCaching`)
- 📝 **Auditing & Logging** — commit history indicates auditing support (e.g., createdBy/updatedBy-style tracking) and structured logging
- 📁 **File Upload** — support for uploading files (e.g., employee documents)
- 🔄 **Transaction Management** — `@Transactional` boundaries around multi-step operations
- 📖 **API Documentation** — Swagger / OpenAPI integration
- 🗺️ **JPA Mappings** — entity relationships including an `@Embeddable EmployeeAddress`
- 📦 **Batch Processing** — bulk operations (e.g., batch employee import/export)
- ⚠️ **Custom Exception Handling** — e.g., `TestBusinessException` under a dedicated `exception` package

## Tech Stack

| Layer | Technology (inferred) |
|---|---|
| Language | Java |
| Framework | Spring Boot |
| Persistence | Spring Data JPA / Hibernate |
| Caching | Redis (Spring Cache abstraction) |
| API Docs | Swagger / springdoc-openapi |
| Boilerplate reduction | Lombok (`@Getter`, `@Setter`, `@Builder`, etc.) |
| Build tool | Maven or Gradle *(unconfirmed — no build file recovered)* |
| Database | Not confirmed — commonly MySQL/PostgreSQL/H2 for this stack |

## Project Structure

Recovered package structure:

```
com.tekpyramid.boot_sample
├── config/
│   └── RedisConfig.java          # Redis cache manager configuration
├── controller/
│   ├── AuthController.java
│   ├── DepartmentController.java
│   ├── EmployeeController.java
│   ├── EmployeeV2Controller.java
│   ├── EmployeeControllerV3.java
│   └── LeaveController.java
├── dto/
│   ├── LeaveRequestDto.java
│   ├── LeaveResponseDto.java
│   └── LeaveBalanceResponseDto.java
├── entity/
│   ├── Employee.java
│   ├── EmployeeAddress.java       # @Embeddable
│   └── Leave.java
├── exception/
│   └── TestBusinessException.java
└── util/
    └── LeaveMapper.java           # DTO <-> Entity mapping
```

> The presence of `EmployeeController`, `EmployeeV2Controller`, and `EmployeeControllerV3` suggests **API versioning** — likely iterative improvements to the employee endpoints (e.g., v1 basic CRUD, v2 adding validation/DTOs, v3 adding pagination/filtering or caching).

## Getting Started

### Prerequisites

- JDK 17+ (Spring Boot 3.x conventions, given `jakarta.persistence` imports)
- Maven or Gradle
- A relational database (MySQL/PostgreSQL, or H2 for local dev)
- Redis server (for caching support)

### Configuration

Set the following in `application.properties` / `application.yml` (adjust to your actual values):

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/ems_db
spring.datasource.username=your_username
spring.datasource.password=your_password

spring.jpa.hibernate.ddl-auto=update

spring.data.redis.host=localhost
spring.data.redis.port=6379
```

### Run the application

```bash
# Maven
mvn clean install
mvn spring-boot:run

# or Gradle
./gradlew bootRun
```

The API should then be available at `http://localhost:8080`, with Swagger UI typically at:

```
http://localhost:8080/swagger-ui.html
```

## API Overview (inferred)

| Resource | Controller | Likely Endpoints |
|---|---|---|
| Auth | `AuthController` | `POST /api/auth/login`, `POST /api/auth/register` |
| Employees | `EmployeeController` / `V2` / `V3` | `GET/POST/PUT/DELETE /api/v{n}/employees` |
| Departments | `DepartmentController` | `GET/POST/PUT/DELETE /api/departments` |
| Leave | `LeaveController` | `POST /api/leaves`, `GET /api/leaves/{employeeId}/balance` |

> Exact paths, request/response shapes, and HTTP verbs should be confirmed against the actual controller source — these are conventional guesses based on class names alone.

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Commit your changes
4. Push and open a Pull Request

## License

*Not specified in recovered files — add a `LICENSE` file if this project is intended to be open source.*

---

### 📌 To get a fully accurate README

Please share the **full project folder** (not just `.git`) — specifically:
- `pom.xml` or `build.gradle` (for real dependency/version info)
- `application.properties`/`.yml` (for actual DB/Redis config)
- The full `controller/`, `service/`, and `entity/` packages (for accurate endpoint documentation)

I can then regenerate this README with verified details instead of inferred ones.
