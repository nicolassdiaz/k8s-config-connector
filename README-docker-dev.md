# Docker Development Environment for macOS

This provides a Linux-based development environment to avoid macOS compatibility issues (BSD vs GNU tools).

## Setup

1. **Copy files to project root:**
   ```bash
   cp Dockerfile.dev ~/nicolassdiaz/k8s-config-connector/
   cp docker-compose.dev.yml ~/nicolassdiaz/k8s-config-connector/
   ```

2. **Build and start the container:**
   ```bash
   cd ~/nicolassdiaz/k8s-config-connector
   docker-compose -f docker-compose.dev.yml build
   docker-compose -f docker-compose.dev.yml run --rm kcc-dev
   ```

3. **Inside the container, you can now run:**
   ```bash
   # Generate code (uses GNU sed, works correctly)
   make generate

   # Run linting
   make lint

   # Run ready-pr checks
   make ready-pr

   # Run E2E tests
   hack/compare-mock pkg/test/resourcefixture/testdata/basic/run/v1alpha1/runworkerpool/runworkerpool-minimal/
   ```

## Benefits

- ✅ Uses GNU sed and other GNU tools (Linux-compatible)
- ✅ Consistent environment across developers
- ✅ Persists Go module and build caches (faster rebuilds)
- ✅ All changes reflected immediately (volume mount)
- ✅ No need to modify macOS system

## Alternative: Run commands directly

Instead of an interactive shell, run specific commands:

```bash
# Run make generate in container
docker-compose -f docker-compose.dev.yml run --rm kcc-dev make generate

# Run make ready-pr in container
docker-compose -f docker-compose.dev.yml run --rm kcc-dev make ready-pr

# Run E2E tests in container
docker-compose -f docker-compose.dev.yml run --rm kcc-dev hack/compare-mock [test-path]
```
