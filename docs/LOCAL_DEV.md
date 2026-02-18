# Local Development Guide

This document describes how to build, run, test, and interact with the project in a local environment.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Build the project locally](#build-the-project-locally)
- [Run the service locally (without Docker)](#run-the-service-locally-without-docker)
- [Run with Docker Compose (local environment)](#run-with-docker-compose-local-environment)
- [Build a Docker image (local)](#build-a-docker-image-local)
- [Publish Docker image to a registry](#publish-docker-image-to-a-registry)
- [Run using a remote published Docker image](#run-using-a-remote-published-docker-image)
- 8. Run a specific unit test
- 9. Run a specific integration test for a specific environment
    - Local env
    - DEV env
    - STAGING env
    - [PROD env- 10. Send requests using curl
    - [GET example](#getT example
- 11. Health check
- 12. Logs
      ``

### Prerequisites

Install the following tools:

- **Java 21**  
  Check version:
  ```bash
  java -version
  ```

- **Maven 3.9+**  
  ```bash
  mvn -version
  ```

- **Docker** & **Docker Compose**  
  ```bash
  docker --version
  docker compose version
  ```

### Build the project locally

Full clean build:
```bash
mvn clean install
```

Build without running tests:
```bash
mvn clean install -DskipTests
```

### Run the service locally (without Docker)

Start Spring Boot application:
```bash
mvn spring-boot:run
```

Or run the built JAR:
```bash
java -jar target/my-service.jar
```

### Run with Docker Compose (local environment)

Start all required services including your Spring Boot service:
```bash
docker compose -f docker-compose.local.yml up --build
```

Stop all services:
```bash
docker compose -f docker-compose.local.yml down
```

Restart only your component:
```bash
docker compose -f docker-compose.local.yml up --build my-service
```

### Build a Docker image (local)

```bash
docker build -t my-service:local -f Dockerfile .
```

Run locally:
```bash
docker run -p 8080:8080 my-service:local
```

### Publish Docker image to a registry

Login:
```bash
docker login my-registry.example.com
```

Tag:
```bash
docker tag my-service:local my-registry.example.com/my-service:1.0.0
```

Push:
```bash
docker push my-registry.example.com/my-service:1.0.0
```

### Run using a remote published Docker image

```bash
docker run -p 8080:8080 my-registry.example.com/my-service:1.0.0
```

Or with Compose:
```bash
IMAGE_TAG=1.0.0 docker compose -f docker-compose.remote.yml up
```

## 8. Run a specific unit test

```bash
mvn -Dtest=com.example.SomeTest test
```

Run a specific test method:
```bash
mvn -Dtest=com.example.SomeTest#shouldDoWork test
```

## 9. Run a specific integration test for a specific environment

### Local env
```bash
mvn verify -Pint-tests -Denv=local
```

### DEV env
```bash
mvn verify -Pint-tests -Denv=dev
```

### STAGING env
```bash
mvn verify -Pint-tests -Denv=staging
```

### PROD env
```bash
mvn verify -Pint-tests -Denv=prod
```

Specific test class:
```bash
mvn verify -Pint-tests -Denv=local -Dtest=com.example.IntegrationTest
```

## 10. Send requests from local system using curl

### GET request example
```bash
curl -v   -X GET "http://localhost:8080/api/v1/items/123"
```

### PUT request example with JSON body
```bash
curl -v   -X PUT "http://localhost:8080/api/v1/items/123"   -H "Content-Type: application/json"   -d '{
    "name": "Updated item",
    "enabled": true,
    "value": 42
  }'
```

## 11. Health check

```bash
curl http://localhost:8080/actuator/health
```

## 12. Logs

Spring Boot logs:
```bash
tail -f target/logs/application.log
```

Docker logs:
```bash
docker logs -f my-service
```
