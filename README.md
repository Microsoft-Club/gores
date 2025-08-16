# gores - the Go Microservice framework

[![Go](https://img.shields.io/badge/go-1.20-blue.svg)](https://golang.org)
[![GitHub Release](https://img.shields.io/github/v/release/Muhammad-Ali-Khan9/gores)](https://github.com/Muhammad-Ali-Khan9/gores/releases)  

---

## Introduction

**Go Microservice framework** is a command-line tool designed to rapidly generate microservice boilerplate code for Go backends. It scaffolds a production-ready project structure including routers, controllers, services, entities, Dockerfiles, and essential configuration files. The generated services follow best practices for modular and maintainable microservice development.

This tool supports generating RESTful microservices with plans to extend support for GraphQL and gRPC APIs.

---

## Features

- Generate boilerplate code for Go microservices with a clean architecture  
- Auto-assign or specify network ports intelligently avoiding conflicts  
- Create shared packages for entities, database connections, and HTTP middleware  
- CLI-based interaction powered by [Cobra](https://github.com/spf13/cobra)  
- Cross-platform support with pre-built binaries for Linux, macOS, and Windows  
- Automated GitHub Actions workflow for seamless releases

---

### What `gores` Generates for You (Detailed)

`gores` aims to give you a head start by providing a well-structured and functional microservice skeleton:

#### 1. Clean Project Structure
Your generated service will adhere to common Go project layout recommendations:
-   **`cmd/main.go`** — The entry point for the microservice application.
-   **`internal/`** — Contains internal packages for the service's specific logic (e.g., `router.go`, `controller.go`, `service.go`). This structure promotes modularity and clean architecture.
-   **`pkg/` (Shared)** — A core part of the monorepo, containing shared packages:
    -   **`entities/`**: Defines common data models like `User` (with `Email`, `Name`, `PasswordHash`, `CreatedAt`, `UpdatedAt`) and other domain entities. The `User` entity is designed for secure password handling with **bcrypt password hashes**.
    -   **`database/postgres/`**: Provides a reusable function for connecting to a PostgreSQL database using GORM.
    -   **`http/middleware/`**: Houses global HTTP middleware (e.g., for JWT authentication, API key validation, CORS, logging).

#### 2. Environment-aware Configuration
-   Services load configuration from `.env` files using [`godotenv`](https://github.com/joho/godotenv), allowing easy management of environment-specific settings (like database credentials, JWT secrets, ports).

#### 3. PostgreSQL Integration via GORM
-   A robust **GORM ORM** integration for PostgreSQL database interactions.
-   Database connection details (host, port, user, password, SSL mode) are read from environment variables.
-   Includes database connection health checking on startup and graceful closing during shutdown.
-   **Automigrations**: Services can be configured to automatically migrate database schemas based on your GORM models.

#### 4. HTTP Server with Fiber ⚡
-   Leverages the high-performance **Fiber** web framework for building HTTP APIs.
-   Automatically sets up basic routing and integrates your shared HTTP middleware.
-   Includes routes for basic operations (e.g., health checks, user upsert/login, user CRUD if applicable).

#### 5. Secure Authentication & Authorization
-   The default `auth-service` implements **production-ready bcrypt password hashing** for storing user credentials securely.
-   Facilitates **JWT (JSON Web Token) issuance** upon user registration/login for application-specific authentication.
-   Includes **middleware for JWT and API Key based authentication**, allowing you to protect your service endpoints with flexible access control (e.g., requiring **EITHER** a valid JWT **OR** an API Key for certain routes).

#### 6. Graceful Shutdown
-   All generated services are equipped with **graceful shutdown** logic, ensuring that HTTP servers and database connections are closed cleanly upon receiving `SIGINT` (Ctrl+C) or `SIGTERM` signals, preventing data corruption and resource leaks.

#### 7. Docker Multi-stage Build 🐳
-   Each generated service includes an **optimized Dockerfile** utilizing multi-stage builds.
-   This compiles a statically linked Go binary in a build stage (using `golang:alpine`) and copies it into a tiny `scratch` image for the final production container, resulting in extremely small and secure Docker images.
-   Essential runtime components like **CA certificates** are copied to enable secure outgoing connections.
-   Service ports and other configurations are managed via environment variables within the Docker image, allowing easy deployment configuration.

---

## Benefits

- **Save Time:** Instantly scaffold all the repetitive setup code
- **Best Practices:** Follow idiomatic Go conventions and clean architecture
- **Extendable:** Easily add your business logic in services and controllers
- **Environment Friendly:** Automatically handles `.env` loading per environment
- **Production Ready:** Built-in graceful shutdown, logging, and Docker support

---

## Installation

### 1. Install via Go CLI (source install)

If you have Go installed, you can directly install the CLI tool using:

```bash
go install github.com/Muhammad-Ali-Khan9/gores/cmd@latest
```

This will compile and install the gores executable in your $GOPATH/bin or $HOME/go/bin directory.
Make sure your Go bin path is in your PATH environment variable:

```bash
export PATH=$PATH:$(go env GOPATH)/bin
```

You can then run:

 1. first initialze the project
```bash
gores init
```
 2. then generate a microservice 
```bash
gores generate myservice
```

### 2. Download pre-built binaries

You can download pre-compiled binaries from the [GitHub Releases page](https://github.com/your-username/go-microservice-boilerplate/releases).

Available binaries:

- `gores-linux-amd64` (Linux)  
- `gores-darwin-amd64` (macOS)  
- `gores-windows-amd64.exe` (Windows)  

**Usage example (Linux/macOS):**

```bash
chmod +x gores-linux-amd64
./gores-linux-amd64 generate myservice
```

**Usage example (Windows PowerShell):**

```Powershell
.\gores-windows-amd64.exe generate myservice
```

## Usage

```bash
gores generate [service-name] [port]
```

 - service-name: The name of the microservice to generate (required).
 - port: (Optional) port number. If omitted, the CLI automatically assigns the next available port starting from 8080.

---

## License

This project is licensed under the [Apache License 2.0](LICENSE).  
You are free to use, modify, and distribute this software — including for commercial purposes — under the terms of the Apache License 2.0.


---

## Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on how to report issues, request features, and submit pull requests.


---