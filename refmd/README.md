# RefMD Helm Chart

This chart deploys the RefMD collaborative markdown platform with a high-availability layout covering the API, web application, Redis, MinIO object storage, and an optional bundled PostgreSQL database.

## Installing the Chart

```bash
helm repo add refmd https://refmdio.github.io/charts
helm install my-refmd refmd/refmd \
  --set api.secrets.JWT_SECRET=your-secret \
  --set api.secrets.ENCRYPTION_KEY=another-secret
```

## Configuration Highlights

- `app.*`: Controls the frontend deployment, service, and ingress settings.
- `api.*`: Configures the backend API, including clustering and storage integration settings.
- `postgres.*`: Enables the bundled PostgreSQL database or allows pointing to an external instance.
- `redis.*`: Manages the Redis instance used for realtime coordination and task queues.
- `minio.*`: Configures the MinIO object storage deployment.
- `minioSetup.*`: Controls the post-install job that provisions buckets and access policies.

See `values.yaml` for the full list of configurable parameters.
