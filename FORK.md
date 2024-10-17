# Fork Information

## Releasing Steps

1. Take a copy of the upstream `knative-vX.Y.Z` tag and create a new __branch__ on this repo called `fork-X.Y.Z`
2. Make any change on the `fork-X.Y.Z` branch, prepending the commit message with `FORK: `
3. Use the GitHub interface to create a tag and release called `vX.Y.Z-deploykf.N`, where `N` is an incremental number starting from `0`
4. On a local clone of that tag, run the following commands to build and push the images:

```bash
#
# WARNING: running this command will immediately push the images to the registry.
#          update the `--tags` flag, and ensure you have checked out the correct commit
#
export KO_DOCKER_REPO="ghcr.io/deploykf/knative-serving"
ko resolve \
  --platform "linux/arm64,linux/amd64" \
  --sbom "none" \
  --tags "X.Y.Z-deploykf.N" \
  --base-import-paths \
  -Rf config/core/ \
> /dev/null
```