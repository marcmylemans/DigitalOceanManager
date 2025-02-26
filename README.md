# Digital Ocean Manager

This project is a multi-tenant web application for managing and deploying VPS instances using DigitalOcean/Hetzner. It includes the following services running in Docker containers:

PostgreSQL (db): Stores application data.
Web: The Flask web application.
Celery: Background worker for long-running tasks.
Redis: Message broker and caching.
Nginx: Reverse proxy for the web service.
Flower: Web UI for monitoring Celery tasks.
All images are built and stored on GitHub Container Registry (GHCR).

## Prerequisites

Docker (https://docs.docker.com/get-docker/)

Docker Compose (https://docs.docker.com/compose/install/)

## Setup Instructions

Clone the Repository:

```bash
https://github.com/marcmylemans/DigitalOceanManager.git
cd your-repository
```

## Configure Environment Variables:

Edit the docker-compose.yml file to update environment variables as needed. For example, update the following:

SECRET_KEY
DATABASE_URL (for the web and celery services)
ENCRYPTION_KEY
CELERY_BROKER_URL and CELERY_RESULT_BACKEND
The provided sample uses these default values for production:

```
diff
- SECRET_KEY=your_prod_secret
- DATABASE_URL=postgresql://youruser:yourpassword@db:5432/multi_tenant_db
- ENCRYPTION_KEY=DaW0qjrXFChaGvzGF5kA-cUhJK3CMlwe3ll7sVhV4d4=
- CELERY_BROKER_URL=redis://redis:6379/0
- CELERY_RESULT_BACKEND=redis://redis:6379/0
```

## Run Docker Compose:

Use the following command to build (if necessary) and run all services in detached mode:

```
docker-compose up -d
```

## Verify the Running Containers:

```
docker-compose ps
```

You should see containers for db, web, celery, redis, nginx, and flower.

## Accessing the Application

### Web Application:

Open your browser and go to http://localhost:5000 (the web container runs on port 5000).

#### Nginx Reverse Proxy:

The Nginx container listens on port 80. Access the application via http://localhost.

### Celery Flower Dashboard:

Access Flower for monitoring tasks at http://localhost:5555.

### PostgreSQL Database:

The PostgreSQL container listens on port 5432. You can connect using the credentials specified in the environment variables.

## Data Persistence

### PostgreSQL Data:

The postgres_data volume is used to persist your PostgreSQL data. This ensures that your database contents survive container restarts.

## Stopping the Application

To stop and remove all containers (without deleting the persistent volume), run:

```
docker-compose down
```

To remove containers and the persistent volume (this will delete your PostgreSQL data), run:

```
docker-compose down -v
```

# Additional Notes

## Production Considerations:

For production deployments, consider using a managed PostgreSQL service, securing your environment variables (using Docker secrets or similar), and using a reverse proxy with SSL termination.

# Troubleshooting:

If you encounter errors related to database migrations, ensure you run your migration commands or initialize the database as needed.
Check container logs with docker-compose logs [service_name] for more details.

# License
MIT
