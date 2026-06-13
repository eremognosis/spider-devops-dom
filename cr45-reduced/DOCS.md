# REPORT
## Approach to Deployment Setup and Challenges

### The Approach

The application uses a **containerized multi-stage build architecture** managed by Docker Compose. This ensures environment consistency across development and production while keeping final production images as lightweight as possible.

* **Frontend:** Built using a Node.js 20 environment, compiled into static assets, and served via a lightweight Nginx Alpine server.
* **Backend:** Built using Go 1.26, compiled into a single statically-linked binary, and run inside a minimal Alpine Linux image.
* **Database:** A standard PostgreSQL 17 database instance with data persistence and automated health checks.


## How to run

Ensure docker is installed
```bash
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
verify with 
```bash
sudo docker run hello-world
```
To run without `sudo`
```bash
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
```

### Build the stack
```bash
docker compose up --build -d
```
### Verify
```bash
docker compose ps
```


##  Service breakdown and Port Mapping

| Service Name | Container Name | Host Port | Container Port | Purpose |
| --- | --- | --- | --- | --- |
| **`db`** | `cr45_reduced` | `5432` | `5432` | Relational database holding application data. |
| **`backend`** | `backend` | `8080` | `8080` | Go REST API handling business logic and DB communication. |
| **`frontend`** | `frontend` | `80` | `80` | Nginx web server serving the compiled frontend SPA static files. |

---

##  How the Frontend Reaches the Backend

Because the frontend is a **Single Page Application (SPA)** compiled into static HTML/JS/CSS assets, it runs inside the *user's browser*, not inside the Docker network.

1. **The Client Request:** A user opens `http://localhost` in their browser. Nginx serves the static files.
2. **API Communication:** The frontend code executing in the browser makes HTTP requests to the backend API.
3. **Network Boundary crossing:** Since the browser sits outside Docker, the frontend must point to the **host's** address: `http://localhost:8080`.

##  How PostgreSQL is Configured

The database is configured via environment variables in the `compose.yaml` file to establish an automated, persistent environment:

* **Credentials:**
* **User:** `postgres`
* **Password:** `classrep123`
* **Database Name:** `cr45_reduced`


* **Persistence:** A named Docker volume `pgdata` is mapped to `/var/lib/postgresql/data`. This ensures that data survives container restarts or removals.
* **Health Validation:** The container uses `pg_isready` to ping the database internally every 5 seconds. Other services (like the backend) rely on this health status before trying to connect.

## How Migrations are Handled

Migrations are partially set up via the Docker structural layout, relying on application-level execution:

**Asset Delivery:** In the `backend/Dockerfile`, the line `COPY --from=builder /app/migrations /migrations` copies SQL migration files from source code into the final minimal runner image.

## Common Failure Cases and How to Debug Them

### Backend Fails to Start due to Database Unavailability

* **Symptom:** Backend container keeps restarting or crashes immediately.
* **Cause:** The database takes longer to initialize and accept connections than the backend's internal timeout allowance.
* **Fix/Debug:** Verify the DB health using `docker compose ps`. Check backend logs using:
```bash
docker compose logs backend

```


### Database Authentication Failure

* **Symptom:** Backend logs show `password authentication failed for user "postgres"`.
* **Cause:** Changing credentials in `compose.yaml` *after* the database volume has already been initialized once. Postgres ignores new env variables if a data directory already exists.
* **Fix/Debug:** Wipe the stale volume and restart (Warning: this deletes DB data):
```bash
docker compose down -v
docker compose up -d
