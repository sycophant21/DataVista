# DataVista — A Database Visualizer

> SQL queries shouldn't require a DBA standing over your shoulder.

A web application that lets users execute and visualize database queries through a browser — with role-based table access controlled by an admin.

## What it does

- **Login / Sign Up** with JWT-based session cookies
- **Table browser**: see all tables the current user is permitted to access
- **Query runner**: execute `SELECT` queries on accessible tables; results displayed as an HTML table
- **Admin panel**: grant or revoke per-table permissions (Read, Insert, Update, Delete) per user

## Built with

- [Spring Boot](https://spring.io/projects/spring-boot) — web framework
- [MySQL](https://www.mysql.com/) — relational data store
- [MongoDB](https://www.mongodb.com/) — supplementary NoSQL store
- [JSP](https://jakarta.ee/specifications/pages/) + [JSTL](https://jakarta.ee/specifications/tags/) — server-side templating

## Dependencies

- `spring-boot-starter-web` — HTTP layer
- `spring-boot-starter-jetty` — embedded Jetty server
- `spring-boot-starter-jdbc` — JDBC connection management
- `tomcat-jdbc` — JDBC connection pool
- `mysql-connector-java` — MySQL driver
- `jwt` (auth0) — JSON Web Token authentication
- `JSTL` + `taglibs-standard-impl` — JSP tag libraries
- `JUnit` — unit testing

## Database schema

**users table**

| Column    | Type         |
|-----------|--------------|
| Email     | VARCHAR(255) |
| Password  | VARCHAR(255) |
| isAdmin   | BOOLEAN      |

**permissions table**

| Column    | Type         |
|-----------|--------------|
| Email     | VARCHAR(255) |
| TableName | VARCHAR(255) |
| Insert    | BOOLEAN      |
| Read      | BOOLEAN      |
| Update    | BOOLEAN      |
| Delete    | BOOLEAN      |

## Getting started

### Prerequisites

- Java 8+
- Gradle 6+
- MySQL running locally (default port 3306)
- MongoDB running locally (default port 27017)

### Configure

1. Open `DBVisualizer/src/main/java/com/test/config/JDBCConfig.java` — set your MySQL host, database name, username, and password.
2. Create the `users` and `permissions` tables (DDL above) in your MySQL database.
3. Insert at least one admin user (set `isAdmin = true`).

### Build & run

```bash
cd DBVisualizer
./gradlew bootRun
```

Or build a JAR:

```bash
./gradlew clean build
java -jar build/libs/DBVisualizer-*.jar
```

The app starts on `http://localhost:8080` by default.

## How it works

- **SignUpController**: inserts a new user with `isAdmin = false`
- **LoginController**: validates credentials, sets a JWT session cookie
- **HomeController**: fetches all table names from MySQL; JSP renders the list
- **TableController**: checks permissions for the requested table; if granted, executes the user's query via `QueryBuilder` → `SQLHandler` and returns results
- **QueryBuilder**: assembles parameterized SQL from `select`, `from`, `where`, `limit` inputs
- **SQLHandler**: executes queries; maps rows to `Table`/`Row` domain objects

## In development

- MongoDB query builder
- Admin UI for permission assignment
- Full UI polish
