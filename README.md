# Task Manager

Simple task manager demo app. Modules:

- `backend/` — Spring Boot 3 (Kotlin, JDK 21) REST API exposing `/api/tasks`. H2 in-memory by default, PostgreSQL via `postgres` profile.
- `frontend/` — React 19 + Vite + TypeScript SPA. Calls backend via `VITE_API_URL`.
- `deploy/my-simple-app/` — Helm chart deploying both services to Kubernetes.
- `docker-compose.yml` / `docker-compose-postgres.yml` — local stack.

## Building the modules

### Backend

```bash
docker build -t <your-registry>.azurecr.io/lab04/backend:<VERSION> --platform linux/amd64 ./backend
docker push <your-registry>.azurecr.io/lab04/backend:<VERSION>
```

### Frontend

```bash
docker build -t <your-registry>.azurecr.io/lab04/frontend:<VERSION> --platform linux/amd64 ./frontend
docker push <your-registry>.azurecr.io/lab04/frontend:<VERSION>
```

## Running with Docker Compose

### H2 (in-memory, default)

No database setup needed. Data resets on container restart.

```bash
docker-compose up --build
```

- Frontend: http://localhost
- Backend: http://localhost:8080

### PostgreSQL (persistent)

Uses a postgres container with a persistent volume.

```bash
docker-compose -f docker-compose.yml -f docker-compose-postgres.yml up --build
```

- Frontend: http://localhost
- Backend: http://localhost:8080
- Postgres: localhost:5432

To stop and remove volumes:

```bash
docker-compose -f docker-compose.yml -f docker-compose-postgres.yml down -v
```
