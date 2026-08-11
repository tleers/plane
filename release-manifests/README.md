# Release manifests

Each directory in this tree identifies one deployable downstream Plane backend release. A manifest records the exact source commit, upstream base, target platform, test evidence, local image tag, and delivery options.

`local-transfer` is the default delivery method for a single private server: build the image on an ARM64 workstation, export it as a compressed OCI/Docker image archive, verify its SHA-256 after transfer, load it on the host, and verify the local image ID before recreating the application containers. It requires no image registry and no production registry credential.

For several hosts or automated rollouts, retain the same manifest and use either `private-registry` (a private GHCR, GitLab, ECR, or other OCI registry) or `public-registry` delivery. The release identity is the source commit plus platform and image ID; a mutable tag alone is never sufficient evidence.

Never store credentials, database data, or user content in this tree. The exported image archive is a build artifact and must not be committed to Git.
