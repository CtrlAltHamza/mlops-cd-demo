# MLOps Continuous Delivery Demo

MLOps class activity demonstrating Continuous Delivery for an ML inference application using:

- Flask
- Pytest
- Docker
- GitHub Actions
- GitHub Container Registry
- Staging and Production environments
- Approval gates
- Versioned releases
- Rollback

## Local development

Run the tests with Docker:

```bash
docker compose run --build --rm mlops-api python -m pytest
```

Start the API locally:

```bash
docker compose up --build
```

Then check `http://localhost:5000/health`.

## Release pipeline

The CD workflow runs only for semantic-version tags such as `v1.0.0`. It tests the application, builds one image, publishes the versioned image and `latest` to GHCR, deploys the versioned image to `staging`, and runs a health check before waiting for approval in the `production` environment.

Configure these secrets in the matching GitHub Environments:

- `STAGING_HOST`, `STAGING_USER`, `STAGING_SSH_KEY`
- `PRODUCTION_HOST`, `PRODUCTION_USER`, `PRODUCTION_SSH_KEY`

After configuring the environments and SSH access, create a release with:

```bash
git tag v1.0.0
git push origin v1.0.0
```

Rollback by deploying the previously published immutable image tag, for example `ghcr.io/ctrlalthamza/mlops-cd-demo:1.0.0`.
