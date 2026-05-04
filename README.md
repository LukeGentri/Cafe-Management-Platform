# Cafe Management Platform

> **Note:** This repository is a public showcase. The full source code is private.
> If you'd like to know more about the implementation, design choices, or technical decisions, I'm open to discussion — feel free to reach out.

### 📑 Table of Contents
- [Project Overview](#project-overview)
- [Backend Design](#backend-design)
  - [Entity / DTOs / Mapper / Repo](#entity--dtos--mapper--repo)
  - [Config / Security / Exception](#config--security--exception)
  - [Service / Implementation / Controller / APIs](#service--implementation--controller--apis)
- [Testing and Coverage](#testing-and-coverage)
- [Branch Management](#branch-management)
- [Setup Guide](#setup-guide)
  - [Environment Setup](#environment-setup)
  - [Backend Setup](#backend-setup)
  - [Frontend Setup](#frontend-setup)
  - [Optional Jenkins Setup](#optional-jenkins-setup)
- [Contributors](#contributors)

# Project Overview

WolfCafe is a full-stack coffee shop management system built for a CSC 326-style course project. It combines a Spring Boot backend with a React/Vite frontend and implements secure authentication, role-based features, inventory and order management, and admin tools.

🔧 Features
- 🔐 JWT-backed authentication for secure login and role-based access control
- 👤 Multiple user roles: `ROLE_CUSTOMER`, `ROLE_BARISTA`, `ROLE_STAFF`, `ROLE_MANAGER`, `ROLE_ADMIN`
- 🧾 Inventory, ingredients, recipes, orders, user management, tax rate, and loyalty point support
- ⚙️ REST API backend with Spring Boot, Spring Security, Spring Data JPA, and MySQL
- ⚛️ Responsive frontend using React, Vite, Bootstrap, and Axios
- 🧪 Automated tests for backend and frontend using JUnit and Vitest

🧰 Tech Stack
- Backend: Java 21, Spring Boot, Spring Security, JWT, MySQL, Maven
- Frontend: React, Vite, Bootstrap, Axios, Vitest
- Build & tooling: Maven, npm, Node.js

> This project is designed as a college application and is structured for easy extension, testing, and deployment.

# Backend Design

## Entity / DTOs / Mapper / Repo
- Backend source lives in `wolf-cafe-backend/src/main/java/edu/ncsu/csc326/wolfcafe`
- Entities and DTOs model users, roles, ingredients, inventory items, recipes, orders, tax rates, and loyalty points
- ModelMapper and custom mappers translate between entities and DTOs for clean REST payloads
- Repositories use Spring Data JPA for database access and CRUD operations

## Config / Security / Exception
- `SpringSecurityConfig` defines JWT security, authentication manager, and request authorization
- `JwtAuthenticationFilter`, `JwtTokenProvider`, and `JwtAuthenticationEntryPoint` handle token creation, validation, and authorization failures
- `SetupDataLoader` initializes roles and default admin user data on application startup
- Custom exception handling is present for invalid requests and data errors

## Service / Implementation / Controller / APIs
- Service layer separates business logic from controllers
- Controllers expose REST endpoints under `/api/*` routes
- Key controllers include:
  - `AuthController` for registration and login
  - `UserController` for admin user management and loyalty points
  - `IngredientController` for ingredient management
  - `InventoryController` for stock and inventory operations
  - `RecipeController` for recipe queries and management
  - `OrderController` for order creation, retrieval, and user order history
  - `TaxRateController` for tax rate retrieval and updates

# Architecture and API Surface

## System Architecture
- The backend is a layered Spring Boot application with separate controller, service, repository, entity, and security modules.
- Authentication is JWT-based, with token issuance handled by `JwtTokenProvider` and security enforced via `JwtAuthenticationFilter`.
- Authorization is role-based using Spring Security annotations (`@PreAuthorize`) across controller endpoints.
- Data persistence is implemented using Spring Data JPA and MySQL, with entity relationships representing users, roles, inventory, recipes, and orders.
- The frontend is a single-page React application built with Vite, using Axios for API communication and Bootstrap for responsive UI.
- The application separates concerns cleanly: backend REST APIs handle business logic and data access, while the frontend focuses on user interaction and presentation.

## API Surface
- `POST /api/auth/register` — user registration with username, email, password
- `POST /api/auth/login` — JWT login endpoint returning bearer token and role
- `GET /api/users` — admin-only user list and management operations
- `GET /api/ingredients` — ingredient inventory listing and CRUD operations for authorized roles
- `GET /api/inventory` — inventory snapshot accessible to baristas, managers, and admins
- `GET /api/recipes` — recipe catalog queries with lookup by ID or name
- `POST /api/orders` — create orders and retrieve order history by user
- `GET /api/tax` — tax rate retrieval and updates
- Role checks and endpoint access are enforced through controller-level security annotations

# Testing and Coverage

## Backend Tests
- Backend unit and integration tests are implemented with JUnit and Mockito
- Run tests from `wolf-cafe-backend/src/test/java` or with Maven

## Frontend Tests
- Frontend test tooling uses Vitest and React Testing Library
- Test files are located under `wolf-cafe-frontend/src/test`

### Run Frontend Tests
- In `wolf-cafe-frontend`, install dependencies:
  ```bash
  npm install
  ```
- Run tests:
  ```bash
  npm run test
  ```
- Run coverage:
  ```bash
  npm run test:coverage
  ```

### Run Backend Tests
- From `wolf-cafe-backend`, run with Maven:
  ```bash
  ./mvnw test
  ```
- Or use your IDE to run tests under `src/test/java`

# Branch Management
- Keep feature work on dedicated branches off `development`
- Split large changes into backend and frontend branches when needed
- Example:
  - `main <- development <- feature-stock <- feature-stock-be`
  - `main <- development <- feature-stock <- feature-stock-fe`

# Setup Guide

## Environment Setup
1. Install JDK 21, Maven, Node.js, and npm
2. Install MySQL and confirm local connectivity
3. Optional: Use MySQL Workbench for database management
4. Configure environment variables or local properties for database credentials and JWT secret

## Backend Setup
1. Open `wolf-cafe-backend` as a Maven project in your IDE
2. Install Lombok support in your IDE if needed
3. Copy `application.properties.template` files into `src/main/resources` and `src/test/resources`, removing the `.template` extension
4. Update `application.properties`:
   - `spring.datasource.url`
   - `spring.datasource.username`
   - `spring.datasource.password`
   - `app.jwt-secret`
   - `app.jwt-expiration-milliseconds`
   - `app.admin-user-password`
5. Generate a secure JWT secret. One option is SHA-256 or a base64-encoded secret value
6. Run Maven update and build:
   ```bash
   ./mvnw clean package
   ```
7. Launch the application:
   ```bash
   ./mvnw spring-boot:run
   ```

## Frontend Setup
1. Open `wolf-cafe-frontend` in your IDE or terminal
2. Install frontend dependencies:
   ```bash
   npm install
   ```
3. Start the development server:
   ```bash
   npm run dev
   ```
4. Open the app in your browser at `http://localhost:3000`
5. Ensure backend is running at `http://localhost:8080` before using the UI

## Optional Jenkins Setup
- Jenkins is optional for CI/CD and can be configured to run Maven and frontend tests
- Add credentials for GitHub and any environment secrets
- Create a pipeline to build `wolf-cafe-backend` and test `wolf-cafe-frontend`
- This project does not include a ready-made Jenkinsfile, so define your own pipeline stages if you choose to use Jenkins

## Contact

**Luke Gentri**
CS @ NC State · Graduated May 2026
[lukegentri1@gmail.com](mailto:lukegentri1@gmail.com) · [LinkedIn](https://www.linkedin.com/in/luke-gentri-384b033a4/)

If you want to dig into any specific component, I'm happy to walk through it.
