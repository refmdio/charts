# RefMD Helm Chart

This chart deploys the RefMD collaborative markdown platform comprised of the API, web application, and an optional bundled PostgreSQL database.

## Installing the Chart

```bash
helm repo add refmd https://refmdio.github.io/charts
helm install my-refmd refmd/refmd \
  --set api.secrets.JWT_SECRET=your-secret \
  --set api.secrets.ENCRYPTION_KEY=another-secret
```

## Configuration Highlights

- `app.*`: Controls the frontend deployment, service, and ingress settings.
- `api.*`: Configures the backend API, including environment variables and persistent volumes.
- `postgres.*`: Enables the bundled PostgreSQL database or allows pointing to an external instance.

See `values.yaml` for the full list of configurable parameters.
