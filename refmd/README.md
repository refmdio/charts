# RefMD Helm Chart

This chart deploys the RefMD collaborative markdown platform with a high-availability layout covering the API, web application, Redis, MinIO object storage, and an optional bundled PostgreSQL database.

## Installing the Chart

```bash
helm repo add refmd https://refmdio.github.io/charts
helm install my-refmd refmd/refmd \
  --set api.secrets.JWT_SECRET=your-secret \
  --set api.secrets.ENCRYPTION_KEY=another-secret \
  --set api.secrets.PLUGIN_ASSET_SIGN_KEY=plugin-sign-secret
```

## Configuration Highlights

- `app.*`: Controls the frontend deployment, service, and ingress settings.
- `api.*`: Configures the backend API, including clustering and storage integration settings. Persistent volumes for the API are disabled by default because the HA profile stores uploads in MinIO/S3. Enable the PVCs only when using the filesystem storage backend. Use `api.extraEnv` / `api.extraEnvFrom` to mount additional secrets or config maps (for example, supplying an external `DATABASE_URL`).
- `*.extraVolumes` / `*.extraVolumeMounts`: Attach additional secrets or configmaps (for instance, a custom CA bundle mounted alongside `NODE_EXTRA_CA_CERTS`).
- `postgres.*`: Enables the bundled PostgreSQL database or allows pointing to an external instance.
- `redis.*`: Manages the Redis instance used for realtime coordination and task queues.
- `minio.*`: Configures the MinIO object storage deployment.
- `minioSetup.*`: Controls the post-install job that provisions buckets and access policies.

See `values.yaml` for the full list of configurable parameters.

## Example Values

Sample overlays are available under `examples/`:

- `values-local-minimal.yaml`: Single-node stack with filesystem uploads and no Redis/MinIO dependencies.
- `values-single-s3.yaml`: Single API replica backed by MinIO/S3 for uploads.
- `values-ha-s3.yaml`: HA profile with two API replicas, Redis coordination, and MinIO/S3 storage.

Install with:

```bash
helm upgrade --install refmd charts/refmd \
  --namespace refmd --create-namespace \
  -f charts/refmd/examples/values-ha-s3.yaml
```

To point the API at an external database managed outside the chart, mount a secret that contains the connection string:

```yaml
api:
  extraEnvFrom:
    - secretRef:
        name: refmd-config-secret
```

Combine this with `postgres.enabled=false` and `api.extraEnv`/`api.env` overrides as needed.
