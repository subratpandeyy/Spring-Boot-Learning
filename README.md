# Ecommerce App

A full-stack e-commerce application built with **React.js** (frontend), **Spring Boot** (backend), and **H2** (in-memory database). This README explains project structure, how to run the app locally, configuration notes, and common tasks.

---

## Table of Contents

* [Overview](#overview)
* [Features](#features)
* [Tech Stack](#tech-stack)
* [Prerequisites](#prerequisites)
* [Getting Started (Local Development)](#getting-started-local-development)

  * [Backend (Spring Boot)](#backend-spring-boot)
  * [Frontend (React)](#frontend-react)
* [H2 Database Console](#h2-database-console)
* [Environment / Configuration](#environment--configuration)
* [API Overview (examples)](#api-overview-examples)
* [Build & Deployment](#build--deployment)
* [Project Structure (suggested)](#project-structure-suggested)
* [Testing](#testing)
* [Contributing](#contributing)
* [License](#license)

---

## Overview

This repository contains a sample e-commerce application demonstrating a typical modern web stack: a React single-page application that consumes a REST API served by Spring Boot. Data is persisted using H2 for easy local development and testing.

Use this project as a learning reference or starting point for a production-ready system by replacing H2 with a persistent store (PostgreSQL, MySQL, etc.), adding proper security and scaling concerns.

---

## Features

* Product listing and product details
* Basic cart functionality (client-side)
* User authentication and authorization (JWT or session — placeholder)
* Order placement workflow (create, view orders)
* Admin endpoints to manage products (CRUD)
* H2 in-memory database for quick setup and testing

---

## Tech Stack

* Frontend: React.js (Create React App or Vite)
* Backend: Spring Boot (Java)
* Database: H2 (in-memory, file-backed optional)
* Build tools: Maven or Gradle for backend; npm/yarn for frontend
* Optional: Lombok, Spring Security, JPA / Hibernate

---

## Prerequisites

* Java 17+ (or Java 11 depending on project config)
* Maven 3.6+ (or Gradle)
* Node.js 14+ and npm (or yarn)
* Git

---

## Getting Started (Local Development)

Clone the repository:

```bash
git clone <repo-url>
cd <repo-directory>
```

### Backend (Spring Boot)

1. Navigate to backend folder (if mono-repo example):

```bash
cd backend
```

2. Configure application properties if needed (see section [Environment / Configuration](#environment--configuration)).

3. Run with Maven:

```bash
mvn clean spring-boot:run
```

Or build and run jar:

```bash
mvn clean package
java -jar target/ecommerce-backend-0.0.1-SNAPSHOT.jar
```

Default backend server: `http://localhost:8080`

If using Gradle:

```bash
./gradlew bootRun
```

### Frontend (React)

1. Navigate to frontend folder:

```bash
cd frontend
```

2. Install dependencies:

```bash
npm install
# or
yarn install
```

3. Start development server:

```bash
npm start
# or
yarn start
```

Default frontend server: `http://localhost:3000`

If the frontend needs to talk to the backend on a different port, ensure you configure the API base URL (see [Environment / Configuration](#environment--configuration)).

---

## H2 Database Console

H2 is configured for local development. By default, the H2 web console is available at:

```
http://localhost:8080/h2-console
```

Default connection values (example — verify `application.properties` or `application.yml`):

* JDBC URL: `jdbc:h2:mem:ecomdb` (or `jdbc:h2:file:./data/ecomdb` for file-backed)
* Username: `sa`
* Password: (empty by default)

If the console path or credentials were changed, consult the backend configuration file.

---

## Environment / Configuration

Example Spring Boot properties (`src/main/resources/application.properties`):

```properties
server.port=8080

spring.datasource.url=jdbc:h2:mem:ecomdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console
spring.jpa.hibernate.ddl-auto=update
```

Example frontend `.env` (React):

```
REACT_APP_API_BASE_URL=http://localhost:8080/api
```

Make sure to restart servers after changing environment variables.

---

## API Overview (examples)

Below are example endpoints and how to call them. Adjust paths to match your implementation.

* Authentication

  * `POST /api/auth/register` — register user
  * `POST /api/auth/login` — authenticate and receive token/session

* Products

  * `GET /api/products` — list products
  * `GET /api/products/{id}` — product details
  * `POST /api/products` — create product (admin)
  * `PUT /api/products/{id}` — update product (admin)
  * `DELETE /api/products/{id}` — delete product (admin)

* Cart & Orders

  * `POST /api/cart` — add item to cart (or client-side cart)
  * `POST /api/orders` — place order
  * `GET /api/orders/{id}` — get order details

Example curl:

```bash
curl -X GET http://localhost:8080/api/products
```

If using JWT, include:

```bash
-H "Authorization: Bearer <token>"
```

---

## Build & Deployment

### Backend

Build artifact:

```bash
mvn clean package
# or
./gradlew bootJar
```

Deploy the produced JAR on any platform that runs Java. For Dockerize, create a Dockerfile and set Java runtime image, copy jar, and run.

### Frontend

Build production bundle:

```bash
npm run build
# or
yarn build
```

Serve static files via Nginx, Apache, or deploy to static hosting (Netlify, Vercel). If serving frontend from Spring Boot, copy build output into backend static resources (e.g., `src/main/resources/static`) or configure a proxy.

---

## Project Structure (suggested)

```
/backend
  ├─ src/main/java/...
  ├─ src/main/resources/application.properties
  ├─ pom.xml (or build.gradle)

 /frontend
  ├─ src/
  ├─ public/
  ├─ package.json

README.md
```

Adjust structure to match your repository organization.

---

## Testing

* Backend: run unit and integration tests with Maven:

```bash
mvn test
```

* Frontend: run React tests:

```bash
npm test
# or
yarn test
```

Add more tests to cover services, controllers, and UI interactions.

---

## Contributing

1. Fork repository
2. Create feature branch: `git checkout -b feat/your-feature`
3. Commit changes with clear messages
4. Create pull request with description of changes

Follow coding style and include unit tests for backend changes. For frontend, include component tests and lint fixes.

---

## Troubleshooting

* H2 console not loading: confirm `spring.h2.console.enabled=true` and check the console path.
* CORS issues: ensure backend allows requests from `http://localhost:3000` during development or configure a proxy.
* Port conflicts: change `server.port` or frontend port via environment variables.

---

## License

This project is provided under the MIT License. See `LICENSE` file for details.

---

If you want, I can generate:

* A `docker-compose.yml` that runs the backend, frontend, and a file-backed H2 or replace H2 with PostgreSQL for persistence.
* Example `application.yml` with profiles (`dev`, `prod`) and JWT authentication scaffolding.
* Boilerplate code snippets (Spring Boot controllers, JPA entities, React components).
