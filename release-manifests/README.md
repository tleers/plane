# Downstream release manifests

Each directory records a locally built Plane backend image that is deployed without a container registry. A release is eligible for production only after its manifest, source range, architecture, test evidence, exported-image checksum, recovery point, and host acceptance receipt are complete.

The image archive is intentionally not committed. It is retained in the private release store and transferred directly to the target host. Do not add production environment files, database exports, uploads, or credentials to this repository.
