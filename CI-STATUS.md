# CI and Build Status

## Overview

This fork has been modernized with the following changes:

### CI/CD Status

✅ **Continuous Integration**: Test and lint workflows are active and green
❌ **Publishing**: All publishing/releasing workflows are disabled by design

### Workflows

#### Active (Testing/Validation)
- **test.yml**: Runs tests across Node.js 20.x and 22.x with multiple database backends
- **lint.yml**: Runs ESLint and Prettier checks
- **docker-ci-tags.yml**: Builds Docker images for validation (push disabled)

#### Disabled (Publishing)
- **main.yml**: GitHub releases workflow (disabled - requires manual trigger, job has `if: false`)

### Known Security Considerations

#### xlsx Dependency
- **Issue**: Using `xlsx@0.18.5` from npm registry (has known vulnerabilities)
- **Reason**: Newer versions (0.20+) only available from SheetJS CDN (blocked in CI environment)
- **Impact**: Affects Excel export functionality only
- **Mitigation**: 
  - Functionality is non-critical (export feature)
  - Vulnerabilities are Prototype Pollution and ReDoS
  - No fix available on npm registry
  - Using node-xlsx@0.22.0 (latest compatible with npm xlsx)

#### Dependency Updates
- Automated via Dependabot (configured in `.github/dependabot.yml`)
- Only creates PRs - no automatic publishing
- Security updates prioritized
- Major version updates disabled to minimize breaking changes

### Build Requirements

- Node.js 20 or higher (20.x, 22.x tested)
- Yarn 1.22.19+
- Docker (for integration tests with databases)

### Running Locally

```bash
# Install dependencies
yarn
(cd client && yarn)
(cd server && yarn)

# Build
./scripts/build.sh

# Lint
yarn lint

# Test (basic)
cd server && npm test

# Test with specific backend
cd server && npm run testpostgres  # requires docker
```

### CI Safety Guarantees

All publishing is disabled:
- ✅ No GitHub releases created automatically
- ✅ No Docker images pushed to registries
- ✅ No npm packages published
- ✅ No artifact uploads to external services
- ✅ Test artifacts are internal to CI runs only

### Last Updated
2025-10-21
