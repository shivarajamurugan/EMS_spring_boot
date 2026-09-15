# EMS_spring_boot
# EMS Spring Boot — Employee Management System

A REST API built with Spring Boot for managing employees, departments, and leave requests. It includes JWT-based authentication, Redis caching, batch CSV import for employee data, and API documentation via Swagger/OpenAPI.

## Features

- **Authentication** — Stateless JWT login (`/login`) backed by Spring Security.
- **Employee management** — CRUD operations, paginated listing, department assignment, and multiple API versions (`v1`, `v2`, `v3`, including multipart file upload support in v3).
- **Department management** — Create, fetch, list, and delete departments.
- **Leave management** — Apply for leave, approve/reject/cancel requests, and track leave balances per employee.
- **Batch processing** — Spring Batch job for bulk-importing employees from a CSV file.
- **Caching** — Redis integration for improved read performance.
- **API docs** — Interactive Swagger UI via springdoc-openapi.
- **Monitoring** — Spring Boot Actuator endpoints (health, info, metrics).

## Tech Stack

| Component        | Technology                          |
|-------------------|--------------------------------------|
| Language           | Java 21                              |
| Framework          | Spring Boot 4.0.8                    |
| Persistence        | Spring Data JPA + MySQL              |
| Security           | Spring Security + JWT (jjwt 0.13.0)  |
| Caching            | Spring Data Redis                    |
| Batch Processing   | Spring Batch                         |
| API Documentation  | springdoc-openapi (Swagger UI)       |
| Build Tool         | Maven                                |
| Boilerplate        | Lombok                               |

## Prerequisites

- JDK 21+
- Maven (or use the included `mvnw` / `mvnw.cmd` wrapper)
- MySQL Server (running locally or reachable)
- Redis Server (running locally or reachable)

## Getting Started

### 1. Clone and configure the database

Create the MySQL database referenced in `application.properties`:

```sql
CREATE DATABASE simple_boot_emp_db;
```

### 2. Configure application properties

Edit `src/main/resources/application.properties` as needed:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/simple_boot_emp_db
spring.datasource.username=root
spring.datasource.password=root

spring.jpa.hibernate.ddl-auto=update

server.port=8097

# Redis
spring.data.redis.host=localhost
spring.data.redis.port=6379
```

> **Note:** For anything beyond local development, move the datasource credentials out of the properties file and into environment variables or a secrets manager.

### 3. Start Redis and MySQL

Make sure both services are running and reachable at the hosts/ports configured above.

### 4. Build and run the application

```bash
# Using the Maven wrapper
./mvnw spring-boot:run

# Or, if you have Maven installed
mvn spring-boot:run
```

The application starts on **`http://localhost:8097`** (configurable via `server.port`).

### 5. Explore the API

Once running, open Swagger UI:

```
http://localhost:8097/swagger-ui/index.html
```

## Authentication

Most endpoints require a valid JWT. Obtain one via the login endpoint, then include it in the `Authorization` header for subsequent requests.

```
POST /login
Content-Type: application/json

{
  "username": "your-username",
  "password": "your-password"
}
```

Response: a JWT string. Use it as:

```
Authorization: Bearer <token>
```

`/login`, `/swagger-ui/**`, and `/v3/api-docs/**` are the only endpoints that don't require authentication.

## API Overview

### Employees (v1) — `/employee`, `/employees`
| Method | Endpoint                                            | Description                  |
|--------|------------------------------------------------------|-------------------------------|
| POST   | `/employee`                                          | Create an employee            |
| GET    | `/employee/{id}`                                     | Get employee by ID            |
| GET    | `/employees`                                         | List all employees            |
| PUT    | `/employee/updateall/{id}`                           | Full update of an employee    |
| PATCH  | `/employee/update/{id}`                              | Partial update of an employee |
| DELETE | `/employee/delete/{id}`                              | Delete an employee            |
| PUT    | `/employee/{employeeId}/department/{departmentId}`   | Assign employee to department |
| POST   | `/employees/details`                                 | Bulk fetch employee details   |

### Employees (v2) — `/api/v2/employee`
| Method | Endpoint            | Description               |
|--------|----------------------|----------------------------|
| GET    | `/api/v2/employee`   | Paginated employee listing |

### Employees (v3) — `/api/v3/employees`
| Method | Endpoint                              | Description                              |
|--------|-----------------------------------------|-------------------------------------------|
| POST   | `/api/v3/employees` (multipart/form-data) | Create employee with document upload    |
| GET    | `/api/v3/employees/test-no-rollback`    | Transaction rollback test endpoint        |

### Departments — `/department`
| Method | Endpoint                     | Description             |
|--------|-------------------------------|---------------------------|
| POST   | `/department/save`            | Create a department       |
| GET    | `/department/fetch/{id}`      | Get department by ID      |
| GET    | `/department/fetchall`        | List all departments      |
| DELETE | `/department/delete/{id}`     | Delete a department       |

### Leaves — `/api/v1/leaves`
| Method | Endpoint                              | Description                        |
|--------|-----------------------------------------|--------------------------------------|
| POST   | `/api/v1/leaves`                        | Apply for leave                     |
| GET    | `/api/v1/leaves`                        | List leave requests                 |
| PUT    | `/api/v1/leaves/{leaveId}/status`       | Approve/reject a leave request      |
| PUT    | `/api/v1/leaves/{leaveId}/cancel`       | Cancel a leave request              |
| GET    | `/api/v1/leaves/balance/{employeeId}`   | Get leave balance for an employee   |
| POST   | `/api/v1/leaves/balance/initialize`     | Initialize leave balance            |

## Batch Import

Employee records can be bulk-imported from `src/main/resources/employees.csv` via the Spring Batch job configured in `EmployeeBatchConfig`. The batch metadata schema is auto-initialized on startup (`spring.batch.jdbc.initialize-schema=always`).

## Monitoring

Actuator endpoints are exposed at:

```
GET /actuator/health
GET /actuator/info
GET /actuator/metrics
```

## Project Structure

```
src/main/java/com/tekpyramid/boot_sample/
├── batch/          # Spring Batch job configuration (CSV import)
├── config/         # Security, JWT filter, Redis, password encoding
├── controller/     # REST controllers (Employee, Department, Leave, Auth)
├── dto/            # Request/response DTOs
├── entity/         # JPA entities (Employee, Department, Leave, etc.)
├── exception/      # Custom exceptions and global exception handling
├── repository/     # Spring Data JPA repositories
├── services/       # Business logic and service interfaces/implementations
└── util/           # Mappers (entity <-> DTO)
```

## Running Tests

```bash
./mvnw test
```

## License

No license specified. Add a `LICENSE` file if you intend to distribute this project.
