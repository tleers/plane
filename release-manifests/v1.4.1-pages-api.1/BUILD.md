# Build and promotion procedure

Build the backend on an ARM64 machine from this branch:

```sh
docker build --platform linux/arm64 --file apps/api/Dockerfile.api --tag tleers/plane-backend:v1.4.1-pages-api.1 apps/api
docker compose -f docker-compose-test.yml run --rm --build api-tests pytest plane/tests/contract/api/test_pages.py -vv
```

Export the resulting local image to the private release store, checksum it, and transfer it directly to Hetzner. Do not publish it to a public registry.

Before production promotion, create and validate an encrypted recovery set, import the archive on the host, retain the prior v1.3.1 Pages image, review the v1.4.1 migration plan against a restored copy, and make the Compose change with `--env-file plane.env`. The deployment changes all four backend services (`migrator`, `api`, `worker`, and `beat-worker`) to the new image, runs the release migration path once, then accepts public TLS, instance API, login, authenticated Pages API, project persistence, container restarts, and sibling routes. On any failure, contain writes and use the recorded version-aware rollback procedure; do not assume older containers are compatible after migrations.
