# migu_video Docker image

Automated multi-architecture Docker builds from [develop202/migu_video](https://github.com/develop202/migu_video). See the upstream project for usage, configuration and licensing information.

## Image

```text
ghcr.io/usirukesikam/migu_video:latest
```

Supported platforms: `linux/amd64`, `linux/arm64`, `linux/arm/v7`.

Example:

```yaml
services:
  migu_video:
    image: ghcr.io/usirukesikam/migu_video:latest
    container_name: migu_video
    ports:
      - "1234:1234"
    restart: unless-stopped
```

Keep existing environment variables and deployment settings when migrating an existing installation. Only use the service in accordance with upstream terms and applicable laws.

## Build policy

- Check upstream `main` and the `node:22-alpine` base image daily (03:37 UTC).
- Rebuild on runtime source, base image or workflow changes, or when the current `latest` digest differs from the recorded successful image.
- Ignore upstream documentation, CI metadata and generated playlists in the update fingerprint. Playlists remain in the build context for startup.
- Pin the checked upstream commit and base digest during the build. Review the workflow if the upstream Dockerfile changes its base image or layout.
- Push a candidate tag (`<run-number>-<run-attempt>`), test the service and M3U endpoint offline on every supported architecture, then promote the tested manifest to `latest`.
- Record successful publication in `versions.txt`. A failed check or smoke test leaves `latest` unchanged. If the state commit fails after promotion, the next check reconciles by rebuilding.
- Run the workflow manually with **force** enabled to rebuild even if inputs are unchanged (for example, to refresh Alpine packages installed during build).

The smoke tests verify startup and seeded playlist delivery. They do not verify external playback, account authentication or live data updates.

## Repository setup

The default branch is `main`. The workflow uses `GITHUB_TOKEN`; no extra repository secrets are required. Allow the workflow to publish the existing GHCR package and commit `versions.txt`. If the repository was recreated, grant the new repository Write access under the package's **Manage Actions access** setting. Retain the package's existing visibility. For public anonymous pulls, the package must be Public.

The first run builds all three platforms. Check that `latest` and `versions.txt` have been published successfully before deleting a previous repository or changing deployments.
