# general-workflows

A repository for shared, reusable GitHub Actions workflows.

## Reusable Workflows

### 1. Shared Obsidian Plugin CI (`plugin-ci.yml`)

This workflow provides a standardized CI/CD pipeline for Obsidian plugins, including:
- Change detection (filtering)
- Linting (ESLint)
- Testing (Vitest with coverage)
- Optional SonarQube analysis
- Release artifact packaging (Zip)
- Automated releases (Semantic Release)

#### Usage

In your Obsidian plugin repository, create `.github/workflows/ci.yml` with the following content:

```yaml
name: CI/CD

on:
  push:
    branches: [main, master, '**']
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  workflow-call:
    uses: <your-username>/general-workflows/.github/workflows/plugin-ci.yml@main
    with:
      plugin-id: "your-plugin-id"
      skip-sonar: false  # Set to true if you don't use SonarQube
    secrets:
      RELEASE_TOKEN: ${{ secrets.RELEASE_TOKEN }}
      SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
```

#### Inputs

| Input | Description | Default | Required |
|---|---|---|---|
| `plugin-id` | The unique ID of the plugin (used for zip naming) | N/A | Yes |
| `node-version` | Node.js version to use | `22` | No |
| `skip-sonar` | Whether to skip SonarQube analysis | `true` | No |

#### Secrets

| Secret | Description |
|---|---|
| `RELEASE_TOKEN` | GitHub Token for semantic-release (needs `write` permissions) |
| `SONAR_TOKEN` | Token for SonarCloud/SonarQube analysis |
