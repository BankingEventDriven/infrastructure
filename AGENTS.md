# AGENTS.md

## Project Overview
`infrastructure` owns local and shared operational assets for the microservices system. It provisions the supporting runtime pieces needed to develop and run the services together, especially PostgreSQL, Apache Pulsar, and Docker Compose wiring.

Its role is environment orchestration, not application logic.

## Responsibilities
- Own Docker Compose definitions, local environment variables, and support scripts.
- Provide reproducible local startup for `api-gateway`, `account-service`, and `transaction-service`.
- Hold infrastructure-specific assets for PostgreSQL and Pulsar setup.

Must NOT:
- Contain domain logic, service business rules, or shared Java code.
- Become the place where services communicate by bypassing their contracts.
- Store service-specific logic that should live in each repo.

## Tech Stack
- Docker and Docker Compose as the primary runtime orchestrator
- PostgreSQL container for service persistence
- Apache Pulsar container for async communication
- No Spring Boot application in this repo

## Architecture Notes
This repo maps to the operational boundary of the system rather than a business bounded context.

Expected structure:
- `docker-compose.yml`: top-level local orchestration
- `postgresql/`: DB init scripts, seed data, or config overrides
- `pulsar/`: topics, namespace setup, or broker config assets
- `scripts/`: helper scripts for startup, teardown, health checks, and bootstrap tasks
- `.env`: local environment values consumed by Compose/scripts

Keep infrastructure concerns separate from service code. If a change only affects one service's domain behavior, it does not belong here.

## Communication
Produces:
- No business events; this repo provisions the Pulsar runtime used by other services

Consumes:
- No business events

Supports interactions between:
- `api-gateway`
- `transaction-service`
- `account-service`
- Local PostgreSQL and Pulsar containers

## Run Instructions
Local:
- `docker compose up --build`

Targeted startup:
- `docker compose up postgres pulsar`

Environment dependencies:
- Docker Desktop / Docker Engine
- Values from `.env` when Compose is expanded to use them
- Service images or Dockerfiles for each application repo

## Coding Guidelines
- Keep operational config declarative and easy to reason about.
- Prefer scripts for repeatable setup tasks instead of manual steps.
- Do not encode service business rules in container startup logic.
- Keep service-to-service communication event-driven; infrastructure should only enable it.

## Constraints
- Do NOT introduce infrastructure shortcuts that couple services tightly.
- Do NOT centralize application logic in scripts or Compose overrides.
- Do NOT assume shared database access between services, even in local dev.
- Respect bounded contexts even when running everything on one machine.

## Folder Structure Explanation
- `docker-compose.yml`: local system composition for services and dependencies
- `.env`: Compose variables and local defaults
- `postgresql/`: reserved for DB bootstrap/config assets
- `pulsar/`: reserved for broker/topic bootstrap assets
- `scripts/`: helper automation for environment setup and maintenance
- `README.md`: human-oriented environment notes

Current state:
- Compose currently provisions `postgres`, `pulsar`, `account-service`, `transaction-service`, and `api-gateway`.
- The `postgresql/`, `pulsar/`, and `scripts/` folders are present but not yet populated; keep future infra assets there instead of scattering them across service repos.
