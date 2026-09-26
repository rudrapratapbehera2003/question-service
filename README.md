# Question Service

The **Question Service** is a core microservice responsible for managing quiz questions, categories, and options. It connects to a PostgreSQL database for data persistence and registers with the Eureka Registry Server.

## Available Features / Endpoints
- Create and manage questions
- Retrieve questions by category
- Internal question generation API (consumed via OpenFeign by the quiz-service)

## Procedure To Run The Application Locally

Take a pull from the repository and let the application resolve the dependencies on the basis of pom.xml file.

### Steps To Successfully Run The Application

1. **Ensure Service Registry is Running** (Must be up and running on port `8761` first).
2. **Configure PostgreSQL Database** and ensure the `question` database is active.
3. **Run the Spring Boot Application** locally via your IDE or using Maven (`mvn spring-boot:run`).

## Containerizing The Service (Using Docker)

1. **Ensure Shared Network Exists**
    ```bash
    docker network create microservices-net
    ```

2. **Run PostgreSQL Container**
    ```bash
    docker run -d \
    --name postgres-db \
    --network microservices-net \
    -p 5432:5432 \
    -e POSTGRES_DB=question \
    -e POSTGRES_USER=postgres \
    -e POSTGRES_PASSWORD=postgres \
    postgres:16-alpine
    ```

3. **Build & Run Question Service Container**
    ```bash
    docker build -t question-service:latest .

    docker run -d \
    --name question-service \
    --network microservices-net \
    -p 8081:8081 \
    --env-file .env \
    question-service:latest
    ```