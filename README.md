# coverage-dashboard

A GitHub Actions-based dashboard for tracking Go code coverage across multiple Tekton and OpenShift Pipelines repositories.

## Overview

This repository automatically generates and publishes code coverage reports for configured repositories. Coverage data is collected daily and published to GitHub Pages.

## Features

- Automated daily coverage collection via GitHub Actions
- Fetches coverage from upstream workflow artifacts when available
- Falls back to running tests locally if upstream coverage unavailable
- Generates HTML coverage reports for detailed analysis
- Tracks coverage history over time
- Supports custom package exclusions per repository

## Configuration

Repositories are configured in `repos.yaml`:

```yaml
repositories:
  - name: tektoncd/pipeline
    upstream_coverage_workflow: go-coverage.yml  # Optional: fetch from upstream artifact
    exclude_dirs:
      - cmd/
      - vendor/
      - test/
    exclude_files:
      - zz_generated.deepcopy.go
```

## How It Works

1. Workflow runs daily (or on-demand via workflow_dispatch)
2. For each configured repository:
   - If `upstream_coverage_workflow` is set, attempts to download coverage artifact from the latest successful run
   - If upstream coverage unavailable, clones the repository and runs tests locally
   - Applies configured exclusions
   - Generates coverage percentage and HTML report
3. Results are published to the `gh-pages` branch

## Viewing Results

Coverage data is available at: `https://<username>.github.io/<repo-name>/coverage.json`

Individual repository coverage reports: `https://<username>.github.io/<repo-name>/coverage/<org>/<repo>/`

## License

See [LICENSE](LICENSE) for details.
