# Deployment

## Render

The root `render.yaml` defines an API web service and independent outbox worker. Review the blueprint, then configure the referenced secrets in Render.

Configure environment variables in Render's secret settings. Run `alembic upgrade head` as a release/pre-deploy command, then run `python -m scripts.seed_data` once. Create the first admin interactively using `python -m scripts.create_admin`; do not store its password in service environment.

## Neon

The Neon CLI writes `DATABASE_URL` (pooled) and `DATABASE_URL_UNPOOLED` (direct) to `.env.local`. The application loads `.env.local` as well as `.env`; keep both files out of version control. API traffic uses the pooled URL, while Alembic automatically uses the direct URL for migrations. Use a separate Neon branch or database for migration testing before applying changes to production.

## Kubernetes

Review the example ConfigMap and Secret, then create the actual secret out-of-band (external secret manager or `kubectl create secret`). Replace the image placeholder, apply ConfigMap/Secret, Deployment, Service, and worker Deployment. Run migrations as a one-off deployment/job before rolling out application replicas. PostgreSQL and Kafka are external managed services.

## Local Compose

From `backend`, copy `.env.example` to `.env`, then run `docker compose up --build`; apply migrations and seed through the Compose API service with `docker compose run --rm api python -m alembic upgrade head` and `docker compose run --rm api python -m scripts.seed_data`. Optional `.env.local` settings override `.env` in the Compose services. Stop with `docker compose down`; volumes persist. Local-only credentials in Compose are not production credentials.
