# Build and local-transfer procedure

Run these commands from the repository checkout at the `release/v1.3.1-pages-api.1` branch on an ARM64 machine:

```bash
docker build --platform linux/arm64 --file apps/api/Dockerfile.api --tag tleers/plane-backend:v1.3.1-pages-api.1 apps/api
docker run --rm tleers/plane-backend:v1.3.1-pages-api.1 python -m compileall -q plane/api plane/db
docker save tleers/plane-backend:v1.3.1-pages-api.1 | zstd --threads=0 --long=27 -o plane-backend-v1.3.1-pages-api.1-linux-arm64.tar.zst
shasum -a 256 plane-backend-v1.3.1-pages-api.1-linux-arm64.tar.zst
```

Before a production transfer, create a restorable Plane recovery set. Copy the archive and its SHA-256 to the host over SSH, verify the checksum there, load the archive, and verify the local image ID against the release manifest. Update only the Plane backend services (`api`, `worker`, `beat-worker`, and `migrator`); retain the upstream v1.3.1 image for rollback.
