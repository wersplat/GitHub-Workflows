# GitHub-Workflows

This repository contains reusable GitHub Actions workflows for CI/CD automation.

## Directory Structure

```
GitHub-Workflows/
├── .github/
│   └── workflows/
│       └── doppler-docker
└── README.md
```

## Doppler Docker CI/CD Workflow

The `doppler-docker` workflow builds and releases a Docker image for a Doppler project. It is designed to be called as a reusable workflow from other repositories.

### Trigger

- `workflow_call`: This workflow is intended to be invoked by other workflows using `uses:`.

### Inputs
- `config` (string, default: `prod`): The Doppler config to use.

### Required Secrets
- `doppler-token`: (required) Doppler token for authentication.
- `sentry-auth-token`: (optional) Sentry auth token for release tracking.
- `sentry-org`: (optional) Sentry organization.
- `sentry-project`: (optional) Sentry project name.

### What it does
- Checks out the code.
- Installs the Doppler CLI.
- Sets up Doppler with the provided token and config.
- Exports Doppler secrets to the environment.
- (If Sentry secrets are provided) Creates and finalizes a Sentry release.
- Builds a Docker image tagged with the current commit SHA.

### Usage Example

To use this workflow in your repository:

```yaml
jobs:
  doppler-docker:
    uses: wersplat/GitHub-Workflows/.github/workflows/doppler-docker@main
    with:
      config: "prod"
    secrets:
      doppler-token: ${{ secrets.DOPPLER_TOKEN }}
      sentry-auth-token: ${{ secrets.SENTRY_AUTH_TOKEN }}
      sentry-org: ${{ secrets.SENTRY_ORG }}
      sentry-project: ${{ secrets.SENTRY_PROJECT }}
```

- Replace the `uses:` path with the correct repo and branch/tag if needed.
- Only `doppler-token` is required; Sentry secrets are optional and only needed if you use Sentry release tracking.

---

For more details, see the workflow file at `.github/workflows/doppler-docker`.